Následující tabulka Podrobnosti ASP.NET Core kódu generátory parametry:

| Parametr               | Popis|
| ----------------- | ------------ |
| -m  | Název modelu. |
| -dc  | Data kontextu. |
| -udl | Použijte výchozí rozložení. |
| -outDir | Relativní výstupní cesta ke složce vytvoření zobrazení. |
| --referenceScriptLibraries | Přidá `_ValidationScriptsPartial` upravovat a vytvářet stránky |

Použití `h` přepínač tak, aby získejte pomoc na `aspnet-codegenerator razorpage` příkaz:

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a>Testování aplikace

* Spusťte aplikaci a připojit `/Movies` na adresu URL v prohlížeči (`http://localhost:port/movies`).
* Testovací **vytvořit** odkaz.

  ![Vytvoření stránky](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Testovací **upravit**, **podrobnosti**, a **odstranit** odkazy.

Pokud se zobrazí podobná následující chyby, ověřte, jestli mají spuštění migrace a aktualizovat databázi:

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
