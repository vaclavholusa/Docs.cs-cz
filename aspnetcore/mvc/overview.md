---
title: "Přehled ASP.NET Core MVC"
author: ardalis
description: "Zjistěte, jak je bohaté rozhraní pro vytváření webových aplikací ASP.NET MVC jádra a rozhraní API pomocí Model-View-Controller návrh vzor."
ms.author: riande
manager: wpickett
ms.date: 01/08/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: ad8a1dfae89a7ecd5573c16ba70d7d12216b4c57
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="overview-of-aspnet-core-mvc"></a>Přehled ASP.NET Core MVC

Podle [Steve Smith](https://ardalis.com/)

Jádro ASP.NET MVC je bohaté rozhraní pro vytváření webových aplikací a rozhraní API pomocí Model-View-Controller návrh vzor.

## <a name="what-is-the-mvc-pattern"></a>Co je vzor MVC?

Vzor architektury Model-View-Controller (MVC) rozděluje aplikace do tří hlavních skupin součástí: modely, zobrazení a Kontrolery. Tento vzor pomáhá zajistit [oddělené oblasti zájmu](http://deviq.com/separation-of-concerns/). Pomocí tohoto vzoru, se směrují požadavky uživatelů na řadič, která zajišťuje pro práci s modelem provádět akce uživatele nebo načíst výsledky dotazů. Řadičem vybere zobrazení pro uživatele a poskytuje Model data, která vyžaduje.

Následující diagram znázorňuje třemi hlavními komponentami a ty, které odkazují jiné:

![Vzor MVC](overview/_static/mvc.png)

Tato vymezení odpovědnosti umožňuje škálovat aplikaci z hlediska složitost, protože je jednodušší code, ladit a testovat (model, zobrazení nebo řadič) něčeho, co má jediné úlohy (a odpovídá [jeden zásady odpovědnosti ](http://deviq.com/single-responsibility-principle/)). Je obtížné aktualizace, testování a ladění kódu, který má závislosti rozloženy dva nebo víc z těchto tří oblastí. Například logiku uživatelského rozhraní se obvykle změnit častěji, než obchodní logiku. Pokud kód a obchodní logiku prezentace jsou sloučeny do jednoho objektu, budete muset upravit objekt, který obsahuje obchodní logiku pokaždé, když změníte uživatelské rozhraní. Je to pravděpodobně vzniku chyb, a vyžaduje opakované zkoušky všechny obchodní logiky po změně každých minimální uživatelské rozhraní.

> [!NOTE]
> Zobrazení a kontroler závisí na modelu. Model však závisí na zobrazení ani kontroleru. To je jedno z klíčových výhod oddělení. Toto rozdělení umožňuje modelu, který má být vytvořeny a testovány nezávislé vizuální prezentaci.

### <a name="model-responsibilities"></a>Model odpovědnosti

Modelu v aplikaci MVC představuje stav aplikace a veškeré obchodní logiky nebo operace, které by měli provádět ho. Obchodní logika by měl zapouzdřený v modelu, společně s žádné implementační logika pro uchování stavu aplikace. Zobrazení silného typu obvykle používat typy ViewModel navržený tak, aby obsahují data pro zobrazení v tomto zobrazení. Správce vytvoří a naplní tyto instance ViewModel z modelu.

> [!NOTE]
> K uspořádání modelu v aplikaci, která používá architekturní vzor MVC mnoha způsoby. Další informace o některých [různé druhy modelu typy](http://deviq.com/kinds-of-models/).

### <a name="view-responsibilities"></a>Zobrazit odpovědnosti

Zobrazení jsou zodpovědní za prezentací obsahu v uživatelském rozhraní. Používají [zobrazovací modul Razor](#razor-view-engine) pro vložení kódu .NET do kódu HTML. Musí být minimální logiku v zobrazeních a veškeré logiky v nich by se měly týkat prezentací obsah. Pokud zjistíte, třeba provádět značnou část logiky v zobrazení souborů, aby bylo možné zobrazit data z komplexní model, zvažte použití [zobrazení součást](views/view-components.md), ViewModel, nebo zobrazit šablonu pro zjednodušení zobrazení.

### <a name="controller-responsibilities"></a>Odpovědnosti řadiče

Kontrolery jsou komponenty, které zpracovávají interakci s uživatelem, pracují s modelem a konečně vybírají vykreslené zobrazení. V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce. Ve vzoru MVC řadičem je počáteční vstupní bod a je odpovědná za výběr modelu typy pro práci s a které zobrazení k vykreslení (proto jeho název – se určuje, jak aplikaci reagovat na daný požadavek).

> [!NOTE]
> Řadiče by neměl být ztěžuje příliš moc odpovědnosti. Pokud chcete zachovat řadiče logiku stal příliš složité, použijte [jeden zásady odpovědnosti](http://deviq.com/single-responsibility-principle/) na obchodní logiku nabízené z kontroleru a do modelu domény.

>[!TIP]
> Pokud zjistíte, že vaše akce kontroleru často provádějí stejné typy akcí, můžete podle [nemusíte sami opakujte Princip](http://deviq.com/don-t-repeat-yourself/) přesunutím těchto běžné akce do [filtry](#filters).

## <a name="what-is-aspnet-core-mvc"></a>Co je technologie ASP.NET MVC jádra

Rozhraní MVC ASP.NET Core je zdroj lightweight, otevřete intenzivního prezentační architektura optimalizovaný pro použití s ASP.NET Core.

Jádro ASP.NET MVC poskytuje na základě vzory způsob, jak vytvářet dynamické weby, která umožňuje čistou oddělené oblasti zájmu. Poskytuje úplnou kontrolu nad značek, podporuje vývoj TDD popisný a využívá nejnovější webové standardy.

## <a name="features"></a>Funkce

Jádro ASP.NET MVC zahrnuje následující položky:

* [Směrování](#routing)
* [Vazby modelu](#model-binding)
* [Ověření modelu](#model-validation)
* [Vkládání závislostí](../fundamentals/dependency-injection.md)
* [Filtry](#filters)
* [Oblasti](#areas)
* [Webová rozhraní API](#web-apis)
* [Možnosti testování](#testability)
* [Zobrazovací modul Razor](#razor-view-engine)
* [Zobrazení se silnými typy](#strongly-typed-views)
* [Pomocné rutiny značek](#tag-helpers)
* [Zobrazení součásti](#view-components)

### <a name="routing"></a>Směrování

ASP.NET MVC základní je postavený na [směrování ASP.NET Core](../fundamentals/routing.md), výkonné komponenta mapování adres URL, která umožňuje vytvářet aplikace, které mají srozumitelné a dohledatelné adresy URL. To umožňuje definovat vaší aplikace vzory mapování adres URL které fungují dobře u optimalizaci pro vyhledávací weby (SEO) a pro generování odkazů, bez ohledu na tom, jak jsou uspořádány soubory na vašem webovém serveru. Můžete definovat pomocí syntaxi šablony pohodlný trasy, která podporuje hodnotu omezení trasy, výchozích hodnot a volitelné hodnoty trasy.

*Založené na konvenci směrování* umožňuje globálně určit adresu URL formáty, které vaše aplikace přijímá a jak každou z těchto formátů mapuje metodu konkrétní akce na zadaný kontroler. Po přijetí příchozího požadavku modulu Směrování analyzuje adresu URL a odpovídá jednomu z definovaných formátech adres URL a pak zavolá metodu akce přidruženého kontroleru.

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

*Atribut směrování* umožňuje zadat informace o směrování podle architekturu řadiče a akce s atributy, které definují trasy vaší aplikace. To znamená, že budou vedle kontroleru a akce, ke kterému jsou přidružené umístěny definic trasy.

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a>Vazby modelu

Jádro ASP.NET MVC [model vazby](models/model-binding.md) převede na objekty, které může zpracovat řadičem dat žádostí klienta (hodnot formuláře, data trasy, parametrů řetězce dotazu, hlaviček protokolu HTTP). V důsledku toho řadič logiky nemá udělat práci při zjištění data na příchozí žádost; jednoduše má data jako parametry pro její metody akce.

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>ověření modelu

ASP.NET MVC základní podporuje [ověření](models/validation.md) podle architekturu modelu objektu s atributy ověření dat poznámky. Atributy ověření se kontroluje na straně klienta, než hodnoty jsou odeslány na server, jakož i na serveru před akce kontroleru je volána.

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

Akce kontroleru:

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

Rozhraní framework zpracovává ověřování data požadavku na klientovi i na serveru. Ověření logiku určenou na typech modelu, přidá se do vykreslené zobrazení jako nerušivý poznámky a je požadováno v prohlížeči s [jQuery ověření](https://jqueryvalidation.org/).

### <a name="dependency-injection"></a>Vkládání závislostí

Má integrovanou podporu pro ASP.NET Core [vkládání závislostí (DI)](../fundamentals/dependency-injection.md). V aplikaci ASP.NET MVC jádra [řadiče](controllers/dependency-injection.md) můžete žádost o potřebných službám pomocí jejich konstruktory, což jim umožní postupujte podle [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/).

Můžete také použít aplikaci [vkládání závislostí v zobrazení souborů](views/dependency-injection.md)pomocí `@inject` – direktiva:

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a>Filtry

[Filtry](controllers/filters.md) pomoci vývojářům zapouzdření mezi vyjímání obavy, jako je zpracování výjimek nebo autorizace. Filtry povolit spuštění vlastní před a po zpracování logiky pro metody akce a může být nakonfigurována pro spuštění v určitých bodech, v rámci spouštěcí kanál pro daný požadavek. Filtry lze použít k řadiče nebo akce jako atributy (nebo můžete spustit globálně). Několik filtry (například `Authorize`) jsou zahrnuty v rozhraní framework.


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a>Oblasti

[Oblasti](controllers/areas.md) poskytnout způsob, jak oddílu velké ASP.NET Core MVC webové aplikace do menších funkční seskupení. Oblast je strukturu MVC uvnitř aplikace. V projektu MVC logické součásti jako Model, Kontroleru a zobrazení jsou uchovány v různých složkách a konvence pojmenování aplikace MVC používá k vytvoření vztahu mezi těmito součástmi. Pro velké aplikace může být výhodné oddílu aplikace na samostatné vysoké úrovni oblasti funkcí. Pro instanci elektronické obchodování aplikace s více organizačních jednotek, jako je například checkout, fakturace a vyhledávání atd. Každý z těchto jednotek mají své vlastní logickou součástí zobrazení, řadiče a modely.

### <a name="web-apis"></a>Webová rozhraní API

Kromě toho, představuje vynikající platformu pro tvorbu webů, má technologie ASP.NET MVC základní podpory pro vytváření webových rozhraní API. Můžete vytvářet služby, které využity širokou škálou klientů včetně prohlížečů a mobilních zařízení.

Rozhraní framework zahrnuje podporu pro vyjednávání obsahu HTTP s integrovanou podporu pro [formátování dat](models/formatting.md) jako XML nebo JSON. Zápis [vlastní formátování](advanced/custom-formatters.md) přidání podpory pro vlastní formáty.

Povolení podpory pro hypermédií pomocí generování odkazů. Snadno povolit podporu pro [(CORS) pro sdílení prostředků různého původu](http://www.w3.org/TR/cors/) tak, aby webová rozhraní API můžete sdílet mezi několika webových aplikací.

### <a name="testability"></a>Možnosti testování

Použití rozhraní framework rozhraní a vkládání závislostí proveďte ho vhodným testování částí a funkcí (např. TestHost a InMemory zprostředkovatele Entity Framework), které zahrnuje rozhraní [testování integrace](../testing/integration-testing.md) rychlé a snadno také. Další informace o [testování řadiče logiku](controllers/testing.md).

### <a name="razor-view-engine"></a>Zobrazovací modul Razor

[Zobrazení ASP.NET MVC základní](views/overview.md) použít [zobrazovací modul Razor](views/razor.md) k vykreslení zobrazení. Syntaxe Razor je compact, výrazovou a plynulá práce šablony značek jazyk pro definování zobrazení pomocí vloženého kódu jazyka C#. Syntaxe Razor je používaný k dynamickému generování webového obsahu na serveru. Ještě jednou je možné kombinovat kódu serveru s obsah na straně klienta a kódu.

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

Pomocí zobrazovací modul Razor můžete definovat [rozložení](views/layout.md), [částečná zobrazení](views/partial.md) a replaceable oddílů.

### <a name="strongly-typed-views"></a>Zobrazení se silnými typy

Zobrazení MVC Razor můžete být silného typu podle modelu. Řadiče můžete předat povolení zobrazení tak, aby měl kontrolu typu a podporu technologie IntelliSense zobrazení silného typu modelu.

Například následující zobrazení vykreslí modelu typu `IEnumerable<Product>`:

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>Pomocníci značky

[Značka pomocné rutiny](views/tag-helpers/intro.md) povolit kód straně serveru k účasti ve vytváření a vykreslení elementů HTML v souborech Razor. Značka pomocné rutiny můžete použít k definování vlastní značky (například `<environment>`) nebo chcete změnit chování stávající značky (například `<label>`). Pomocníci značka vytvořit vazbu na konkrétní elementy na základě názvu elementu a jeho atributy. Poskytují výhod vykreslování na straně serveru při zachování stále HTML prostředí pro úpravy.

Existuje mnoho předdefinovaných značky pomocníci pro běžné úlohy -, jako je například vytváření formulářů, odkazy, načítání prostředků a další - a i další dostupné ve veřejných úložišť GitHub a jako NuGet balíčky. Pomocníci značky vytvořené v C# a jejich cílových elementů HTML na základě název elementu, název atributu nebo nadřazené značky. Například předdefinovaná LinkTagHelper lze vytvořit odkaz `Login` akce `AccountsController`:

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

`EnvironmentTagHelper` Slouží k zobrazení (například nezpracovanou nebo minifikovaný) založených na prostředí runtime, jako je například vývoj, pracovním nebo produkčním zahrnout jiné skripty:

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

Pomocníci značky poskytují HTML-friendly vývojového prostředí a bohaté prostředí technologie IntelliSense pro vytvoření značky HTML a syntaxe Razor. Většina předdefinované značky Pomocníci cíli stávající elementy HTML a poskytuje serverové atributy elementu.

### <a name="view-components"></a>Zobrazení součásti

[Zobrazit součásti](views/view-components.md) umožňují balíček logiku vykreslování a opakovaně ji používat v celé aplikaci. Jsou podobná [částečná zobrazení](views/partial.md), ale přidružené logiky.
