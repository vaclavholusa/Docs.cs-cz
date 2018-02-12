---
title: "Ověření modelu v aplikaci ASP.NET MVC jádra"
author: rachelappel
description: "Další informace o ověření modelu v aplikaci ASP.NET MVC jádra."
manager: wpickett
ms.author: riande
ms.date: 12/18/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/validation
ms.openlocfilehash: e2911adcfa3a203a06bdae106499994671a055c4
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2018
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a>Úvod k ověření modelu v aplikaci ASP.NET MVC jádra

Podle [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Úvod k ověření modelu

Než se aplikace ukládá data do databáze, aplikace musí ověřit data. Data se musí kontrolovat potenciální bezpečnostní hrozby ověřili, že je správně naformátovaný podle typu a velikosti a musí odpovídat pravidel. Ověření je nutné, i když může být redundantní a zdlouhavé pro implementaci. Ověření dochází v MVC, klient i server.

Naštěstí má .NET abstrahované ověření do atributů ověření. Tyto atributy obsahovat kód pro ověření, a snižují množství kód, který musíte napsat.

[Zobrazení nebo stažení ukázky z webu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Atributy ověření

Atributy ověření představují způsob, jak nakonfigurovat ověření modelu tak, aby byl koncepčně podobá ověření na pole v tabulkách databáze. To zahrnuje omezení, jako je například přiřazení datových typů nebo povinná pole. Ostatní typy ověření, které zahrnuje použití vzorce k datům a vynutit obchodních pravidel, jako je například platební karty, telefonní číslo nebo e-mailovou adresu. Atributy ověření proveďte vynucování tyto požadavky mnohem jednodušší a snadněji použitelná.

Níže je poznámkami `Movie` modelu z aplikace, která uchovává informace o filmy a televizní pořady. Většinu vlastností jsou vyžadované a několik vlastností řetězec mít požadavky na délku. Kromě toho je číselný rozsah omezení nastavené pro `Price` vlastnost od 0 do $999,99, spolu s vlastní ověřovací atribut.

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

Pravidla týkající se dat pro tuto aplikaci, usnadnit zachování kód jednoduše čtení prostřednictvím modelu zjistí. Níže jsou několik atributů oblíbených integrované ověření:

* `[CreditCard]`: Ověří vlastnost má formát platební karty.

* `[Compare]`: Ověří dvě vlastnosti v modelu shody.

* `[EmailAddress]`: Ověří vlastnost má formát e-mailu.

* `[Phone]`: Ověří vlastnost má formát telefonu.

* `[Range]`: Ověří spadá hodnotu vlastnosti v daném rozsahu.

* `[RegularExpression]`: Ověří, že data odpovídá zadanému regulárnímu výrazu.

* `[Required]`: Provede vyžaduje vlastnost.

* `[StringLength]`: Ověří, že ve vlastnosti string. má nejvíce dané maximální délku.

* `[Url]`: Ověří vlastnost má formát adresy URL.

MVC podporuje všechny atributy, které je odvozena z `ValidationAttribute` pro účely ověření. Mnoho atributů užitečné ověření najdete v [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) oboru názvů.

Mohou existovat instance, kde je nutné více funkcí než nabízejí integrované atributy. Pro tyto dobu, můžete vytvořit vlastního ověřování atributy odvozené z `ValidationAttribute` nebo změna modelu k implementaci `IValidatableObject`.

## <a name="notes-on-the-use-of-the-required-attribute"></a>Poznámky k použití požadovaný atribut

Hodnotu Null [typů hodnot](/dotnet/csharp/language-reference/keywords/value-types) (například `decimal`, `int`, `float`, a `DateTime`) jsou ze své podstaty potřeba a nepotřebujete `Required` atribut. Aplikace provádí žádné kontroly ověřování na straně serveru pro použití hodnot Null typy, které jsou označené `Required`.

Vazby modelu MVC, která není problémem ověření a atributů ověření, odmítne odeslání formuláře pole obsahující chybějící hodnoty nebo prázdných znaků pro hodnotu Null typu. Chybí `BindRequired` atributu pro vlastnost target vazby modelu ignoruje chybějící data pro použití hodnot Null typy, kde chybí pole formuláře z příchozích dat formuláře.

[BindRequired atribut](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (viz také [přizpůsobit chování vazby modelu s atributy](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) je užitečné pro zajištění dokončení dat formuláře. Při použití na vlastnost, systém vazba modelu vyžaduje hodnotu pro tuto vlastnost. Při použití typu, systém vazba modelu vyžaduje hodnoty pro všechny vlastnosti tohoto typu.

Při použití [Nullable\<T > typ](/dotnet/csharp/programming-guide/nullable-types/) (například `decimal?` nebo `System.Nullable<decimal>`) a označte ji `Required`, se provádí kontrolu ověření na straně serveru, jako kdyby měla vlastnost standardní typ s možnou hodnotou Null (pro Příklad, `string`).

Ověřování na straně klienta vyžaduje hodnotu pro pole formuláře, který odpovídá vlastnosti modelu, které jste označili `Required` a pro vlastnost neumožňující hodnotu Null typu, které nejsou označeny jako `Required`. `Required`slouží k řízení chybové zprávy ověření na straně klienta.

## <a name="model-state"></a>Stav modelu

Stav modelu, který představuje chyb při ověřování v odeslaných hodnot formuláře HTML.

MVC bude pokračovat až dosáhnou ověřování polí maximálního počtu chyb (200 ve výchozím nastavení). Toto číslo můžete nakonfigurovat vložením následující kód do `ConfigureServices` metoda v *Startup.cs* souboru:

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>Stav modelu zpracování chyb

Před každou akci kontroleru volané dojde k ověření modelu a metoda akce odpovídá kontrola `ModelState.IsValid` a náležitě reagovat. V mnoha případech je vhodné reakce vrátit chybnou odpověď, ideálně s podrobnostmi o důvod, proč se nezdařilo ověření modelu.

Některé aplikace vybere podle standardní konvence pro řešení chyb při ověřování modelu, ve kterých může být případ filtr příslušné místo pro implementaci tato zásada. Měli byste otestovat chování vaše akce s stavů modelu platný a je neplatný.

## <a name="manual-validation"></a>Ruční ověřování

Po dokončení se vazby modelu a ověření, můžete opakovat části. Například uživatele zadali text v poli očekává celé číslo, nebo budete muset výpočtu hodnoty pro vlastnosti modelu.

Musíte ručně spusťte ověření. Chcete-li tak učinit, zavolejte `TryValidateModel` metoda, jak je vidět tady:

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Vlastního ověřování

Atributy ověření fungovat pro většinu potřeb ověření. Některé ověřovací pravidla jsou však specifické pro vaši firmu. Pravidla nemusí být běžné techniky ověření dat, jako je zajištění pole je vyžadován, nebo že vyhovuje rozsah hodnot. Atributy vlastního ověřování pro tyto scénáře jsou vynikající řešení. Vytváření vlastních atributů vlastní ověření v MVC je snadné. Právě dědí `ValidationAttribute`a přepsat `IsValid` metoda. `IsValid` Metoda přijímá dva parametry, je první objekt s názvem *hodnotu* a druhá `ValidationContext` objekt s názvem *validationContext*. *Hodnota* odkazuje na skutečnou hodnotu z pole, které je vlastní validátor ověřování.

V následující ukázce stavy obchodní pravidlo, že uživatelé nemusí nastavená genre na *Classic* pro film vydanou po 1960. `[ClassicMovie]` Atribut nejprve hledá genre, a pokud je klasický, pak zkontroluje datum vydání a zkontrolujte, že je novější než 1960. Pokud vydání po 1960, ověření se nezdaří. Atribut přijme parametrem celé číslo představující rok, můžete ověřit data. Hodnota parametru v konstruktoru atributu můžete zaznamenat, jak je vidět tady:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

`movie` Proměnné výše představuje `Movie` objekt, který obsahuje data z odeslání formuláře k ověření. V tomto případě kód ověření ověří datum a genre v `IsValid` metodu `ClassicMovieAttribute` třída podle pravidla. Po úspěšném ověření `IsValid` vrátí `ValidationResult.Success` kód, a když se ověřování nezdaří, `ValidationResult` s chybovou zprávou. Když uživatel změní `Genre` pole a formulář odešle, `IsValid` metodu `ClassicMovieAttribute` bude ověřte, zda se na video klasický. Podobně jako všechny předdefinované atribut použít `ClassicMovieAttribute` vlastnosti, jako třeba `ReleaseDate` zajistit, ověření se stane, jak je znázorněno v předchozí ukázce kódu. Vzhledem k tomu, že v příkladu pracuje pouze s `Movie` typy, lepší možností je používat `IValidatableObject` jak je znázorněno v následujícím odstavci.

Alternativně může umístit tento stejný kód v modelu tím, že implementujete `Validate` metodu `IValidatableObject` rozhraní. Při ověřování vlastní atributy fungují dobře u ověřování jednotlivé vlastnosti, implementace `IValidatableObject` lze použít k implementaci ověření na úrovni třídy, jak je vidět tady.

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>Ověřování na straně klienta

Ověřování na straně klienta je velmi užitečný pro uživatele. Ho šetří čas, které by jinak věnovat čekání na dobu odezvy na server. V obchodní podmínky dokonce i pár zlomků sekund násobí stokrát každý den přidá až velké množství času, náklady a před. Snadný a okamžitou ověření umožňuje uživatelům vyšší efektivity práce a vytvořit lepší kvalitu vstup a výstup.

Zobrazení s správné odkazům na skript JavaScript musí mít zavedené pro ověřování na straně klienta pro práci jak kterou tady vidíte.

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

[JQuery Nerušivý ověření](https://github.com/aspnet/jquery-validation-unobtrusive) skript je vlastní front-end knihovna Microsoft, který je založený na oblíbených [jQuery ověřením](https://jqueryvalidation.org/) modulu plug-in. Bez jQuery Nerušivý ověření, budete muset code stejnou ověřovací logiku na dvou místech: jednou v atributech straně ověření serveru na vlastnosti modelu a poté znovu v skripty na straně klienta (příklady pro architekturu jQuery ověřit na [ `validate()` ](https://jqueryvalidation.org/validate/) metoda ukazuje, jak komplexní to může být). Místo toho MVC [značky Pomocníci](xref:mvc/views/tag-helpers/intro) a [pomocné objekty HTML](xref:mvc/views/overview) mohli použít atributy ověření a metadata z vlastnosti modelu k vykreslení HTML 5 typu [atributy datového](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) v elementy form vyžadující ověření. Generuje MVC `data-` atributy pro předdefinované a vlastní atributy. jQuery Nerušivý ověření pak analyzuje tez `data-` atributy a předá logiku jQuery ověřením, efektivně "kopírování" logiku ověření straně serveru do klienta. Můžete zobrazit chyby ověření na straně klienta pomocí pomocné rutiny relevantní značky, jak je vidět tady:

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Výše uvedené značky Pomocníci vykreslení HTML níže. Všimněte si, že `data-` atributy v kódu HTML výstup odpovídají atributů ověření pro `ReleaseDate` vlastnost. `data-val-required` Atribut níže obsahuje chybovou zprávu, pokud uživatel není vyplnit pole Datum verze se má zobrazit. jQuery Nerušivý ověření předá tuto hodnotu jQuery ověřením [ `required()` ](https://jqueryvalidation.org/required-method/) metodu, která pak zobrazí zprávy z odpovídajícího  **\<span >** element.

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

Ověřování na straně klienta brání odeslání, dokud formulář je platný. Tlačítko pro odeslání spustí JavaScript, která buď formulář odešle, nebo zobrazí chybové zprávy.

MVC určuje na základě typu dat .NET vlastnosti, které by mohly mít přepsat pomocí hodnoty atributu typu `[DataType]` atributy. Základní `[DataType]` atribut nemá žádné skutečných serverovou ověření. Prohlížeče zvolte vlastní chybové zprávy a zobrazit tyto chyby, jako si přejí, ale ověření Nerušivého balíček jQuery můžete přepsat zprávy a zobrazit konzistentní s ostatními. To se stane, když uživatelé nejvíce samozřejmě použít `[DataType]` podtřídy jako `[EmailAddress]`.

### <a name="add-validation-to-dynamic-forms"></a>Přidávání ověření do dynamické formulářů

Protože jQuery Nerušivý ověření předá logiku ověření a parametry jQuery ověřením při prvním načtení stránky, nebude dynamicky generovaném forms automaticky vykazovat ověření. Místo toho se musí zjistit jQuery Nerušivý ověření analyzovat dynamické formuláře ihned po jeho vytvoření. Například následující kód ukazuje, jak může nastavit ověřování na straně klienta ve formuláři přidané prostřednictvím AJAX.

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

`$.validator.unobtrusive.parse()` Metoda přijímá jQuery selektor pro jeho jeden argument. Tato metoda informuje jQuery Nerušivý ověření analyzovat `data-` atributy formulářů v rámci tohoto selektoru. Hodnoty těchto atributů jsou předána do modulu plug-in jQuery ověřit tak, aby formuláře jádro vykazuje ověřovací pravidla klienta požadované straně.

### <a name="add-validation-to-dynamic-controls"></a>Přidání ověření do dynamických ovládacích prvků

Můžete také aktualizovat pravidla ověřování ve formuláři, pokud jednotlivé prvky, jako například `<input/>`s a `<select/>`s, jsou generována dynamicky. Nelze předat selektory pro tyto prvky, aby `parse()` metoda přímo protože okolního formulář již byla analyzovat a nebude aktualizovat. Místo toho nejprve odstranit existující data ověření, pak rozboru celý formulář, jak je uvedeno níže:

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a>IClientModelValidator

Můžete vytvořit logiku straně klienta pro vaše vlastní atribut a [nerušivý ověření](http://jqueryvalidation.org/documentation/) se spustí na klienta pro vás automaticky jako součást ověření. Prvním krokem je řídí, jaké atributy datového přidají implementací `IClientModelValidator` rozhraní, jak je vidět tady:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

Atributy, které toto rozhraní implementovat můžete přidat atributy HTML generovaného pole. Zkoumání výstup `ReleaseDate` element zjistí HTML, který je podobný na předchozí příklad, s výjimkou teď není `data-val-classicmovie` atribut, který byl definován v `AddValidation` metodu `IClientModelValidator`.

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

Nerušivý ověření používá data v `data-` atributy, které mají zobrazovat chybové zprávy. Ale jQuery nebude vědět o pravidlech nebo zprávy, dokud je nepřidáte do jQuery na `validator` objektu. To je znázorněno v následujícím příkladu, který přidává metodu s názvem `classicmovie` obsahující vlastního ověřovacího kódu do jQuery `validator` objektu.

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

JQuery má teď informace o spuštění vlastního ověřování jazyka JavaScript, jakož i chybovou zprávu zobrazena v případě, že ověřovací kód vrátí hodnotu false.

## <a name="remote-validation"></a>Vzdálené ověření

Vzdálené ověření je skvělé funkce, která použijte, pokud je nutné ověřit data v klientovi s daty na serveru. Aplikace může například muset ověřte, jestli e-mailu nebo uživatelské jméno je již používán, a je nutné dotaz velké množství dat. Uděláte to tak. Stahování velké nastaví dat pro ověřování jednu nebo několik polí využívá příliš mnoho prostředků. Může také nabízet citlivé informace. Alternativou je vytvořte žádost na operace round-trip pro ověření pole.

Vzdálené ověření můžete implementovat ve dvou krocích. Nejdřív musíte označit modelu pomocí `[Remote]` atribut. `[Remote]` Atribut přijímá několik přetížení, které můžete použít k přímé JavaScript na straně klienta pro příslušný kód volat. Následující příklad ukazuje na `VerifyEmail` metody akce `Users` řadiče.

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

Druhým krokem je uvedení ověřovacího kódu v odpovídající metodu akce, jak jsou definovány v `[Remote]` atribut. Podle jQuery ověřením [ `remote()` ](https://jqueryvalidation.org/remote-method/) metoda dokumentaci:

> Serverside odpovědi musí být řetězec formátu JSON, který musí být `"true"` pro platné prvky a může být `"false"`, `undefined`, nebo `null` neplatný elementy, používá výchozí chybová zpráva. Pokud odpověď serverside je řetězec, např. `"That name is already taken, try peter123 instead"`, zobrazí se tento řetězec jako vlastní chybová zpráva místo výchozího.

Definice `VerifyEmail()` metoda řídí následujícími pravidly, jak je uvedeno níže. Vrátí chybu ověření zpráv, pokud nedojde k e-mailu, nebo `true` Pokud e-mailu je zdarma a zabalí výsledek v `JsonResult` objektu. Na straně klienta pak můžete použít vrácená hodnota pokračovat nebo zobrazit chyba v případě potřeby.

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

Nyní když uživatelé zadají e-mailu, JavaScript v zobrazení umožňuje vzdálené volání zda že e-mailové byla přijata, a pokud ano, zobrazí se chybová zpráva. Uživatel, jinak můžete odeslat formuláře jako obvykle.

`AdditionalFields` Vlastnost `[Remote]` atribut je užitečný pro ověření kombinace pole s daty na serveru. Například pokud `User` modelu z výše měl dva další vlastnosti názvem `FirstName` a `LastName`, můžete chtít ověřit, že žádné stávající uživatelé již mají tuto dvojici názvy. Můžete definovat nové vlastnosti, jak je znázorněno v následujícím kódu:

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

`AdditionalFields`může jste explicitně nastavena na řetězce `"FirstName"` a `"LastName"`, ale pomocí [ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) operátor takto zjednodušuje refaktoring později. Metody akce k provedení ověření musí přijměte dva argumenty, jeden pro hodnotu `FirstName` a jeden pro hodnotu `LastName`.

[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

Nyní když uživatelé zadat název první a poslední, JavaScript:

* Díky vzdálené volání chcete zobrazit, pokud je už zabraný této dvojice názvů.
* Pokud byly převzaty dvojici, se zobrazí chybová zpráva. 
* Pokud nebyla provedena, uživatel odešle formulář.

Pokud budete muset ověřit minimálně dva další pole s `[Remote]` atribut jim poskytujete jako seznam oddělený čárkami. Chcete-li například přidat `MiddleName` nastavena pro model `[Remote]` atributu, jak je znázorněno v následujícím kódu:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, jako jsou všechny argumenty atributu musí být konstantní výraz. Proto nesmí používat [interpolované řetězce](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings) nebo volání [ `string.Join()` ](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) k chybě při inicializaci `AdditionalFields`. Pro každý další pole, která přidáte do `[Remote]` atribut, musíte přidat jiné argument odpovídající metoda akce kontroleru.
