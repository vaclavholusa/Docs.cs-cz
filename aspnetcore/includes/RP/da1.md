# <a name="update-the-generated-pages"></a>Aktualizovat generované stránky

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Máme o dobrý začátek aplikace movie, ale v prezentaci není ideální. Nechceme zobrazíte čas (12:00:00 dop. na následujícím obrázku) a **ReleaseDate** by měl být **datum vydání** (dvě slova).

![Otevřít v prohlížeči Chrome ukazující data o filmech aplikace Movie](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizace generovaného kódu

Otevřít *Models/Movie.cs* a přidejte zvýrazněné řádky je znázorněno v následujícím kódu:

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
