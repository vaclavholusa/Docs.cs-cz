---
title: Reference syntaxe Razor pro ASP.NET Core
author: rick-anderson
description: Další informace o syntaxi Razor kód pro vložení kódu na serveru do webové stránky.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/razor
ms.openlocfilehash: 224c855b355b8ecde36377bba6966edec251af6a
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962489"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a>Reference syntaxe Razor pro ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylora MÜLLENA](https://twitter.com/ntaylormullen), a [Vicarel Dana](https://github.com/Rabadash8820)

Syntaxe Razor je syntaxe kód pro vložení kódu na serveru do webové stránky. Syntaxe Razor se skládá z Razor značek, C# a HTML. Soubory obsahující Razor obecně *.cshtml* příponu souboru.

## <a name="rendering-html"></a>Vykreslování HTML

Výchozí jazyk Razor jsou ve formátu HTML. Vykreslování HTML z kódu Razor je nejsou jiné než vykreslování protokolu HTML ze souboru HTML. Značka jazyka HTML v *.cshtml* soubory Razor je vykreslen metodou serveru beze změny.

## <a name="razor-syntax"></a>Syntaxe Razor

Syntaxe Razor podporuje C# a používá `@` symbol, který má přechod z HTML a C#. Syntaxe Razor vyhodnotí výrazy jazyka C# a vykreslí je ve výstupním formátu HTML.

