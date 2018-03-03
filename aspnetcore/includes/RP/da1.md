# <a name="update-the-generated-pages"></a><span data-ttu-id="dda72-101">Aktualizovat generovaného stránky</span><span class="sxs-lookup"><span data-stu-id="dda72-101">Update the generated pages</span></span>

<span data-ttu-id="dda72-102">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dda72-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dda72-103">Máme správné spuštění na filmová aplikace, ale není ideální prezentaci.</span><span class="sxs-lookup"><span data-stu-id="dda72-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="dda72-104">Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být **datum vydání** (dvě slova).</span><span class="sxs-lookup"><span data-stu-id="dda72-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="dda72-106">Aktualizace generovaný kód</span><span class="sxs-lookup"><span data-stu-id="dda72-106">Update the generated code</span></span>

<span data-ttu-id="dda72-107">Otevřete *Models/Movie.cs* souboru a přidejte následující kód ukazuje zvýrazněné řádky:</span><span class="sxs-lookup"><span data-stu-id="dda72-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
