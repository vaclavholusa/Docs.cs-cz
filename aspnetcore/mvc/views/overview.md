---
title: "Zobrazení v jádro ASP.NET MVC"
author: ardalis
description: "Zjistěte, jak pracovat zobrazení prezentace dat aplikace a interakce uživatelů v aplikaci ASP.NET MVC jádra."
manager: wpickett
ms.author: riande
ms.date: 12/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/overview
ms.openlocfilehash: 4f55b3b439d268c371ce40a298b0f63dad9eef69
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2018
---
# <a name="views-in-aspnet-core-mvc"></a>Zobrazení v jádro ASP.NET MVC

Podle [Steve Smith](https://ardalis.com/) a [Luke Latham](https://github.com/guardrex)

Tento dokument popisuje zobrazení použitých v aplikacích ASP.NET MVC jádra. Informace na stránkách Razor najdete v tématu [Úvod do stránky Razor](xref:mvc/razor-pages/index).

V **M**odelu -**V**obrazit -**C**vzor ontroller (MVC), *zobrazení* zpracovává data aplikace prezentace a uživatelské interakce. Zobrazení je šablony HTML s vložených [kódu Razor](xref:mvc/views/razor). Značka Razor je kód, který komunikuje s kódu HTML, který vytvoří webovou stránku, která je odeslána do klienta.

V aplikaci ASP.NET MVC jádra, jsou zobrazení *.cshtml* soubory, které používají [programovací jazyk C#](/dotnet/csharp/) v kódu Razor. Obvykle jsou seskupené zobrazit soubory do složky s názvem pro jednotlivé aplikace [řadiče](xref:mvc/controllers/actions). Složky jsou uložené v *zobrazení* složku v kořenovém adresáři aplikace:

![Zobrazení složky v řešení pro Průzkumníka Visual Studio je otevřený s otevřený a zobrazit soubory About.cshtml, Contact.cshtml a Index.cshtml Domovská složka](overview/_static/views_solution_explorer.png)

*Domů* je reprezentována řadiče *Domů* složky uvnitř *zobrazení* složky. *Domů* složka obsahuje zobrazení pro *o*, *kontaktujte*, a *Index* webové stránky (domovské stránce). Když uživatel požádá o, jednu z těchto tří webových stránkách akce kontroleru v *Domů* řadič určit, který tři zobrazení, které se používá k sestavení a vrátí webové stránky pro uživatele.

Použití [rozložení](xref:mvc/views/layout) k poskytování části konzistentní webové stránky a snížit opakování kódu. Rozložení často obsahují záhlaví, navigace a nabídky prvky a zápatí. Záhlaví a zápatí obvykle obsahuje často používaný kód pro mnoho elementy metadata a odkazy na prostředky skriptu a stylu. Rozložení umožňuje vyhnout se tento často používaný kód v zobrazení.

[Částečná zobrazení](xref:mvc/views/partial) snížit zdvojení kódu pomocí správy opakovaně použitelné součásti zobrazení. Například je užitečné pro biografii Autor na web blogu, který se zobrazí v několik zobrazení částečné zobrazení. Vytváření biografii je obyčejnou zobrazení obsahu a nevyžaduje kód provést, aby bylo možné vytvořit obsahu pro webovou stránku. Autor biografii obsah není k dispozici na zobrazení ve samostatně, vazby modelu, takže použití částečné zobrazení pro tento typ obsahu je ideální.

[Zobrazit součásti](xref:mvc/views/view-components) jsou podobná částečné zobrazení, protože umožňují snížit opakovaných kód, ale jsou vhodné pro zobrazení obsahu, který vyžaduje kód pro spuštění na serveru k vykreslení webovou stránku. Zobrazení součásti jsou užitečné při vykreslené obsah vyžaduje interakci databáze, například pro web nákupní košík. Zobrazení součásti nejsou omezena na modelu vazby k vytvoření výstupu webovou stránku.

## <a name="benefits-of-using-views"></a>Výhody použití zobrazení

Zobrazení nápovědy k vytvoření [ **S**eparation **o**f **C**oncerns (SoC) návrhu](http://deviq.com/separation-of-concerns/) v aplikaci MVC oddělením kód uživatelské rozhraní z dalších částí aplikace. Následující SoC návrhu, bude vaše aplikace modulární, který nabízí několik výhod:

* Tato aplikace je snazší správa, protože je lépe uspořádány. Zobrazení jsou obecně seskupené podle funkce aplikace. Díky tomu je snazší najít související zobrazení při práci na funkce.
* Součástí aplikace jsou volně vázány. Můžete vytvářet a aktualizovat zobrazení aplikace odděleně od komponenty obchodní logiku a data access. Zobrazení aplikace můžete upravit aniž byste museli mít aktualizovat dalších částí aplikace.
* Aby bylo jednodušší testování části uživatelského rozhraní aplikace, protože zobrazení jsou samostatné jednotky.
* Kvůli lepší organizaci je méně pravděpodobné, že budete ať už náhodně opakování části uživatelského rozhraní.

## <a name="creating-a-view"></a>Vytvoření zobrazení

Zobrazení, které jsou specifické pro určitý kontroler jsou vytvořené v *zobrazení / [ControllerName]* složky. Zobrazení, které jsou sdíleny mezi řadiče jsou umístěny v *zobrazení a sdílených* složky. Pokud chcete vytvořit zobrazení, přidejte nový soubor a poskytněte stejný název jako jeho přidruženému kontroleru akce s *.cshtml* příponu souboru. Vytvoření zobrazení, která odpovídá *o* akce v *Domů* řadič, vytvořit *About.cshtml* souboru v *zobrazení Domů*složky:

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Syntaxe Razor* značek začíná `@` symbol. Spusťte příkazy jazyka C# tím, že umístíte C# kódu v rámci [bloky kódu Razor](xref:mvc/views/razor#razor-code-blocks) nastavit ve složených závorkách (`{ ... }`). Například najdete v sekci přiřazení "O" `ViewData["Title"]` uvedené výše. Můžete zobrazit hodnoty v kódu HTML hodnotu s odkazem jednoduše `@` symbol. Zobrazit obsah `<h2>` a `<h3>` prvků uvedených výše.

Zobrazení obsahu uvedené výše je jenom část celé webové stránky, které je vykresleno do uživatele. Zbytek rozložení stránky a dalších běžných aspektů zobrazení jsou určené v jiných souborů zobrazení. Další informace najdete v tématu [rozložení tématu](xref:mvc/views/layout).

## <a name="how-controllers-specify-views"></a>Jak určit řadiče zobrazení

Zobrazení jsou obvykle vrácené z akce jako [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), který je typem [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult). Můžete vytvořit a vrátí metodu akce `ViewResult` přímo, ale neprovádí běžně. Vzhledem k tomu, že většina řadičů dědí [řadič](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), jednoduše použijte `View` Pomocná metoda vrátit `ViewResult`:

*HomeController.cs*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

Po návratu tato akce *About.cshtml* zobrazení zobrazen v poslední části vykresleno jako na následující webové stránce:

![O stránku v prohlížeči Edge](overview/_static/about-page.png)

`View` Pomocná metoda má několik přetížení. Volitelně můžete zadat:

* Explicitní zobrazení vrátit:

  ```csharp
  return View("Orders");
  ```
* A [modelu](xref:mvc/models/model-binding) mají být předána do zobrazení:

  ```csharp
  return View(Orders);
  ```
* Zobrazení a modelu:

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a>Zobrazení zjišťování

Když akce vrátí zobrazení, proces se nazývá *zobrazení zjišťování* proběhne. Tento proces určuje, který soubor zobrazení se používá na základě názvu zobrazení. 

Výchozí chování `View` – metoda (`return View();`) je lze vrátit zobrazení se stejným názvem jako metodu akce, ve kterém je volána. Například *o* `ActionResult` název metody řadiče se používá k hledání pro zobrazení soubor s názvem *About.cshtml*. Nejprve hledá modulu runtime v *zobrazení / [ControllerName]* složka pro zobrazení. Pokud se nenajde odpovídající zobrazení, vyhledávání *sdílené* složka pro zobrazení.

Není důležité, pokud se implicitně vrátíte `ViewResult` s `return View();` nebo explicitně předat název zobrazení má `View` metoda s `return View("<ViewName>");`. V obou případech zobrazení zjišťování hledá odpovídající soubor zobrazení v tomto pořadí:

   1. *Views/\[ControllerName]/\[ViewName].cshtml*
   1. *Views/Shared/\[ViewName].cshtml*

Místo názvu zobrazení se dá zadat cestu k zobrazení souboru. Pokud používáte absolutní cestu spouštění v kořenovém adresáři aplikace (volitelně od verze "/" nebo "~ /"), *.cshtml* musí být zadán rozšíření:

```csharp
return View("Views/Home/About.cshtml");
```

Relativní cesta můžete použít také k určení zobrazení v různých adresářích bez *.cshtml* rozšíření. Uvnitř `HomeController`, můžete se vrátit *Index* zobrazení vaší *spravovat* zobrazení s relativní cesta:

```csharp
return View("../Manage/Index");
```

Podobně můžete určit, na aktuální adresář konkrétní řadič se ". /" předpony:

```csharp
return View("./About");
```

[Částečná zobrazení](xref:mvc/views/partial) a [zobrazení součásti](xref:mvc/views/view-components) použijte mechanismy pro zjišťování podobné (ale ne identické).

Můžete přizpůsobit výchozí konvenci pro způsob zobrazení se nacházejí v rámci aplikace s použitím vlastní [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).

Zobrazení zjišťování spoléhá na hledání zobrazit soubory podle názvu souboru. Pokud podkladový systém souborů je velká a malá písmena, názvy zobrazení jsou pravděpodobně velká a malá písmena. Pro kompatibilitu mezi operační systémy malá a velká písmena mezi kontroleru a akce a přidružené zobrazení složky a názvy souborů. Pokud dojde k chybě, která soubor zobrazení nebyl nalezen při práci s systém souborů malá a velká písmena, potvrďte, že mezi požadované zobrazení souboru a název souboru skutečné zobrazení odpovídá malá a velká písmena.

Postupujte podle osvědčený postup, uspořádání strukturu souborů pro zobrazení tak, aby odrážela vztahy mezi řadiče, akce a zobrazení udržovatelnosti a srozumitelnější.

## <a name="passing-data-to-views"></a>Předávání dat zobrazení

Můžete předat data do zobrazení pomocí několik přístupů. Většina robustní přístupu slouží k zadání [modelu](xref:mvc/models/model-binding) typu v zobrazení. Tento model se často označuje jako *viewmodel*. Instance typu viewmodel předáte do zobrazení z akce.

Použití viewmodel k předávání dat do zobrazení umožňuje zobrazit využívat výhod *silné* kontrola typu. *Silného typování* (nebo *silného typu*) znamená, že všechny proměnné a konstanta nemá explicitně definovaný typ (například `string`, `int`, nebo `DateTime`). V době kompilace se kontroluje platnost typy používané v zobrazení.

[Visual Studio](https://www.visualstudio.com/vs/) a [Visual Studio Code](https://code.visualstudio.com/) seznamu členů silně typované třídy pomocí funkci [IntelliSense](/visualstudio/ide/using-intellisense). Pokud chcete zobrazit vlastnosti viewmodel, zadejte název proměnné pro viewmodel následovaný tečkou (`.`). To umožňuje rychlejší psaní kódu s méně chyby.

Zadejte model pomocí `@model` – direktiva. Použití modelu s `@Model`:

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

K poskytnutí modelu pro zobrazení, předává kontroleru je jako parametr:

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

Neexistují žádná omezení na modelu typy, které můžete zadat do zobrazení. Doporučujeme používat **P**rostý **O**ld **C**LR **O**viewmodels bject (objektů POCO) s minimální nebo žádnou chování (metody) definované. Obvykle jsou viewmodel třídy buď uložena v *modely* složku nebo samostatné *ViewModels* složku v kořenovém adresáři aplikace. *Adresu* viewmodel použít v předchozím příkladu je objektů POCO viewmodel, uložené v souboru s názvem *Address.cs*:

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

> [!NOTE]
> Nic brání použití stejné tříd pro vaše viewmodel typy a typy modelu vaší firmy. Použití samostatných modelů však umožňuje zobrazení k odlišení nezávisle z obchodní logiku a data přístup částí aplikace. Oddělení modely a viewmodels také nabízí výhody zabezpečení při použití modelů [model vazby](xref:mvc/models/model-binding) a [ověření](xref:mvc/models/validation) pro data odesílaná do aplikace uživatelem.


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a>Slabě typované data (ViewData a ViewBag)

Poznámka: `ViewBag` není k dispozici na stránkách Razor.

Kromě zobrazení silného typu zobrazení mají přístup k *slabě typované* (také nazývané *volného typu*) shromažďování dat. Na rozdíl od silné typy *slabé typy* (nebo *ztrátě typy*) znamená, že jste si deklarovat explicitně typu dat, který používáte. Shromažďování dat o slabě typované můžete použít pro předávání malé množství dat a deaktivovat kontrolery a zobrazení.

| Předávání dat mezi...                        | Příklad                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| Řadiče a zobrazení                             | Naplnění rozevíracího seznamu, s daty.                                          |
| Zobrazení a [rozložení zobrazení](xref:mvc/views/layout)   | Nastavení  **\<title >** obsahu elementu v zobrazení rozložení ze zobrazení souboru.  |
| [Částečné zobrazení](xref:mvc/views/partial) a zobrazení | Pomůcku, která zobrazuje data v závislosti na webové stránky, uživatel si vyžádal.      |

Tuto kolekci lze odkazovat pomocí buď `ViewData` nebo `ViewBag` vlastnosti kontrolery a zobrazení. `ViewData` Vlastnost je slovník slabě typované objektů. `ViewBag` Vlastnost je obálku kolem `ViewData` poskytuje dynamické vlastnosti základní `ViewData` kolekce.

`ViewData` a `ViewBag` jsou vyřešeny dynamicky za běhu. Vzhledem k tomu, že nemáte nabízejí kontrolu typu kompilaci, jak jsou obecně více náchylné než použití viewmodel. Z tohoto důvodu někteří vývojáři radši chtěli použít minimálně nebo nikdy `ViewData` a `ViewBag`.


<a name="VD"></a>

**ViewData**

`ViewData` je [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) objekt přistupovat prostřednictvím `string` klíče. Řetězec dat lze ukládat a používat přímo bez nutnosti přetypování, ale musíte vysílat dalších `ViewData` objekt hodnoty pro konkrétní typy při extrahování je. Můžete použít `ViewData` předání dat z řadiče zobrazení a v zobrazeních, včetně [částečná zobrazení](xref:mvc/views/partial) a [rozložení](xref:mvc/views/layout).

Tady je příklad, který nastavuje hodnoty pozdrav a adresy `ViewData` v akci:

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

Práce s daty v zobrazení:

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

**ViewBag**

Poznámka: `ViewBag` není k dispozici na stránkách Razor.

`ViewBag` je [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) objekt, který poskytuje dynamické přístup k objektům, které jsou uložené v `ViewData`. `ViewBag` může být pohodlnější můžete pracovat, protože nevyžaduje přetypování. Následující příklad ukazuje, jak používat `ViewBag` se stejné výsledky jako pomocí `ViewData` výše:

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

**Pomocí ViewData a ViewBag současně**

Poznámka: `ViewBag` není k dispozici na stránkách Razor.

Vzhledem k tomu `ViewData` a `ViewBag` odkazovat na stejnou základní `ViewData` kolekce, můžete je používat `ViewData` a `ViewBag` a kombinovat a párovat mezi nimi při čtení a zápis hodnoty.

Nastavit název pomocí `ViewBag` a popis pomocí `ViewData` v horní části *About.cshtml* zobrazení:

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

Při čtení vlastností ale reverse použití `ViewData` a `ViewBag`. V *_Layout.cshtml* souboru, získat název pomocí `ViewData` a získat pomocí popis `ViewBag`:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

Mějte na paměti, že řetězce nevyžadují přetypování pro `ViewData`. Můžete použít `@ViewData["Title"]` bez přetypování.

Pomocí obou `ViewData` a `ViewBag` ve stejnou dobu funguje, jako nepodporuje kombinace a porovnávání čtení a zápisu vlastnosti. Následující kód je vykreslen:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**Souhrnné informace o rozdílech mezi ViewData a ViewBag**

 `ViewBag` není k dispozici na stránkách Razor.

* `ViewData`
  * Odvozená z [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), takže má slovník vlastností, které mohou být užitečné, například `ContainsKey`, `Add`, `Remove`, a `Clear`.
  * Klíče ve slovníku jsou řetězce, takže je povolen prázdný znak. Příklad: `ViewData["Some Key With Whitespace"]`
  * Jakýkoli typ jinými než `string` musíte vysílat v zobrazení použít `ViewData`.
* `ViewBag`
  * Odvozená z [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), takže umožňuje vytváření dynamických vlastností pomocí zápisu s tečkou (`@ViewBag.SomeKey = <value or object>`), a není vyžadováno žádné přetypování. Syntaxe `ViewBag` umožňuje rychlejší pro přidání do kontrolery a zobrazení.
  * Jednodušší zkontrolujte hodnoty null. Příklad: `@ViewBag.Person?.Name`

**Kdy použít ViewData nebo ViewBag**

Obě `ViewData` a `ViewBag` jsou rovnoměrně platný přístupy k předání malé množství dat mezi kontrolery a zobrazení. Volba používané podle preference. Můžete kombinovat a párovat `ViewData` a `ViewBag` objekty, ale kód je snazší ke čtení a udržovat s jedním ze způsobů použít konzistentně. Obou přístupů jsou přeložit dynamicky za běhu a proto náchylné k způsobuje chyby za běhu. Vyhněte se některé vývojové týmy je.

### <a name="dynamic-views"></a>Dynamické zobrazení

Typ zobrazení, která není deklarovat model pomocí `@model` , ale dostatek instanci modelu předán do je (například `return View(Address);`) může odkazovat na instanci vlastnosti dynamicky:

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Tato funkce nabízí flexibilitu ale nenabízí kompilace ochrany ani IntelliSense. Pokud vlastnost neexistuje, webová stránka generování selže za běhu.

## <a name="more-view-features"></a>Více zobrazení funkcí

[Značka pomocné rutiny](xref:mvc/views/tag-helpers/intro) snadno přidat chování na straně serveru do existující značky HTML. Pomocí značky Pomocníci zabraňuje nemusíte psát vlastní kód nebo pomocné rutiny v rámci zobrazení. Značka pomocníků se použijí jako atributy prvků HTML a ignorují podle editory, které nelze zpracovat. To umožňuje upravit a vykreslit zobrazení Kód v různých nástrojů.

Generování vlastní značky HTML můžete dosáhnout s mnoho předdefinovaných pomocné rutiny HTML. Složitější logiku uživatelského rozhraní lze provádět pomocí [zobrazení součásti](xref:mvc/views/view-components). Zobrazení součásti zadejte stejné SoC kterých řadiče a nabídnout zobrazení. Se může eliminovat potřebu akcemi a zobrazeními, které pracují s daty používá společné prvky uživatelského rozhraní.

Například mnoho dalších aspektů ASP.NET Core, zobrazení podporu [vkládání závislostí](xref:fundamentals/dependency-injection), povolení services [vloženy do zobrazení](xref:mvc/views/dependency-injection).