Když `@` následuje symbol [Razor vyhrazené – klíčové slovo](#razor-reserved-keywords), přechází do kódu Razor specifické. Jinak přejde do prostý C#.

Řídicí `@` symbolů v kódu Razor, použijte druhý `@` symbol:

```cshtml
<p>@@Username</p>
```

Kód je vykreslena ve formátu HTML s jedním `@` symbol:

```html
<p>@Username</p>
```

Atributy HTML a obsah, který obsahuje e-mailové adresy nejsou s pracovat `@` symbol jako znak přechodu. E-mailové adresy v následujícím příkladu jsou nezměněné pomocí syntaxe Razor analýza:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Implicitní výrazy Razor

Implicitní výrazy Razor začínat `@` následuje kód C#:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

S výjimkou jazyka C# `await` – klíčové slovo, implicitní výrazy nesmí obsahovat mezery. Pokud příkaz jazyka C# zrušte ukončuje, můžete intermingled mezery:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Implicitní výrazy **nelze** obsahovat obecnými typy C#, jako znaků v závorkách (`<>`) se interpretují jako značky jazyka HTML. Následující kód je **není** platné:

```cshtml
<p>@GenericMethod<int>()</p>
```

Předchozí kód vygeneruje chybu kompilátoru podobně jako jednu z těchto možností:

 * Element "int" nebyla uzavřena. Všechny elementy musí být buď samoobslužné zavírání nebo koncová značka.
 *  Metoda skupiny 'GenericMethod' bez delegátem typu 'objekt' nelze převést. Opravdu chcete vyvolat metodu? " 
 
Volání obecné metody musí být uzavřen do [explicitní výraz Razor](#explicit-razor-expressions) nebo [blok kódu Razor](#razor-code-blocks).

## <a name="explicit-razor-expressions"></a>Explicitní výrazy Razor

Explicitní výrazy Razor obsahovat `@` symbol s vyrovnáváním závorky. K vykreslení čas poslední týden, se používá následující syntaxe Razor kód:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Veškerý obsah v rámci `@()` závorky je vyhodnocena a vykresleny výstup.

Implicitní výrazy, které jsou popsané v předchozí části, obecně nesmí obsahovat mezery. V následujícím kódu není jeden týden odečten od aktuální čas:

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

Vykreslí kód HTML následující:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Explicitní výrazy můžete použít ke zřetězení textu pomocí výsledek výrazu:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Bez explicitního výrazu `<p>Age@joe.Age</p>` je považován za e-mailovou adresu a `<p>Age@joe.Age</p>` je vykreslen. Když k zapsání jako výraz explicitní `<p>Age33</p>` je vykreslen.

Explicitní výrazy můžete použít k vykreslení výstupu z obecné metody v *.cshtml* soubory. Následující kód ukazuje, jak chybu opravit chyby, zobrazí dříve způsobené hranatých závorkách obecného C#. Kód je zapsána jako explicitní výrazu:

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>Výraz kódování

Jsou výrazy jazyka C#, která se vyhodnotí jako řetězec kódovaný jazykem HTML. Výrazy jazyka C#, která se vyhodnotí jako `IHtmlContent` vykreslují přímo přes `IHtmlContent.WriteTo`. C# výrazů, které nejsou vyhodnocení `IHtmlContent` jsou převedeno na řetězec pomocí `ToString` a kódováním než jejich se zobrazí.

```cshtml
@("<span>Hello World</span>")
```

Vykreslí kód HTML následující:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

HTML je zobrazena v prohlížeči jako:

```
<span>Hello World</span>
```

`HtmlHelper.Raw` výstup není kódovaný ale vykresleno jako značka jazyka HTML.

> [!WARNING]
> Pomocí `HtmlHelper.Raw` unsanitized uživatelský vstup je bezpečnostní riziko. Uživatelský vstup může obsahovat jiné zneužití nebo škodlivý JavaScript. Úpravě uživatelský vstup je obtížné. Nepoužívejte `HtmlHelper.Raw` se vstupem uživatele.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Vykreslí kód HTML následující:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Bloky kódu Razor

Bloky kódu Razor začínat `@` a jsou uvedeny v `{}`. Na rozdíl od výrazy není vykreslen kódu C# do bloků kódu. Bloky kódu a výrazy v zobrazení sdílejí stejný obor a jsou definovány v pořadí:

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

Vykreslí kód HTML následující:

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>Implicitní přechody

Je výchozí jazyk v bloku kódu C#, ale stránky Razor můžete přejít zpět do formátu HTML:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Explicitní přechod s oddělovači

K definování část bloku kódu, který by měl vykreslení HTML, uzavřete znaků pro vykreslování s syntaxi Razor  **\<text >** značky:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Tuto metodu použijte k vykreslení HTML, který není obklopená značky jazyka HTML. Bez značku HTML nebo Razor nastane běhová chyba syntaxe Razor.

**\<Text >** značka je vhodné pro řízení prázdných znaků při vykreslování obsahu:

* Pouze obsah, mezi  **\<text >** je vykreslen. 
* Žádné prázdné před nebo po  **\<text >** značky se zobrazí ve výstupu protokolu HTML.

### <a name="explicit-line-transition-with-"></a>Explicitní řádek přechodu se @:

K vykreslení zbytek celý řádek jako kód HTML uvnitř bloku kódu, použijte `@:` syntaxe:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Bez `@:` v kódu, je generována chyba runtime Razor.

Upozornění: Navíc `@` znaky v souboru nástroje Razor mohou způsobit chyby kompilátoru na příkazy v bloku později. Tyto chyby kompilátoru může být obtížné zjistit, protože dojde k chybě skutečné před ohlášené chyby. Tato chyba je běžné po kombinování více implicitního nebo explicitního výrazů do jednoho kód bloku.

## <a name="control-structures"></a>Řídicí struktury

Řídicí struktury, jsou rozšíření bloky kódu. Všechny aspekty bloky kódu (Přechod na kód, vložené C#) také použít následující struktury:

### <a name="conditionals-if-else-if-else-and-switch"></a>Podmíněné příkazy @if, pokud jiný, #else a @switch

`@if` ovládací prvky při spuštění kódu:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else` a `else if` nevyžadují `@` symbol:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

Následující kód ukazuje, jak používat příkaz switch:

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>Ve smyčce @for, @foreach, @while, a @do při

Použitím šablon HTML, které lze vykreslit s opakování ve smyčce řídicí příkazy. K vykreslení seznam lidí, kteří:

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

Podporovány jsou následující příkazy opakování:

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>Složené @using

V jazyce C# `using` příkaz slouží k zajištění uvolnění objektu. V prostředí Razor shodný mechanismus slouží k vytvoření pomocné rutiny HTML, které obsahují další obsah. V následujícím kódu pomocné rutiny HTML, vykreslení značku formuláře s `@using` příkaz:


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

Lze provést akce na úrovni oboru [značky Pomocníci](xref:mvc/views/tag-helpers/intro).

### <a name="try-catch-finally"></a>@try, nakonec catch

Zpracovávání výjimek v jazyce je podobná C#:

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Syntaxe Razor má možnost chránit kritické oddíly s příkazy uzamčení:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Komentáře

Syntaxe Razor podporuje komentáře jazyka C# a HTML:

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

Vykreslí kód HTML následující:

```html
<!-- HTML comment -->
```

Komentáře syntaxe Razor serverem jsou odebrány, než se zobrazí webová stránka. Používá syntaxi Razor `@*  *@` pro vymezení komentáře. Následující kód je označeno jako komentář, tak server nemá vykreslení žádné značky:

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a>Direktivy

Direktivy Razor jsou reprezentované pomocí implicitní výrazy s následující vyhrazená slova `@` symbol. Direktivu obvykle mění způsob zobrazení je analyzována nebo jinou funkci povolí.

Pochopení, jak Razor generuje kód pro zobrazení usnadňuje pochopili, jak funguje direktivy.

[!code-html[](razor/sample/Views/Home/Contact8.cshtml)]

Generuje kód třídu podobný následujícímu:

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

Dále v tomto článku v části [zobrazení třídy Razor C# vytvořené pro zobrazení](#viewing-the-razor-c-class-generated-for-a-view) vysvětluje, jak zobrazit tento vygenerované třídy.

<a name="using"></a>
### <a name="using"></a>@using

`@using` – Direktiva přidá jazyka C# `using` direktivy generované zobrazení:

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

`@model` – Direktiva Určuje typ modelu předána do zobrazení:

```cshtml
@model TypeNameOfModel
```

V aplikaci ASP.NET MVC základní vytvořené pomocí jednotlivých uživatelských účtů *Views/Account/Login.cshtml* zobrazení obsahuje následující prohlášení modelu:

```cshtml
@model LoginViewModel
```

Vygenerované třídy dědí z `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Zpřístupní Razor `Model` vlastnost pro přístup k modelu předaná do zobrazení:

```cshtml
<div>The Login Email: @Model.Email</div>
```

`@model` – Direktiva Určuje typ této vlastnosti. Direktiva Určuje `T` v `RazorPage<T>` že generované třídy, zobrazení je odvozena z. Pokud `@model` – direktiva není zadán, `Model` vlastnost je typu `dynamic`. Hodnota modelu se z řadiče předaná do zobrazení. Další informace najdete v tématu [silného typu modely a &commat;modelu – klíčové slovo](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).

### <a name="inherits"></a>@inherits

`@inherits` – Direktiva poskytuje úplné řízení třídy dědí zobrazení:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Následující kód je vlastní typ stránky Razor:

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText` Se zobrazí v zobrazení:

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

Vykreslí kód HTML následující:

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model` a `@inherits` lze použít ve stejném zobrazení. `@inherits` může být v *_ViewImports.cshtml* soubor, který importuje zobrazení:

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

Následující kód je příkladem zobrazení silného typu:

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

Pokud "rick@contoso.com" je předán v modelu zobrazení generuje následující kód HTML:

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


`@inject` – Direktiva umožňuje vložit služby z této stránky Razor [kontejneru služby](xref:fundamentals/dependency-injection) do zobrazení. Další informace najdete v tématu [vkládání závislostí do zobrazení](xref:mvc/views/dependency-injection).

### <a name="functions"></a>@functions

`@functions` – Direktiva povolí Přidat blok kódu jazyka C# do zobrazení Razor stránky:

```cshtml
@functions { // C# Code }
```

Příklad:

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

Kód generuje následující kód HTML:

```html
<div>From method: Hello</div>
```

Následující kód je generovaná třída Razor C#:

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

`@section` – Direktiva se používá ve spojení s [rozložení](xref:mvc/views/layout) umožnit zobrazení k vykreslení obsahu v různých částech stránky HTML. Další informace najdete v tématu [části](xref:mvc/views/layout#layout-sections-label).

## <a name="tag-helpers"></a>Pomocníci značky

Existují tři direktivy, které se týkají [značky Pomocníci](xref:mvc/views/tag-helpers/intro).

| – Direktiva | Funkce |
| --------- | -------- |
| [&commat;addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Zpřístupní Pomocníci značky k zobrazení. |
| [&commat;removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Odebere značky Pomocníci dříve přidány ze zobrazení. |
| [&commat;tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Určuje předponu značky, chcete-li povolit podporu pomocníků značky a vytvoření značky pomocná využití explicitní. |

## <a name="razor-reserved-keywords"></a>Klíčová slova Razor vyhrazena

### <a name="razor-keywords"></a>Klíčová slova Razor

* stránka (vyžaduje základní technologie ASP.NET 2.0 a novější)
* – obor názvů
* – funkce
* dědí
* model
* section
* pomocné rutiny (aktuálně se nepodporuje ASP.NET Core)

Klíčová slova Razor jsou uvozeny zpětným `@(Razor Keyword)` (například `@(functions)`).

### <a name="c-razor-keywords"></a>Klíčová slova jazyka C# Razor

* case
* do
* default
* pro
* foreach
* if
* else
* lock
* – přepínač
* Zkuste
* catch
* finally
* používání
* while

Klíčová slova jazyka C# Razor musí být znaky s `@(@C# Razor Keyword)` (například `@(@case)`). První `@` řídicí sekvence analyzátor Razor. Druhý `@` řídicí sekvence analyzátor jazyka C#.

### <a name="reserved-keywords-not-used-by-razor"></a>Vyhrazená slova, která nepoužívá Razor

* třída

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Zobrazení třídy Razor C# vytvořené pro zobrazení

Přidejte následující třídu do projektu ASP.NET MVC jádra:

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

Přepsání `RazorTemplateEngine` přidal MVC s `CustomTemplateEngine` třídy:

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

Nastavit bod přerušení na `return csharpDocument` prohlášení o `CustomTemplateEngine`. Při spuštění programu zastavení okamžiku přerušení, zobrazit hodnotu `generatedCode`.

![Zobrazení textu vizualizér generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>Zobrazení vyhledávání a rozlišování velkých a malých písmen

Zobrazovací modul Razor provede malá a velká písmena vyhledávání pro zobrazení. Nicméně skutečné vyhledávání je dáno podkladový systém souborů:

* Na základě zdrojového souboru: 
  * V operačních systémech se systémy souborů malá a velká písmena (například Windows) jsou fyzický soubor poskytovatele vyhledávání malá a velká písmena. Například `return View("Test")` výsledkem shod */Views/Home/Test.cshtml*, */Views/home/test.cshtml*a všechny ostatní variant velká a malá písmena.
  * V systémech souborů s malá a velká písmena (například Linux, OSX a s `EmbeddedFileProvider`), hledání se malá a velká písmena. Například `return View("Test")` konkrétně odpovídá */Views/Home/Test.cshtml*.
* Předkompilované zobrazení: základní technologie ASP.NET 2.0 a vyšší, vyhledávání předkompilovaných zobrazení je rozlišování malých a velkých písmen pro všechny operační systémy. Toto chování je stejné chování zprostředkovatele fyzického souboru v systému Windows. Pokud se dvě předkompilovaných zobrazení liší pouze v případě, výsledek vyhledávání je Nedeterministický.

Vývojáři se doporučuje tak, aby odpovídaly malá a velká písmena názvů souborů a adresářů na malá a velká písmena systému:

    * Názvy oblasti, kontroleru a akce. 
    * Stránky Razor.
    
Přiřazení případ zajišťuje, že nasazení najít jejich zobrazení bez ohledu na podkladový systém souborů.
