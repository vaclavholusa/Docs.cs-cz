<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Předchozí `Index` metoda:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

Aktualizovaný `Index` metoda s `id` parametr:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

Název hledání můžete nyní předat jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.

![Index zobrazení s neodstraněných Wordu přidat adresu Url a vrácený film seznam dvou filmy, Ghostbusters a Ghostbusters 2](~/tutorials/first-mvc-app/search/_static/g2.png)

Nelze však budou uživatelé chcete upravit adresu URL pokaždé, když chtějí hledat film. Proto nyní přidáte prvky uživatelského rozhraní na jejich filtrovat filmy. Pokud jste změnili podpis `Index` metoda k testování jak předat trasy vázané `ID` nastavte zpátky tak, aby trvá parametr s názvem, parametr `searchString`:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

Otevřete *Views/Movies/Index.cshtml* souboru a přidejte `<form>` značek zvýrazněná níže:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

HTML `<form>` značku používá [pomocná značku formuláře](xref:mvc/views/working-with-forms), takže při odesílání formuláře řetězec filtru je odeslána do `Index` akce filmy řadiče. Uložte změny a pak testování filtru.

![Index zobrazení s neodstraněných word zadali do textového pole Název filtru](~/tutorials/first-mvc-app/search/_static/filter.png)

Neexistuje žádné `[HttpPost]` přetížení z `Index` metoda, jak jste očekávali. Není nutné, protože metoda není změny stavu aplikace a právě filtrování dat.

Můžete přidat následující `[HttpPost] Index` metoda.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

`notUsed` Parametr se používá k vytvoření přetížení pro `Index` metoda. Budeme mluvit o který později v tomto kurzu.

Pokud chcete přidat tuto metodu, původce volání akce odpovídá `[HttpPost] Index` metody a `[HttpPost] Index` metoda by spustit jak je znázorněno na obrázku níže.

![Okno prohlížeče s odpovědí aplikace z indexu HttpPost: na neodstraněných filtr](~/tutorials/first-mvc-app/search/_static/fo.png)

Ale i v případě, přidejte tuto `[HttpPost]` verzi `Index` metoda, existuje omezení v tom, jak to všechny byl implementován. Představte si, že chcete bookmark konkrétní hledání nebo chcete poslat odkaz přátel, chcete-li zobrazit stejný filtrovaný seznam filmy mohou klepnutím. Všimněte si, že adresa URL požadavku HTTP POST je stejný jako adresu URL pro požadavek GET (localhost:xxxxx nebo filmy nebo Index) – chybí informace o vyhledávání v adrese URL. Hledání řetězce informace jsou odeslány na server jako [formuláři hodnota pole](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data). Můžete ověřit, která pomocí nástrojů pro vývojáře prohlížeče nebo vynikající [nástroj Fiddler](http://www.telerik.com/fiddler). Následující obrázek ukazuje vývojářské nástroje prohlížeče Chrome:

![Síťové kartě nástrojů pro vývojáře v Microsoft Edge zobrazující textu žádosti s hodnotou searchString neodstraněných](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

Můžete zobrazit parametr vyhledávání a [XSRF](xref:security/anti-request-forgery) tokenu v textu požadavku. Všimněte si, jak je uvedeno v předchozí kurz [pomocná značku formuláře](xref:mvc/views/working-with-forms) generuje [XSRF](xref:security/anti-request-forgery) token proti padělání. Jsme nejsou úpravy dat, takže potřebujeme si ověřit token v metodě řadiče.

Protože parametr vyhledávání je v těle žádosti a nikoliv adresu URL, nemůže zaznamenat tyto informace vyhledávání záložku nebo sdílet s ostatními. Tuto opravu jsme budete udělat tak, že zadáte, by měl být požadavek `HTTP GET`.
