# <a name="working-with-sqlite-in-and-razor-pages"></a>Práce s SQLite v a stránky Razor

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

`MovieContext` Objekt zpracovává úlohu s připojením k databázi a mapování `Movie` objekty záznamy v databázi. Kontext databáze není zaregistrována [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru v `ConfigureServices` metoda v *Startup.cs* souboru:

[!code-csharp[Main](code/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a>SQLite

[SQLite](https://www.sqlite.org/) webu stavy:

> SQLite je samostatná, vysoká spolehlivost, embedded, plně funkční, veřejné domény, databázový stroj SQL. SQLite je nejpoužívanější databázový stroj na světě.

Existuje mnoho nástroje třetích stran, si můžete stáhnout spravovat a zobrazovat databáze SQLite. Následující obrázek je z [DB prohlížeče pro SQLite](http://sqlitebrowser.org/). Pokud máte nástroj Oblíbené SQLite, co se vám líbí o něm ponechte komentář.

![DB prohlížeče pro SQLite zobrazující film db](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Počáteční hodnoty databáze

Vytvořte novou třídu s názvem `SeedData` v *modely* složky. Generovaného kódu nahraďte následujícím textem:

[!code-csharp[Main](code\Models\SeedData.cs)]

Pokud jsou všechny filmy v databázi, vrátí inicializátoru počáteční hodnoty.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Přidat inicializátoru počáteční hodnoty

Přidat inicializátoru počáteční hodnoty do `Main` metoda v *Program.cs* souboru:

[!code-csharp[Main](../../tutorials/razor-pages\razor-pages-start\sample\RazorPagesMovie\Program.cs)]

### <a name="test-the-app"></a>Testování aplikace

Odstraňte všechny záznamy v databázi (tak, aby metoda počáteční hodnoty spuštěno). Zastavení a spuštění aplikace počáteční hodnoty databáze.

Aplikace zobrazuje dosazená data.