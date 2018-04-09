---
title: Globalizace a lokalizace v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak ASP.NET Core poskytuje služby a middleware pro lokalizaci obsahu do různých jazyků a kultur.
manager: wpickett
ms.author: riande
ms.date: 01/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/localization
ms.openlocfilehash: 3ae73cb40b4db492883f302aeb559b9606aa8ee7
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="globalization-and-localization-in-aspnet-core"></a>Globalizace a lokalizace v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien překážka](https://twitter.com/damien_bod), [ADAM Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), a [Ateya Hisham Koš](https://twitter.com/hishambinateya)

Vytvoření vícejazyčné webu pomocí ASP.NET Core vám umožní vaší lokality k dosažení širší cílovou skupinu. Základní technologie ASP.NET poskytuje služby a middleware pro lokalizace do různých jazyků a kultur.

Internacionalizace zahrnuje [globalizace](https://docs.microsoft.com/dotnet/api/system.globalization) a [lokalizace](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization). Globalizace je proces návrhu aplikace, které podporují různé jazykové verze. Globalizace přidává podporu pro vstup, zobrazení a výstupní sada skripty jazyka, které se vztahují k určité geografické oblasti.

Lokalizace je proces přizpůsobení globalizovaná aplikaci, která již byl zpracován lokalizovatelnosti na konkrétní jazykové verze nebo národní prostředí. Další informace najdete v části **globalizace a lokalizace podmínky** téměř na konci tohoto dokumentu.

Lokalizace aplikací zahrnuje následující:

1. Aby lokalizovatelný obsah aplikace

2. Zadejte lokalizované prostředky pro jazyků a kultur, které podporujete

3. Implementace strategie pro vyberte jazyk nebo jazykovou verzi pro každý požadavek

## <a name="make-the-apps-content-localizable"></a>Aby lokalizovatelný obsah aplikace

Byla zavedená v ASP.NET Core `IStringLocalizer` a `IStringLocalizer<T>` byly navržen k zvýšení produktivity při vývoji lokalizované aplikace. `IStringLocalizer` používá [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) a [ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader) k poskytnutí prostředků specifické pro jazykovou verzi za běhu. Jednoduché rozhraní má indexer a `IEnumerable` pro vrácení lokalizovaných řetězců. `IStringLocalizer` nevyžaduje ukládání řetězce výchozí jazyk v souboru prostředků. Můžete vyvíjet aplikace cílené na lokalizace a není nutné vytvářet soubory prostředků v rané fázi vývoj. Následující kód ukazuje, jak zabalit řetězec "Title o" pro lokalizaci.

[!code-csharp[](localization/sample/Localization/Controllers/AboutController.cs)]

Ve výše, kódu `IStringLocalizer<T>` implementace pochází z [vkládání závislostí](dependency-injection.md). Pokud lokalizované hodnoty "Title o" nebyl nalezen, pak klíč indexeru je vrácen, tedy řetězec "Title o". Můžete ponechat výchozí nastavení literálu řetězce jazyků v aplikaci a zabalení v lokalizátora, tak, aby se mohli zaměřit na vývoj aplikace. Vývoj aplikace s výchozí jazyk a jeho přípravu pro lokalizaci bez vytvoření první výchozí soubor prostředků. Alternativně můžete použít tradiční přístup a zadejte klíč k získání řetězce výchozí jazyk. Pro mnoho vývojáře nový pracovní postup nemá výchozí jazyk *RESX* souborové služby a jednoduše zabalení textové literály můžete snížit režii lokalizace aplikace. Jinými vývojáři bude upřednostňovat tradiční pracovní postup, jak ho můžete bylo snazší práce s delší textové literály a usnadňují aktualizovat lokalizované řetězce.

Použití `IHtmlLocalizer<T>` implementace pro prostředky, které obsahují HTML. `IHtmlLocalizer` Argumenty, které jsou ve formátu v řetězec prostředku kóduje HTML, ale nepodporuje kódování HTML řetězec prostředku sám sebe. V ukázce zvýrazněná níže pouze hodnota `name` parametr není kódován jazykem HTML.

[!code-csharp[](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

**Poznámka:** chcete obecně pouze lokalizovat text a není ve formátu HTML.

Na nejnižší úrovni, můžete získat `IStringLocalizerFactory` mimo [vkládání závislostí](dependency-injection.md):

[!code-csharp[](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

Výše uvedený kód ukazuje každé dvě objektu pro vytváření vytvořit metody.

Můžete oddílu lokalizovaných řetězců řadič, oblasti nebo mít jenom jeden kontejner. V ukázkové aplikace s názvem třídu fiktivní `SharedResource` se používá pro sdílené prostředky.

[!code-csharp[](localization/sample/Localization/Resources/SharedResource.cs)]

Někteří vývojáři použít `Startup` třídy tak, aby obsahovala globální nebo sdíleného řetězce. V ukázce níže `InfoController` a `SharedResource` překladatelům při lokalizaci používají:

[!code-csharp[](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>Lokalizace zobrazení

`IViewLocalizer` Služba poskytuje lokalizované řetězce pro [zobrazení](https://docs.microsoft.com/aspnet/core). `ViewLocalizer` Třída implementuje toto rozhraní a vyhledá umístění prostředků z cesty k souboru zobrazení. Následující kód ukazuje, jak používat výchozí implementaci `IViewLocalizer`:

[!code-cshtml[](localization/sample/Localization/Views/Home/About.cshtml)]

Výchozí implementaci `IViewLocalizer` vyhledá soubor prostředků na základě názvu souboru zobrazení. Neexistuje žádná možnost použít soubor globální sdílený prostředek. `ViewLocalizer` implementuje lokalizátora pomocí `IHtmlLocalizer`, takže není HTML Razor kódování lokalizovaný řetězec. Můžete parametrizovat řetězce prostředků a `IViewLocalizer` se použije kódování HTML parametry, ale není řetězec prostředku. Vezměte v úvahu následující syntaxe Razor kód:

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

Soubor francouzštině prostředků může obsahovat následující:

| Key | Hodnota |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

Vykreslené zobrazení bude obsahovat kód HTML ze zdrojového souboru.

**Poznámka:** chcete obecně pouze lokalizovat text a není ve formátu HTML.

Pokud chcete používat soubor sdílený prostředek v zobrazení, Vložit `IHtmlLocalizer<T>`:

[!code-cshtml[](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>Lokalizace DataAnnotations

Lokalizace DataAnnotations chybové zprávy s `IStringLocalizer<T>`. Pomocí možnosti `ResourcesPath = "Resources"`, chyba zprávy v `RegisterViewModel` mohou být uloženy v některém z následujících cestách:

* *Resources/ViewModels.Account.RegisterViewModel.fr.resx*
* *Resources/ViewModels/Account/RegisterViewModel.fr.resx*

[!code-csharp[](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

Lokalizace v ASP.NET MVC základní 1.1.0 a vyšší, bez ověření atributy. Jádro ASP.NET MVC 1,0 nemá **není** vyhledávání lokalizovaných řetězců pro atributy bez ověření.

<a name="one-resource-string-multiple-classes"></a>
### <a name="using-one-resource-string-for-multiple-classes"></a>Pomocí jednoho řetězec prostředku pro několik tříd

Následující kód ukazuje, jak používat jeden řetězec prostředku pro ověření atributy s více tříd:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

V kódu, který předchází `SharedResource` je třída odpovídající resx, kde jsou uložené ověřovacích zpráv. Při tomto postupu se bude používat jenom DataAnnotations `SharedResource`, namísto prostředků pro každou třídu.

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>Zadejte lokalizované prostředky pro jazyků a kultur, které podporujete

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures a SupportedUICultures

ASP.NET Core můžete zadat hodnoty dvou jazykové verze, `SupportedCultures` a `SupportedUICultures`. [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) objekt pro `SupportedCultures` určuje výsledky funkcí závislých na jazykové verzi, jako je například datum, čas, číslo a formátování měny. `SupportedCultures` také určuje pořadí řazení textu, konvence velká a malá písmena a porovnání řetězců. V tématu [CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) Další informace o tom, jak server získá jazykovou verzi. `SupportedUICultures` Určuje, který překládá řetězce (z *RESX* soubory) jsou vyhledávat pomocí [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager). `ResourceManager` Jednoduše vyhledá specifické pro jazykovou verzi řetězce, které je dáno `CurrentUICulture`. Každé vlákno v rozhraní .NET má `CurrentCulture` a `CurrentUICulture` objekty. ASP.NET Core zkontroluje tyto hodnoty při vykreslování funkcí závislých na jazykové verzi. Pokud jazykové verze aktuálního vlákna je nastavena na "en US" (angličtina, USA), například `DateTime.Now.ToLongDateString()` zobrazí "Čtvrtek, února 18 2016", ale pokud `CurrentCulture` je nastaven na "es-ES" (španělština, Španělsko) bude výstup "jueves, de smluvních 18 de 2016".

## <a name="resource-files"></a>Soubory prostředků

Soubor prostředků je užitečné mechanismus pro oddělení lokalizovatelný řetězce z kódu. Přeložené řetězce pro jiné než výchozí jazyk izolují *RESX* soubory prostředků. Například můžete chtít vytvořit španělské zdrojový soubor s názvem *Welcome.es.resx* obsahující přeložit řetězce. "es" je kód jazyka pro španělštinu. K vytvoření tohoto souboru prostředků v sadě Visual Studio:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na složku, která bude obsahovat soubor prostředků > **přidat** > **novou položku**.

    ![Vnořené kontextové nabídky: V Průzkumníku řešení Kontextová nabídka je otevřen pro prostředky. Druhá Kontextová nabídka je otevřen pro přidat zobrazující zvýrazněná příkaz novou položku.](localization/_static/newi.png)

2. V **vyhledávání – nainstalované šablony** zadejte "prostředek" a název souboru.

    ![Přidat novou položku – dialogové okno](localization/_static/res.png)

3. Zadejte hodnotu klíče (nativní string) **název** sloupce a přeložené řetězce v **hodnotu** sloupce.

    ![Welcome.ES.resx soubor (soubor úvodní prostředků pro španělštinu) slovem Hello ve sloupci Název a word Hola (Hello ve španělštině) ve sloupci Hodnota](localization/_static/hola.png)

    Visual Studio zobrazí *Welcome.es.resx* souboru.

    ![Průzkumník řešení zobrazující souboru prostředků úvodní Španělština (es)](localization/_static/se.png)

<a name="error"></a>

Pokud používáte verzi sady Visual Studio 2017 Preview 15.3, získáte v editoru prostředků indikátor chyby. Odeberte *ResXFileCodeGenerator* z hodnoty *Custom Tool* mřížky vlastnosti, aby se tato chybová zpráva:

![Resx editoru](localization/_static/err.png)

Alternativně můžete tuto chybu ignorovat. Věříme, chcete-li v příští verzi.

## <a name="resource-file-naming"></a>Pojmenovávání souborů prostředků

Prostředky jsou pojmenované pro název úplné typu jejich třídy minus název sestavení. Například francouzštině prostředků v projektu, jehož hlavní sestavení `LocalizationWebsite.Web.dll` pro třídu `LocalizationWebsite.Web.Startup` by se jmenovala *Startup.fr.resx*. Prostředek pro třídu `LocalizationWebsite.Web.Controllers.HomeController` by se jmenovala *Controllers.HomeController.fr.resx*. Pokud cílovou třídu oboru názvů není stejný jako název sestavení je nutné název úplné typu. Například v ukázkovém projektu prostředků pro typ `ExtraNamespace.Tools` by se jmenovala *ExtraNamespace.Tools.fr.resx*.

V ukázkovém projektu `ConfigureServices` metoda nastaví `ResourcesPath` "Zdroje", takže projektu relativní cesta k souboru francouzštině prostředek domovské řadičem je *Resources/Controllers.HomeController.fr.resx*. Složky můžete alternativně použít k uspořádání souborů prostředků. Pro domácí řadič, cesta bude *Resources/Controllers/HomeController.fr.resx*. Pokud nepoužijete `ResourcesPath` možnost, *RESX* by se dostala soubor v adresáři základní projektu. Soubor prostředků pro `HomeController` by se jmenovala *Controllers.HomeController.fr.resx*. Možnost použití tečky nebo cesta zásady vytváření názvů závisí na tom, jak chcete uspořádání souborů prostředků.

| Název prostředku | Tečky nebo názvů cest |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | Tečka  |
| Resources/Controllers/HomeController.fr.resx  | Cesta |
|    |     |

Soubory prostředků pomocí `@inject IViewLocalizer` v zobrazení syntaxe Razor podle podobný Princip. Soubor prostředků pro zobrazení může mít název pomocí pojmenování tečkou nebo názvů cest. Soubory prostředků zobrazení syntaxe Razor napodobovat cestu k souboru jejich přidružené zobrazení. Za předpokladu, že nastaví `ResourcesPath` Chcete-li "Zdroje", soubor francouzštině prostředků související s *Views/Home/About.cshtml* zobrazení může být buď z následujících akcí:

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

Pokud nepoužijete `ResourcesPath` možnost, *RESX* soubor pro zobrazení by nacházet ve stejné složce jako zobrazení.

## <a name="culture-fallback-behavior"></a>Chování záložní jazykovou verzi

Při hledání prostředku, lokalizace mezi "alternativního národního prostředí". Od požadovanou jazykovou verzi, pokud nebyl nalezen, se vrátí do nadřazené jazyková verze této jazykové verze. Jako vyhraďte [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) vlastnost představuje nadřazenou jazykovou verzi. To obvykle (ale ne vždy) znamená national signifier odebráním ISO. Požadovaný dialekt Španělština používaný v Mexico je například "es-MX". Má nadřazený "es"&mdash;španělské nespecifickou do kterékoli zemi.

Představte si, že vaše lokalita obdrží požadavek pro prostředek "Vítá", s využitím jazykové verze "fr-CA". Lokalizace systému hledá v následujících zdrojích v pořadí a vybere na první shodu:

* *Welcome.fr-CA.resx*
* *Welcome.fr.resx*
* *Welcome.resx* (Pokud `NeutralResourcesLanguage` je "fr-CA")

Jako příklad Pokud odeberete označení culture ".fr" a budete mít jazykové verze nastavte na francouzštinu, výchozí soubor prostředků je pro čtení a jsou lokalizované řetězce. Správce prostředků označí výchozí nebo záložní prostředku pro Pokud nic splňuje vaše požadovanou jazykovou verzi. Pokud chcete právě vracet klíč chybí prostředek pro požadovanou jazykovou verzi můžete nesmí mít výchozí soubor prostředků.

### <a name="generate-resource-files-with-visual-studio"></a>Generovat soubory prostředků pomocí sady Visual Studio

Pokud vytvoříte soubor prostředků v sadě Visual Studio bez jazykové verzi v názvu souboru (například *Welcome.resx*), s vlastností pro každý řetězec třída C# vytvoří sada Visual Studio. Který je obvykle není co chcete s ASP.NET Core. Obvykle nebudete mít výchozí *RESX* souboru prostředků ( *RESX* soubor bez název jazykové verze). Doporučujeme vám vytvořit *RESX* soubor s názvem jazykové verze (například *Welcome.fr.resx*). Při vytváření *RESX* soubor s názvem jazykovou verzi sady Visual Studio nebude generovat soubor třídy. Očekáváme, že celá řada vývojářů nevytvoří soubor výchozí jazyk prostředků.

### <a name="add-other-cultures"></a>Přidání nových jazykových verzí

Každá kombinace jazyka a jazykovou verzi (jiné než výchozí jazyk) vyžaduje jedinečný soubor prostředků. Soubory prostředků jiných jazykových verzí a národní prostředí vytvoříte tak, že vytvoříte nové soubory prostředků, ve kterých jsou ISO kód jazyka součástí názvu souboru (například **en-us**, **fr-ca**, a  **en-gb**). Tyto kódy ISO jsou umístěny mezi název souboru a *RESX* souboru rozšíření, jako v *Welcome.es MX.resx* (španělština/Mexico).

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>Implementace strategie pro vyberte jazyk nebo jazykovou verzi pro každý požadavek

### <a name="configure-localization"></a>Konfigurace lokalizace

Lokalizace je nakonfigurovaný v `ConfigureServices` metoda:

[!code-csharp[](localization/sample/Localization/Program.cs?name=snippet1)]

* `AddLocalization` Přidá službu lokalizaci ke kontejneru služby. Kód výše také nastaví prostředky cesta k "Zdroje".

* `AddViewLocalization` Přidává podporu pro lokalizované zobrazit soubory. V této ukázkové zobrazení lokalizace založen na příponě souboru zobrazení. Například "fr" v *Index.fr.cshtml* souboru.

* `AddDataAnnotationsLocalization` Přidává podporu pro lokalizované `DataAnnotations` ověření zprávy prostřednictvím `IStringLocalizer` abstrakce.

### <a name="localization-middleware"></a>Lokalizace middlewaru

Aktuální jazykovou verzi na vyžádání je nastavena v lokalizace [Middleware](xref:fundamentals/middleware/index). Lokalizace middlewaru je povolena v `Configure` metoda. Lokalizace middleware musí být nakonfigurovaná před veškerý middleware, která může kontrola jazyková verze požadavku (například `app.UseMvcWithDefaultRoute()`).

[!code-csharp[](localization/sample/Localization/Program.cs?name=snippet2)]

`UseRequestLocalization` inicializuje `RequestLocalizationOptions` objektu. U každého požadavku seznamu z `RequestCultureProvider` v `RequestLocalizationOptions` je výčet a první zprostředkovatele, který můžete určit úspěšně jazykovou verzi požadavku se používá. Výchozí zprostředkovatele pocházet z `RequestLocalizationOptions` třídy:

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

Výchozím seznamu přejde od nejvíce po nejméně specifická. Později v článku uvidíme, jak můžete změnit pořadí a dokonce přidat vlastní jazykové verze zprostředkovatele. Pokud žádná z zprostředkovatele můžete určit, k žádosti o jazykové verzi, `DefaultRequestCulture` se používá.

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

Některé aplikace bude používat řetězec dotazu nastavit [jazykovou verzi a jazyková verze uživatelského rozhraní](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx). Pro aplikace, které používají soubor cookie nebo přístup hlavičky Accept-Language přidání na adresu URL, řetězec dotazu je vhodný pro ladění a testování kódu. Ve výchozím nastavení `QueryStringRequestCultureProvider` je registrován jako první lokalizace zprostředkovatele v `RequestCultureProvider` seznamu. Předávání parametrů řetězce dotazu `culture` a `ui-culture`. Následující příklad nastaví Španělština/Mexico konkrétní jazykové verze (jazyk a oblast):

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

Pokud předáte pouze v jednom ze dvou (`culture` nebo `ui-culture`), nastaví obě hodnoty pomocí jeden, je předaná zprostředkovateli řetězec dotazu. Například nastavit právě jazykovou verzi nastaví i `Culture` a `UICulture`:

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

Produkční aplikace bude často poskytují mechanismus nastavit jazykovou verzi pomocí souboru cookie jazykovou verzi ASP.NET Core. Použití `MakeCookieValue` metodu pro vytvoření souboru cookie.

`CookieRequestCultureProvider` `DefaultCookieName` Vrátí výchozí název souboru cookie použít ke sledování uživatel upřednostňované jazykové verze. Výchozí název souboru cookie je `.AspNetCore.Culture`.

Formát souboru cookie je `c=%LANGCODE%|uic=%LANGCODE%`, kde `c` je `Culture` a `uic` je `UICulture`, například:

    c=en-UK|uic=en-US

Pokud zadáte jenom jeden z informace o jazykové a jazyková verze uživatelského rozhraní, použije se zadanou jazykovou verzi pro informace o jazykové a jazyková verze uživatelského rozhraní.

### <a name="the-accept-language-http-header"></a>Hlavička Accept-Language HTTP

[Hlavičky Accept-Language](https://www.w3.org/International/questions/qa-accept-lang-locales) je možné nastavit v většina prohlížečů a byl původně určený k zadejte jazyk pro uživatele. Toto nastavení určuje, co prohlížeče byla nastavena k odeslání, nebo dědí ze základního operačního systému. Hlavičku Accept-Language HTTP z prohlížeče požadavku není spolehlivý způsob zjistit upřednostňovaný jazyk uživatele (viz [nastavení jazykové předvolby v prohlížeči](https://www.w3.org/International/questions/qa-lang-priorities.en.php)). Produkční aplikace by měla obsahovat způsob, jak uživatelům přizpůsobit si sami vyberou jazykové verze.

### <a name="set-the-accept-language-http-header-in-ie"></a>Nastavit hlavičku Accept-Language HTTP v aplikaci Internet Explorer

1. V zařízeních ikonu, klepněte na **Možnosti Internetu**.

2. Klepněte na **jazyky**.

    ![Možnosti Internetu](localization/_static/lang.png)

3. Klepněte na **nastavit předvolby jazyka**.

4. Klepněte na **přidat jazyk**.

5. Přidáte jazyk.

6. Klepněte na jazyk a potom klepněte na **nahoru**.

### <a name="use-a-custom-provider"></a>Použití vlastního zprostředkovatele

Předpokládejme, že chcete zákazníkům ukládat jejich jazyce a jazykové verzi v databázích máte. Můžete napsat zprostředkovatele k vyhledání tyto hodnoty pro uživatele. Následující kód ukazuje, jak přidat vlastního zprostředkovatele:

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

Použití `RequestLocalizationOptions` chcete přidat nebo odebrat lokalizace zprostředkovatele.

### <a name="set-the-culture-programmatically"></a>Nastavit jazykovou verzi prostřednictvím kódu programu

Tato ukázka **Localization.StarterWeb** projektu na [Githubu](https://github.com/aspnet/entropy) obsahuje uživatelského rozhraní nastavit `Culture`. *Views/Shared/_SelectLanguagePartial.cshtml* souboru můžete vybrat ze seznamu podporovaných jazykových verzí jazyková verze:


[!code-cshtml[](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

*Views/Shared/_SelectLanguagePartial.cshtml* se přidá soubor `footer` rozložení souboru, bude k dispozici u všech zobrazení:

[!code-cshtml[](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

`SetLanguage` Metoda nastaví jazykovou verzi souboru cookie.

[!code-csharp[](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

Nelze připojit *_SelectLanguagePartial.cshtml* na ukázkový kód pro tento projekt. **Localization.StarterWeb** projektu na [Githubu](https://github.com/aspnet/entropy) má kód, které jsou předávány `RequestLocalizationOptions` k částečné prostřednictvím Razor [vkládání závislostí](dependency-injection.md) kontejneru.

## <a name="globalization-and-localization-terms"></a>Globalizace a lokalizace podmínky

Proces lokalizace aplikace taky vyžaduje základní znalost relevantní znakových sad, které jsou běžně používaný v vývoj moderních softwaru a pochopení problémů, souvisejících s nimi. I když všechny počítače ukládání textu čísla (kódy), uložení různých systémů stejný text použití různých čísel. Proces lokalizace odkazuje na překladu aplikace uživatelského rozhraní (UI) pro konkrétní jazykové verze nebo národní prostředí.

[Lokalizovatelnost](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review) je dílčí proces pro ověření, že globalizovaná aplikace je připravena k lokalizaci.

[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) formátu pro název jazykové verze je `<languagecode2>-<country/regioncode2>`, kde `<languagecode2>` je kód jazyka a `<country/regioncode2>` je subkulturu kód. Například `es-CL` pro španělština (základě), `en-US` pro angličtinu (Spojené státy), a `en-AU` pro angličtinu (Austrálie). [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) je kombinací ISO 639 malá jazykovou verzi dvoupísmenným kódem související s jazykem a ISO 3166 velká subkulturu dvoupísmenným kódem přidružené zemi či oblasti. V tématu [název jazykové verze jazyka](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).

Internacionalizace často zkratka "I18N". Zkratka přebírá první a poslední písmena a počet písmena mezi nimi, takže 18 znamená počet písmena mezi první "I" a poslední "N". Totéž platí i pro (G11N) globalizace a lokalizace (L10N).

Podmínky:

* Globalizace (G11N): Proces převedení aplikace podporují různé jazyky a oblasti.
* Lokalizace (L10N): Proces přizpůsobení aplikace pro daný jazyk a oblast.
* Internacionalizace (I18N): Popisuje globalizace a lokalizace.
* Jazyková verze: Je jazyk a volitelně oblast.
* Neutrální jazykové verze: jazyková verze, která má určený jazyk, ale není v oblasti. (například "en", "es")
* Konkrétní jazykové verze: jazyková verze, která má zadaný jazyk a oblast. (pro příklad "en US", "en-GB", "es-CL")
* Nadřazená jazyková verze: neutrální jazykovou verzi, která obsahuje konkrétní jazykové verze. (například "en" je nadřazenou jazykovou verzi "en US" a "en-GB")
* Národní prostředí: Národního prostředí je stejný jako jazykovou verzi.

## <a name="additional-resources"></a>Další zdroje

* [Projekt Localization.StarterWeb](https://github.com/aspnet/entropy) použít v článku.
* [Soubory prostředků v sadě Visual Studio](https://docs.microsoft.com/cpp/windows/resource-files-visual-studio)
* [Prostředky v soubory RESX](https://docs.microsoft.com/dotnet/framework/resources/working-with-resx-files-programmatically)
* [Sada nástrojů pro vícejazyčné aplikace](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
