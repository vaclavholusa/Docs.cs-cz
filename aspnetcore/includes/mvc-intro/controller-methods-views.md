
Budeme se zabývat těmito tématy [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) v dalším kurzu. [Zobrazit](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) atribut určuje, co má být zobrazen pro název pole (v tomto případě "Datum vydání" místo "ReleaseDate"). [Datový typ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atribut určuje typ dat (datum), a proto není zobrazit čas informace uložené v poli.

Vyhledejte `Movies` řadiče a podržte ukazatel myši nad **upravit** odkaz zobrazíte cílová adresa URL.

![Okno prohlížeče s myši přes odkaz pro úpravy a odkaz Adresa Url http://localhost:1234 nebo filmy/Edit/5 jsou uvedené.](../../tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

**Upravit**, **podrobnosti**, a **odstranit** základní MVC ukotvení značky Pomocník generované odkazy *Views/Movies/Index.cshtml* souboru.

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

[Pomocné rutiny značky](xref:mvc/views/tag-helpers/intro) umožňují, aby se kód na straně serveru v souborech Razor podílel na vytváření a vykreslování prvků HTML. Ve výše, kódu `AnchorTagHelper` dynamicky vygeneruje HTML `href` hodnota atributu z id metoda a směrování akce kontroleru. Používáte **zobrazit zdroj** z oblíbeném prohlížeči nebo použijte nástrojů pro vývojáře prozkoumat vygenerovaný kód. Část generovaný kód jazyka HTML, je zobrazena níže:

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

Odvolat formát [směrování](xref:mvc/controllers/routing) nastavit *Startup.cs* souboru:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Přeloží ASP.NET Core `http://localhost:1234/Movies/Edit/4` do žádost, aby `Edit` metody akce `Movies` řadiče s parametrem `Id` 4. (Metody kontroleru jsou také známé jako metody akce.)

[Značka pomocné rutiny](xref:mvc/views/tag-helpers/intro) jsou jednou z nejčastěji používané nových funkcí v ASP.NET Core. V tématu [další prostředky](#additional-resources) Další informace.

Otevřete `Movies` řadiče a prozkoumat dva `Edit` metody akce. Následující kód ukazuje `HTTP GET Edit` metodu, která načte film a naplní formuláře upravit vygenerované *Edit.cshtml* souboru nástroje Razor.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

Následující kód ukazuje `HTTP POST Edit` metoda, která zpracovává odeslaných film hodnoty:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

`[Bind]` Atribut je jedním ze způsobů pro ochranu proti [typu overpost](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost). Vlastnosti v by měl obsahovat jenom `[Bind]` atribut, který chcete změnit. V tématu [chránit řadiči z přečerpání příspěvků](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application) Další informace. [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) poskytnout alternativní způsob zabráníte přečerpání účtování.

Všimněte si, druhý `Edit` předchází metody akce `[HttpPost]` atribut.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]

`HttpPost` Atribut určuje, že to `Edit` může být volána metoda *pouze* pro `POST` požadavky. Je možné aplikovat `[HttpGet]` atribut prvního upravit metodu, ale není nutné protože `[HttpGet]` je výchozí.

`ValidateAntiForgeryToken` Atribut se používá ke [ochraně před paděláním požadavku](xref:security/anti-request-forgery) a je spárován k tokenu proti zfalšování vygenerované v souboru zobrazení upravit (*Views/Movies/Edit.cshtml*). Upravit zobrazení souboru vygeneruje token proti padělání s [pomocná značku formuláře](xref:mvc/views/working-with-forms).

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

[Pomocná značku formuláře](xref:mvc/views/working-with-forms) generuje skryté token proti padělání, který se musí shodovat `[ValidateAntiForgeryToken]` vygenerovat token proti padělání v `Edit` metoda filmy řadiče. Další informace najdete v tématu [požadavek proti padělání](xref:security/anti-request-forgery).

`HttpGet Edit` Metoda přebírá film `ID` parametr vyhledává film používající rozhraní Entity Framework `SingleOrDefaultAsync` metoda a vrátí vybraného videa na zobrazení pro úpravy. Pokud nelze nalézt film, `NotFound` (HTTP 404) je vrácen.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

Při generování uživatelského rozhraní systému vytvoření zobrazení pro úpravy, zkontrolován `Movie` třídy a vytvořený kód k vykreslení `<label>` a `<input>` prvky pro každou vlastnost třídy. Následující příklad ukazuje zobrazení úpravy, který byl vytvořen v sadě Visual Studio generování uživatelského rozhraní systému:

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/EditCopy.cshtml?highlight=1)]

Všimněte si, jak se má zobrazit šablonu `@model MvcMovie.Models.Movie` příkaz v horní části souboru. `@model MvcMovie.Models.Movie` Určuje, že zobrazení očekává, že model pro zobrazení šablony být typu `Movie`.

