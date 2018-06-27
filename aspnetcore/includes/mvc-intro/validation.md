# <a name="add-validation-to-an-aspnet-core-mvc-app"></a>Přidejte ověřování do aplikace ASP.NET MVC jádra

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V této části přidáte logiku ověření pro `Movie` modelu a budete ujistěte, že ověřovací pravidla se uplatňují vždy, když uživatel vytvoří nebo upraví film.

## <a name="keeping-things-dry"></a>Zachování věcí suchého

Jeden z návrhu principů MVC je [SUCHÝCH](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("nemáte opakujte sami"). ASP.NET MVC umožňuje zadejte funkce nebo chování jenom jednou a potom ho v aplikaci projeví everywhere mít. To snižuje množství kódu, které potřebujete k zápisu a umožňuje kód, který menší chyba náchylné k chybám, usnadnění testování a jednodušší správu zápisu.

Podpora ověřování poskytované MVC a Entity Framework Core Code First je dobrým příkladem SUCHÝCH Princip v akci. V jednom místě (ve třídě modelu) můžete určit deklarativně ověřovací pravidla a pravidla jsou vynucována everywhere v aplikaci.

## <a name="adding-validation-rules-to-the-movie-model"></a>Přidávání pravidel ověřování modelu filmu

Otevřete *Movie.cs* souboru. DataAnnotations poskytuje integrovanou sadu atributů ověření, které se vztahují deklarativně všechny třídy nebo vlastnost. (Také obsahuje formátování atributů, například `DataType` který usnadní formátování a neposkytují žádné ověření.)

Aktualizace `Movie` třída využít předdefinované `Required`, `StringLength`, `RegularExpression`, a `Range` atributů ověření.

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]
::: moniker-end

Atributy ověření zadejte chování, které chcete vynutit ve vlastnostech modelu, který jste použili na. `Required` a `MinimumLength` atributy znamená, že vlastnost musí mít hodnotu; ale nic zabrání zadat mezer splňovat toto ověření uživatele. `RegularExpression` Atribut se používá k omezení znaků, může být vstup. Ve výše, kódu `Genre` a `Rating` musí používat pouze písmena (první písmeno velké, bílé mezery, číslice a speciální znaky nejsou povoleny). `Range` Atribut omezuje hodnota v zadaném rozsahu. `StringLength` Atribut umožňuje nastavit maximální délka ve vlastnosti string a volitelně jeho minimální délka. Typů hodnot (například `decimal`, `int`, `float`, `DateTime`) jsou ze své podstaty potřeba a nepotřebujete `[Required]` atribut.

S ověřovacích pravidel automaticky vynucuje technologie ASP.NET pomáhá zkontrolujte robustnější aplikace. Také zajistí, že nelze zapomenete a ověřit něco nechtěně umožní chybná data do databáze.

## <a name="validation-error-ui-in-mvc"></a>Chyba ověření uživatelského rozhraní v MVC

Spusťte aplikaci a přejděte na filmy kontroleru.

Klepněte **vytvořit nový** odkaz na přidání nové videa. Vyplňte formulář s některé neplatné hodnoty. Jakmile jQuery ověřování na straně klienta zjistí chybu, zobrazí chybovou zprávu.

![Film zobrazit formulář s více chyby ověření klienta straně jQuery](~/tutorials/first-mvc-app/validation/_static/val.png)

