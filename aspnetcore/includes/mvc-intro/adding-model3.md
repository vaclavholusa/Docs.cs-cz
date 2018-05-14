
## <a name="test-the-app"></a>Testování aplikace

* Spusťte aplikaci a klepněte **Mvc film** odkaz.
* Klepněte **vytvořit nový** propojit a vytvořit film.

  ![Vytvoření zobrazení v polích pro genre, ceny, datum vydání a název](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* Nelze zadat desetinných míst nebo čárkami v `Price` pole. Pro podporu [k ověřování jQuery](https://jqueryvalidation.org/) pro neanglická národní prostředí, které používají čárkou (",") pro desetinné čárky a formát data neanglických USA, musíte provést kroky globalizace aplikace. V tématu https://github.com/aspnet/Docs/issues/4076 a [další prostředky](#additional-resources) Další informace. Teď zadejte jenom celá čísla, jako je 10.

<a name="displayformatdatelocal"></a>

* V některých národní prostředí budete muset zadat formát data. Viz následující zvýrazněný kód.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

Budeme mluvit o `DataAnnotations` dál v tomto kurzu.

Klepnutím **vytvořit** způsobí, že formulář odeslat na server, kde film informace se ukládají do databáze. Aplikace provede přesměrování */Movies* adresu URL, kde se zobrazují informace nově vytvořený film.

![Výpis film zobrazení zobrazující nově vytvořený filmy](../../tutorials/first-mvc-app/adding-model/_static/h.png)

Vytvořte několik další film položky. Zkuste **upravit**, **podrobnosti**, a **odstranit** odkazy, které jsou všechny funkční.
