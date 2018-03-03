# <a name="update-the-generated-pages"></a>Aktualizovat generovaného stránky

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Máme správné spuštění na filmová aplikace, ale není ideální prezentaci. Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být **datum vydání** (dvě slova).

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizace generovaný kód

Otevřete *Models/Movie.cs* souboru a přidejte následující kód ukazuje zvýrazněné řádky:

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