> [!NOTE]
> Nemusí být možné je zadat desetinné čárky ve `Price` pole. Pro podporu [k ověřování jQuery](https://jqueryvalidation.org/) pro neanglická národní prostředí, které používají čárkou (",") pro desetinné čárky a formát data neanglických USA, musíte provést kroky globalizace aplikace. To [potíže Githubu 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) postup pro přidání desetinnou čárkou. 

Všimněte si, jak formulář automaticky vykreslí příslušné ověření chybovou zprávu do jednotlivých polí, která obsahuje neplatnou hodnotu. Chyby se vynucují (pomocí jazyka JavaScript a jQuery) klientské a serverové (v případě, že uživatel, který má JavaScript zakázaná).

Významné výhodou je, že nemám potřebu změnit jeden řádek kódu `MoviesController` třídy nebo v *Create.cshtml* zobrazení, aby bylo možné povolit toto ověření uživatelského rozhraní. Řadič a zobrazení, které jste vytvořili dříve v tomto kurzu automaticky zachyceny až ověření pravidla, kterou jste zadali pomocí atributů ověření ve vlastnostech `Movie` třída modelu. Test ověření pomocí `Edit` metody akce a stejné ověření platí.

Data formuláře není odesílat na server, dokud nejsou žádné chyby ověření straně klienta. Můžete to ověřit umístěním přerušení `HTTP Post` metodu, pomocí [nástroj Fiddler](http://www.telerik.com/fiddler) , nebo [nástroje pro vývojáře F12](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).

## <a name="how-validation-works"></a>Jak funguje ověření

Může vás zajímat, jak byl vygenerován ověření uživatelské rozhraní bez jakýchkoli aktualizací kód v kontroleru nebo zobrazení. Následující kód ukazuje dva `Create` metody.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

První (HTTP GET) `Create` metoda akce zobrazí počáteční vytvořit formulář. Druhý (`[HttpPost]`) verze zpracovává post formuláře. Druhý `Create` – metoda ( `[HttpPost]` verze) volání `ModelState.IsValid` zkontrolujte, zda video má všechny chyby ověření. Voláním této metody vyhodnotí všechny atributy ověřování, které byly použity k objektu. Pokud objekt má chyby ověření, `Create` metoda znovu zobrazí formulář. Pokud nejsou žádné chyby, metoda nové film uloží v databázi. V našem příkladu film formuláře není odeslána na server po ověření chyb zjištěných na straně klienta; druhý `Create` metoda je volána nikdy, když jsou chyby ověření straně klienta. Pokud zakážete JavaScript v prohlížeči, ověření klienta je zakázán a můžete otestovat HTTP POST `Create` metoda `ModelState.IsValid` zjišťování všechny chyby ověření.

Můžete nastavit bod přerušení v `[HttpPost] Create` metoda a ověřte nikdy volání metody, ověřování na straně klienta nebude odesílat data formuláře, pokud jsou zjištěny chyby ověřování. Pokud zakázat JavaScript v prohlížeči a pak odeslání formuláře s chybami, se setkají místo přerušení. Se stále objevuje úplného ověření bez JavaScript. 

Následující obrázek ukazuje, jak zakázat JavaScript v prohlížeči FireFox.

![Firefox:, Zrušte zaškrtnutí políčka Povolit Javascript na kartě Možnosti v obsahu.](~/tutorials/first-mvc-app/validation/_static/ff.png)

Následující obrázek ukazuje, jak zakázat JavaScript v prohlížeči Chrome.

![Google Chrome: Část v Javascript v nastavení obsahu a vyberte zakázat libovolnou lokalitu spouštět JavaScript.](~/tutorials/first-mvc-app/validation/_static/chrome.png)

Po zakázání JavaScript post neplatná data a projděte ladicího programu.

![Při ladění na post neplatných dat, Intellisense v ModelState.IsValid ukazuje, že je hodnota false.](~/tutorials/first-mvc-app/validation/_static/ms.png)

Níže uvádíme část *Create.cshtml* zobrazit šablonu, která vygeneroval dříve v tomto kurzu. Použije se uvedené výše obě metody akce k zobrazení původního formuláře a znovu ji zobrazit v případě chyby.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]

[Vstupní značka pomocná](xref:mvc/views/working-with-forms) používá [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atributů a atributů HTML, které jsou potřebné pro architekturu jQuery ověření na straně klienta vytváří. [Pomocná rutina pro ověření značky](xref:mvc/views/working-with-forms#the-validation-tag-helpers) zobrazí chyby ověření. V tématu [ověření](xref:mvc/models/validation) Další informace.

Co je skutečně dobrý o tento přístup je, že ani řadičem ani `Create` zobrazit šablonu zná nic o skutečné ověřovacích pravidel vynucován nebo o specifické chybové zprávy zobrazují. Ověřovací pravidla a řetězce chyby se zadávají pouze v `Movie` třídy. Tyto stejné ověřovací pravidla budou automaticky použita pro `Edit` zobrazení a jakékoli další zobrazení šablony můžete vytvořit které upravit modelu.

Pokud potřebujete změnit logiku ověření, můžete tak učinit na jednom místě přidáním atributů ověření modelu (v tomto příkladu `Movie` třídy). Nebudete mít starat o různých částech aplikace je nekonzistentní s jak se vynucují pravidla – veškerou logiku ověřování definované na jednom místě se použije everywhere. To zajišťuje kód velmi vyčištění a umožňuje snadno spravovat a momentální. . To znamená, že je budete mít plně ctít zásady SUCHÝCH Princip.

## <a name="using-datatype-attributes"></a>Pomocí atributů datový typ

Otevřete *Movie.cs* soubor a zkontrolujte `Movie` třídy. `System.ComponentModel.DataAnnotations` Obor názvů poskytuje atributy formátování kromě integrovanou sadu atributů ověření. Provedli jsme již `DataType` Výčtová hodnota, datum vydání a pole cena. Následující kód ukazuje `ReleaseDate` a `Price` vlastnosti s příslušnou `DataType` atribut.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` Atributy pouze poskytovat pro modul zobrazení pro zobrazení dat (a poskytuje elementy nebo atributy, jako `<a>` pro adresy URL a `<a href="mailto:EmailAddress.com">` e-mailu. Můžete použít `RegularExpression` atribut pro ověření formátu dat. `DataType` Atribut slouží k určení datový typ, který je specifičtější než vnitřní typ databáze, že nejste atributů ověření. V tomto případě chceme jenom udržování přehledu o datum, není čas. `DataType` Výčtu poskytuje pro mnoho typů dat, jako je například datum, čas, telefonní číslo, měny, EmailAddress a další. `DataType` Atributu můžete také povolit aplikace automaticky poskytnout konkrétní typ funkce. Například `mailto:` může vytvořit odkaz pro `DataType.EmailAddress`, a datum selektor lze zadat pro `DataType.Date` v prohlížečích podporujících HTML5. `DataType` Atributy vysílá standardu HTML 5 `data-` (výrazný data dash) atributy, které můžete porozumět standardu HTML 5 prohlížeče. `DataType` Atributy provést **není** žádné ověřování.

`DataType.Date` neuvádí formát data, které se zobrazí. Ve výchozím nastavení, zobrazí se pole dat podle výchozích formátů podle serveru `CultureInfo`.

`DisplayFormat` Atribut slouží k explicitnímu zadání formát data:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Nastavení určuje, že formátování, které také bude použito při hodnota je zobrazena v textovém poli pro úpravy. (Který není vhodné pro některá pole – například hodnoty měny, pravděpodobně nechcete symbolu měny do textového pole pro úpravy.)

Můžete použít `DisplayFormat` atribut podle sám sebe, ale obecně je vhodné používat `DataType` atribut. `DataType` Atribut přenese tak sémantika data a jak vykreslit ho na obrazovce a nabízí následující výhody, které není dostupná s DisplayFormat:

* V prohlížeči můžete povolit funkce HTML5 (např. k zobrazení ovládacího prvku kalendář, symbolu měny vhodné národního prostředí, e-mailu odkazy, atd.)

* Ve výchozím nastavení bude v prohlížeči vykreslovat data ve správném formátu podle národního prostředí.

* `DataType` Atributu můžete povolit MVC na výběr šablony pravé pole k poskytnutí dat ( `DisplayFormat` pokud používá samotné používá šablonu řetězec).

> [!NOTE]
> k ověřování jQuery nefunguje s `Range` atribut a `DateTime`. Například následující kód bude vždy zobrazovat chyby ověření straně klienta, i v případě, že datum je v zadaném rozsahu:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

Budete muset zakázat ověřování jQuery datum používat `Range` atribut s `DateTime`. Je obecně není vhodné zkompilovat pevného data v modely, takže pomocí `Range` atribut a `DateTime` se nedoporučuje.

Následující kód ukazuje kombinování atributy na jeden řádek:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

V další části řady, jsme budete zkontrolujte, zda aplikace a některá vylepšení pro automaticky generované `Details` a `Delete` metody.

## <a name="additional-resources"></a>Další zdroje

* [Práce s formuláři](xref:mvc/views/working-with-forms)
* [Globalizace a lokalizace](xref:fundamentals/localization)
* [Úvod do pomocné rutiny značky](xref:mvc/views/tag-helpers/intro)
* [Autor značky pomocné rutiny](xref:mvc/views/tag-helpers/authoring)