Automaticky generovaný kód používá několik značky pomocné metody k zjednodušení kód HTML. - [Popisek značky pomocná](xref:mvc/views/working-with-forms) zobrazí název pole ("Title", "ReleaseDate", "Genre" nebo "Cena"). [Vstupní značka pomocná](xref:mvc/views/working-with-forms) vykreslí HTML `<input>` elementu. [Pomocná rutina pro ověření značky](xref:mvc/views/working-with-forms) zobrazí všechny zprávy ověření přidružené k této vlastnosti.

Spusťte aplikaci a přejděte do `/Movies` adresy URL. Klikněte na tlačítko **upravit** odkaz. V prohlížeči zobrazte zdroj pro stránku. Generovaný kód HTML pro `<form>` element jsou uvedeny níže.

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

`<input>` Prvky jsou v `HTML <form>` element jejichž `action` k vystavování příspěvků na nastavený atribut `/Movies/Edit/id` adresy URL. Data formuláře budou odeslány na server při `Save` po kliknutí na tlačítko. Poslední řádek před uzavírací `</form>` ukazuje skrytého elementu [XSRF](xref:security/anti-request-forgery) token vygenerované [pomocná značku formuláře](xref:mvc/views/working-with-forms).

## <a name="processing-the-post-request"></a>Zpracování požadavku POST

Následující seznam uvádí `[HttpPost]` verzi `Edit` metody akce.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

`[ValidateAntiForgeryToken]` Ověří Atribut skrytého [XSRF](xref:security/anti-request-forgery) token generované generátor token proti padělání v [pomocná značku formuláře](xref:mvc/views/working-with-forms)

[Model vazby](xref:mvc/models/model-binding) systém přijímá hodnoty odeslaného formuláře a vytvoří `Movie` objekt, který se předá jako `movie` parametr. `ModelState.IsValid` Metoda ověří, že odeslaná data do formuláře lze použít k úpravě (upravit nebo aktualizace) `Movie` objektu. Pokud je platná data jsou uložené. Aktualizované (upravená) film data budou uložena do databáze při volání `SaveChangesAsync` metoda kontext databáze. Po uložení dat, kód přesměruje uživatele `Index` metody akce `MoviesController` třídy, která zobrazuje kolekce film, včetně právě změny.

Před formuláře je odeslána na server, ověřování na straně klienta kontroluje všechna pravidla ověřování na pole. Pokud nejsou žádné chyby ověření, zobrazí se chybová zpráva a není odeslání formuláře. Pokud JavaScript je vypnuta, nebude mít ověřování na straně klienta, ale server zjistí odeslaných hodnot, které nejsou platné a hodnot formuláře se zobrazí znovu s chybové zprávy. Později v tomto kurzu jsme zkontrolujte [ověření modelu](xref:mvc/models/validation) podrobněji. [Pomocná rutina pro ověření značky](xref:mvc/views/working-with-forms) v *Views/Movies/Edit.cshtml* zobrazení šablony postará zobrazení příslušné chybové zprávy.

![Upravit zobrazení: výjimku pro nesprávnou hodnotu ceny abc stavy, že pole cena musí být číslo. Výjimka pro nesprávnou hodnotu Datum vydání xyz stavů prosím zadejte platné datum.](../../tutorials/first-mvc-app/controller-methods-views/_static/val.png)

Všechny `HttpGet` metody v kontroleru film podle podobný Princip. Získají objekt film (nebo seznam objektů, v případě `Index`) a předat objekt (modelu) do zobrazení. `Create` Metoda předá objekt prázdný film tak, aby `Create` zobrazení. Takže v udělat všechny metody, které vytvořit, upravit, odstranit nebo jinak měnit data `[HttpPost]` přetížení metody. Úprava dat v `HTTP GET` metoda představuje bezpečnostní riziko. Úprava dat v `HTTP GET` metoda také porušuje HTTP osvědčené postupy a architektury [REST](http://rest.elkstein.org/) vzor, který určuje, že požadavky GET nesmí změnit stav vaší aplikace. Jinými slovy provádění operace GET by měl být bezpečné operace, která nemá žádné vedlejší účinky a nezmění trvalá data.

## <a name="additional-resources"></a>Další zdroje

* [Globalizace a lokalizace](xref:fundamentals/localization)
* [Úvod do pomocné rutiny značky](xref:mvc/views/tag-helpers/intro)
* [Vytváření Pomocníci značky](xref:mvc/views/tag-helpers/authoring)
* [Ochrana proti padělání požadavků](xref:security/anti-request-forgery)
* Chránit řadiči z [typu overpost](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)
* [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [Pomocník značku formuláře](xref:mvc/views/working-with-forms)
* [Pomocník vstupní značky](xref:mvc/views/working-with-forms)
* [Pomocník značky popisek](xref:mvc/views/working-with-forms)
* [Vyberte značku pomocné rutiny](xref:mvc/views/working-with-forms)
* [Pomocná rutina pro ověření značky](xref:mvc/views/working-with-forms)
