---
title: "Pomocníci značky ve formulářích v ASP.NET Core"
author: rick-anderson
description: "Popisuje předdefinované značky Pomocníci použít s formuláři."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/working-with-forms
ms.openlocfilehash: 805c2ba5b3a9669d5547e1c595883436eea0d11a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a>Základní informace o použití značky Pomocníci ve formulářích v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), a [Jerrie Pelser](https://github.com/jerriep)

Tento dokument ukazuje práci s formuláři a elementů HTML běžně používají na formuláři. HTML [formuláře](https://www.w3.org/TR/html401/interact/forms.html) element poskytuje primární mechanismus webové aplikace využívají k odesílání dat zpět na server. Většina tento dokument popisuje [značky Pomocníci](tag-helpers/intro.md) a jak se vám může pomoct efektivní vytvořit robustní formuláře HTML. Doporučujeme, abyste si přečetli [Úvod do pomocné rutiny značky](tag-helpers/intro.md) před čtením tohoto dokumentu.

V mnoha případech pomocné objekty HTML poskytnout alternativní způsob konkrétní pomocné rutiny značky, ale je důležité vědět, že pomocné rutiny značky není nahradit pomocné rutiny HTML a neexistuje žádná značka Pomocník pro každý pomocné rutiny HTML. Pokud existuje alternativu pomocné rutiny HTML, je uvedený.

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a>Pomocník značku formuláře

[Formuláře](https://www.w3.org/TR/html401/interact/forms.html) značky pomocné rutiny:

* Generuje kód HTML [ \<formuláře >](https://www.w3.org/TR/html401/interact/forms.html) `action` hodnotu atributu pro akce kontroleru MVC nebo pojmenovanou trasu

* Vytváří skrytý [žádosti o ověření tokenu](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) při ochraně před paděláním požadavku posílaného mezi weby (při použití s `[ValidateAntiForgeryToken]` atribut v metodě akce HTTP Post)

* Poskytuje `asp-route-<Parameter Name>` atribut, kde `<Parameter Name>` se přidá do hodnoty trasy. `routeValues` Parametry, které `Html.BeginForm` a `Html.BeginRouteForm` poskytuje podobné funkce.

* Má alternativu pomocné rutiny HTML `Html.BeginForm` a`Html.BeginRouteForm`

Ukázka:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

Pomocné rutiny formuláře značka výše generuje následující HTML:

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

Modul runtime rozhraní MVC vygeneruje `action` hodnotu atributu ze atributů pomocná značku formuláře `asp-controller` a `asp-action`. Pomocník značku formuláře také vytváří skrytý [žádosti o ověření tokenu](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) při ochraně před paděláním požadavku posílaného mezi weby (při použití s `[ValidateAntiForgeryToken]` atribut v metodě akce HTTP Post). Ochrana proti padělání požadavku posílaného mezi weby čistý formuláře HTML je obtížné, pomocná značku formuláře poskytuje tuto službu můžete.

### <a name="using-a-named-route"></a>Pomocí pojmenovanou trasu

`asp-route` Značky pomocný atribut můžete také vygenerovat kód pro kód HTML `action` atribut. Aplikace s [trasy](../../fundamentals/routing.md) s názvem `register` použít následující kód pro stránku registrace:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

Řadu zobrazení *zobrazení nebo účet* složky (vygeneruje, když vytvoříte novou webovou aplikaci s *jednotlivé uživatelské účty*) obsahují [asp. trasy returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) atribut:

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
>Pomocí integrované šablony `returnUrl` je pouze naplněny automaticky při pokusu o přístup k autorizovaným prostředků, ale není ověřený nebo oprávnění. Když zkusíte neoprávněným přístupem, middleware zabezpečení vás přesměruje na přihlašovací stránku s `returnUrl` nastavit.

## <a name="the-input-tag-helper"></a>Pomocník vstupní značky

Váže vstupní značka pomocné rutiny HTML [ \<vstupní >](https://www.w3.org/wiki/HTML/Elements/input) element modelu výrazu v zobrazení syntaxe razor.

Syntaxe:

```HTML
<input asp-for="<Expression Name>" />
```

Pomocník vstupní značky:

* Generuje `id` a `name` atributy HTML pro název výraz zadaný v `asp-for` atribut. `asp-for="Property1.Property2"`je ekvivalentní `m => m.Property1.Property2`. Název výrazu se bude používat pro `asp-for` hodnota atributu. Najdete v článku [názvy výrazů](#expression-names) části Další informace.

* Nastaví HTML `type` atribut hodnotu na základě typu modelu a [datové poznámky](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributy použité na vlastnost modelu

* Nedojde k přepsání HTML `type` hodnota atributu, pokud byl zadán jeden

* Generuje [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) atributy ověření ze [datové poznámky](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributy použité na vlastnosti modelu

* Obsahuje funkci pomocné rutiny HTML, které se překrývají s `Html.TextBoxFor` a `Html.EditorFor`. Najdete v článku **pomocné rutiny HTML alternativy vstupní značka pomocná** podrobnosti.

* Poskytuje silného typování. Pokud název změny vlastností a neaktualizovat pomocná značky budete dojde k chybě podobný následujícímu:

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

`Input` Nastaví značku pomocné rutiny HTML `type` atribut založený na typu .NET. Následující tabulka uvádí některé běžné typy .NET a vygenerovaný typ HTML (ne každý typ formátu .NET je uvedena).

|Typ formátu .NET|Typ vstupu|
|---|---|
|BOOL|type=”checkbox”|
|String|type=”text”|
|DateTime|type=”datetime”|
|Byte|typ = "number"|
|celá čísla|typ = "number"|
|Jednoduché, Double|typ = "number"|


Následující tabulka uvádí některé běžné [datových poznámek](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributy, které pomocné rutiny vstupní značka se mapování na konkrétní typy vstupu (ne každý atribut ověření je uvedena):


|Atribut|Typ vstupu|
|---|---|
|[EmailAddress]|type=”email”|
|[Url]|type=”url”|
|[HiddenInput]|type=”hidden”|
|[Phone]|type=”tel”|
|[DataType(DataType.Password)]| typ = "password"|
|[DataType(DataType.Date)]| type=”date”|
|[DataType(DataType.Time)]| typ = "čas"|


Ukázka:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

Výše uvedený kód generuje následující HTML:

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid email address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

U anotací dat `Email` a `Password` vlastnosti generovat metadata na modelu. Pomocník značky vstup využívá metadat modelu a vytváří [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` atributy (najdete v části [ověření modelu](../models/validation.md)). Tyto atributy popisují validátory pro připojení k vstupních polí. To umožňuje nerušivý HTML5 a [jQuery](https://jquery.com/) ověření. Nerušivý atributy mají formát `data-val-rule="Error Message"`, kde pravidlo je název pravidla ověření (například `data-val-required`, `data-val-email`, `data-val-maxlength`atd.) Pokud atribut je součástí chybovou zprávu, zobrazí se jako hodnota `data-val-rule` atribut. Existují také atributy ve formátu `data-val-ruleName-argumentName="argumentValue"` které poskytují další informace o pravidle, například `data-val-maxlength-max="1024"` .

### <a name="html-helper-alternatives-to-input-tag-helper"></a>Pomocné rutiny HTML alternativy vstupní značka pomocné rutiny

`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` a `Html.EditorFor` překrývajících se funkce s pomocnou rutinou vstupní značka. Pomocné rutiny vstupní značka automaticky nastaví `type` atribut; `Html.TextBox` a `Html.TextBoxFor` nebude. `Html.Editor`a `Html.EditorFor` zpracování kolekcí, komplexní objekty a šablony; není pomocný vstupní značka. Pomocné rutiny vstupní značka, `Html.EditorFor` a `Html.TextBoxFor` jsou silného typu (jejich použití výrazů lambda); `Html.TextBox` a `Html.Editor` nejsou (používají názvy výrazů).

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()`a `@Html.EditorFor()` použít speciální `ViewDataDictionary` položka s názvem `htmlAttributes` při provádění jejich výchozí šablony. Toto chování je volitelně rozšířen pomocí `additionalViewData` parametry. Klíč "htmlAttributes" nerozlišuje velká a malá písmena. Klíč "htmlAttributes" se zpracovávají podobně jako `htmlAttributes` byl předán objekt pomocné rutiny jako vstup `@Html.TextBox()`.

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>Názvy výrazů

`asp-for` Hodnota atributu je `ModelExpression` a pravé straně výrazu lambda. Proto `asp-for="Property1"` stane `m => m.Property1` generovaného kódu, proto nemusíte předpony s `Model`. Můžete použít "@" znak spustit výraz vložené a přesuňte před `m.`:

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

Generuje následující:

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

Pomocí vlastnosti kolekce `asp-for="CollectionProperty[23].Member"` vygeneruje stejný název jako `asp-for="CollectionProperty[i].Member"` při `i` má hodnotu `23`.

### <a name="navigating-child-properties"></a>Navigace vlastnosti podřízené

Můžete také přejít na vlastnosti podřízené pomocí cesty k vlastnosti zobrazení modelu. Vezměte v úvahu složitější třídu modelu, která obsahuje podřízenou `Address` vlastnost.

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

V zobrazení, můžeme vytvořit vazbu na `Address.AddressLine1`:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

Následující kód HTML se generuje pro `Address.AddressLine1`:

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a>Názvy výrazů a kolekce

Ukázkový model obsahující pole `Colors`:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

Metoda akce:

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

Následující syntaxe Razor ukazuje, jak přistupovat k konkrétní `Color` element:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

*Views/Shared/EditorTemplates/String.cshtml* šablony:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

Ukázka použití `List<T>`:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

Následující syntaxe Razor ukazuje, jak iterace v kolekci:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
>Vždy používat `for` (a *není* `foreach`) k iterace v seznamu. Vyhodnocení indexer v dotazu LINQ výraz může být nákladné a by měl být minimální.

&nbsp;

>[!NOTE]
>Výše uvedené komentáři vzorový kód ukazuje, jak by nahraďte výraz lambda s `@` operátor pro přístup k každý `ToDoItem` v seznamu.

## <a name="the-textarea-tag-helper"></a>Pomocník Textarea značky

`Textarea Tag Helper` Pomocná značky je podobná pomocné rutiny vstupní značka.

* Generuje `id` a `name` a atributů ověření dat z modelu pro [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) element.

* Poskytuje silného typování.

* Alternativní pomocné rutiny HTML:`Html.TextAreaFor`

Ukázka:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

Následující kód HTML se vygeneruje:

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a>Pomocník značky popisek

* Generuje popisek popisek a `for` atributu u [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) element pro název výrazu

* Alternativní pomocné rutiny HTML: `Html.LabelFor`.

`Label Tag Helper` Přes čistý element popisku HTML poskytuje následující výhody:

* Automaticky získána hodnota popisné označení `Display` atribut. Určený zobrazovaný název mohou změnit čas a kombinace `Display` atribut a popisek značky pomocná budou platit `Display` všude, kde se používá.

* Menší značek ve zdrojovém kódu

* Silné typování s vlastností modelu.

Ukázka:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

Následující kód HTML se generuje pro `<label>` element:

```HTML
<label for="Email">Email Address</label>
```

Pomocné rutiny značky popisek vygenerované `for` hodnotu atributu "E-mailu", což je Identifikátor přidružený `<input>` elementu. Pomocníci značky generovat konzistentní `id` a `for` elementy, aby mohly být správně přidružena. Titulek v této ukázce pochází z `Display` atribut. Pokud model neobsahovalo `Display` atribut popisek by být název vlastnosti výrazu.

## <a name="the-validation-tag-helpers"></a>Pomocníci značky ověření

Existují dvě značky Pomocníci ověření. `Validation Message Tag Helper` (Které zobrazí zprávu ověření pro jedinou vlastnost v modelu) a `Validation Summary Tag Helper` (zobrazuje souhrn chyb ověřování). `Input Tag Helper` Přidá HTML5 klientské straně ověření atributy jako vstup elementy na základě dat atributy poznámky na třídách modelu. Ověření je také provést na serveru. Pomocná rutina pro ověření značky zobrazí tyto chybové zprávy, když dojde k chybě ověření.

### <a name="the-validation-message-tag-helper"></a>Pomocná rutina pro ověření zprávy značky

* Přidá [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` atribut [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, který připojí chybové zprávy ověření na vlastnosti zadaného modelu vstupní pole. Když dojde k chybě ověření straně klienta, [jQuery](https://jquery.com/) zobrazí chybovou zprávu ve `<span>` elementu.

* Ověření také probíhá na serveru. Klienti mohou mít JavaScript zakázán a některé ověření provést pouze na straně serveru.

* Alternativní pomocné rutiny HTML:`Html.ValidationMessageFor`

`Validation Message Tag Helper` Se používá s `asp-validation-for` atribut HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) elementu.

```HTML
<span asp-validation-for="Email"></span>
```

Pomocná rutina pro ověření zprávy značky se generují následující kód HTML:

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

Obvykle použijete `Validation Message Tag Helper` po `Input` značky Pomocník pro stejnou vlastnost. Díky tomu se zobrazí chybové zprávy ověření téměř vstupu, který chybu způsobil.

> [!NOTE]
> Musíte mít zobrazení s správný jazyk JavaScript a [jQuery](https://jquery.com/) skript odkazy na místě pro ověřování na straně klienta. V tématu [ověření modelu](../models/validation.md) Další informace.

Když se stane Chyba ověření straně serveru (například pokud máte ověřování na straně serveru vlastní nebo je zakázáno ověřování na straně klienta), MVC umístí tuto chybovou zprávu jako text `<span>` elementu.

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>Pomocná rutina pro ověření Summary – značka

* Cíle `<div>` elementů s hodnotou `asp-validation-summary` atribut

* Alternativní pomocné rutiny HTML:`@Html.ValidationSummary`

`Validation Summary Tag Helper` Se používá k zobrazení Souhrn ověřovacích zpráv. `asp-validation-summary` Hodnota atributu může být libovolná z následujících:

|ASP souhrnu ověření|Zobrazí ověřovacích zpráv|
|--- |--- |
|ValidationSummary.All|Vlastnost a model úroveň|
|ValidationSummary.ModelOnly|Model|
|ValidationSummary.None|Žádné|

### <a name="sample"></a>Ukázka

V následujícím příkladu je model dat označených pomocí `DataAnnotation` atributy, které generuje chybové zprávy ověření na `<input>` elementu.  Když dojde k chybě ověření, zobrazí pomocná rutina pro ověření značky chybová zpráva:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

Vygenerovaný HTML (když je model platný):

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a>Vyberte značku pomocné rutiny

* Generuje [vyberte](https://www.w3.org/wiki/HTML/Elements/select) a přidružených [možnost](https://www.w3.org/wiki/HTML/Elements/option) prvky pro vlastnosti modelu.

* Má alternativu pomocné rutiny HTML `Html.DropDownListFor` a`Html.ListBoxFor`

`Select Tag Helper` `asp-for` Určuje název vlastnosti modelu pro [vyberte](https://www.w3.org/wiki/HTML/Elements/select) elementu a `asp-items` Určuje [možnost](https://www.w3.org/wiki/HTML/Elements/option) elementy.  Příklad:

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

Ukázka:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

`Index` Metoda inicializuje `CountryViewModel`, nastaví vybrané země a předává jej do `Index` zobrazení.

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

HTTP POST `Index` metoda zobrazí výběr:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

`Index` Zobrazení:

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

Které generuje následující HTML (písmeny "CA" vybraná):

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> Nedoporučujeme používat `ViewBag` nebo `ViewData` Pomocník vyberte značky. Model zobrazení je robustnější na poskytování metadat MVC a obecně menší problém.

`asp-for` Hodnota atributu je zvláštní případ a nevyžaduje `Model` předpony, další atributy do pomocné rutiny značky (například `asp-items`)

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>Vazba výčtu

Často je možné použít `<select>` s `enum` vlastnost a generovat `SelectListItem` elementy z `enum` hodnoty.

Ukázka:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

`GetEnumSelectList` Metoda generuje `SelectList` objekt výčtu.

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

Můžete uspořádání seznamu enumerátor s `Display` atribut získat bohatší uživatelského rozhraní:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

Následující kód HTML se vygeneruje:

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a>Možnost skupiny

HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) element se vygeneruje, když model zobrazení obsahuje jeden nebo více `SelectListGroup` objekty.

`CountryViewModelGroup` Skupin `SelectListItem` elementy do skupiny "Severní Americe" a "Evropa":

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

Níže jsou uvedeny dvě skupiny:

![Příklad skupiny možnost](working-with-forms/_static/grp.png)

Generovaný kód HTML:

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a>Více vyberte

Značka pomocná vyberte automaticky vygeneruje [více = "více"](http://w3c.github.io/html-reference/select.html) -li zadána vlastnost v atributu `asp-for` atribut je `IEnumerable`. Například uděleno model následující:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

Pomocí následující zobrazení:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

Generuje následující HTML:

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a>Žádná výběru

Pokud se přistihnete pomocí možnosti "nebyl zadán" na více stránkách, můžete vytvořit šablonu k odstranění opakovaných HTML:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

*Views/Shared/EditorTemplates/CountryViewModel.cshtml* šablony:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

Přidání HTML [ \<možnost >](https://www.w3.org/wiki/HTML/Elements/option) není omezen na elementy *žádná výběru* případu. Například následující metodu zobrazení a akce se generují kód HTML podobně jako výše uvedený kód:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

Správný `<option>` element bude vybrána (obsahovat `selected="selected"` atribut) v závislosti na aktuální `Country` hodnota.

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a>Další zdroje

* [Pomocné rutiny značek](xref:mvc/views/tag-helpers/intro)
* [Element formuláře HTML](https://www.w3.org/TR/html401/interact/forms.html)
* [Žádost o ověření tokenu](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* [Vazby modelu](xref:mvc/models/model-binding)
* [Ověření modelu](xref:mvc/models/validation)
* [IAttributeAdapter rozhraní](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [Fragmenty kódu pro tento dokument](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample)
