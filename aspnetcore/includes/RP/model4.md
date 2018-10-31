Následující tabulka obsahuje podrobnosti o parametry generátor kódu ASP.NET Core:

| Parametr               | Popis|
| ----------------- | ------------ |
| -m  | Název modelu. |
| -dc  | Data kontextu. |
| -udl | Použijte výchozí rozložení. |
| -outDir | Relativní cesta k výstupní složce vytvořte zobrazení. |
| --referenceScriptLibraries | Přidá `_ValidationScriptsPartial` upravovat a vytvářet stránky |

Použití `h` přepínače můžete zobrazit nápovědu pro `aspnet-codegenerator razorpage` příkaz:

```console
dotnet aspnet-codegenerator razorpage -h
```

<a name="test"></a>

### <a name="test-the-app"></a>Testování aplikace

* Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/Movies`).
* Test **vytvořit** odkaz.

  ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Test **upravit**, **podrobnosti**, a **odstranit** odkazy.

Pokud se zobrazí chyba podobná následující, ověřte máte spusťte migrace a aktualizovat databázi:

`An unhandled exception occurred while processing the request. 'no such table: Movie'.`
