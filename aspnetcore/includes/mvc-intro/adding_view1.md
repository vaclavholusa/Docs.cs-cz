# <a name="adding-a-view-to-an-aspnet-core-mvc-app"></a>Přidání zobrazení do aplikace ASP.NET MVC jádra

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

V této části upravíte `HelloWorldController` třídu se má použít soubory šablon do této aplikace zapouzdření proces generování odpovědi HTML klientovi zobrazení syntaxe Razor.

Můžete vytvořit zobrazení souboru šablony pomocí syntaxe Razor. Zobrazení syntaxe Razor pro šablony mají *.cshtml* příponu souboru. Poskytují elegantní způsob vytvoření výstupu HTML pomocí jazyka C#.

Aktuálně `Index` metoda vrátí řetězec s zprávu, která je pevně zakódovaná ve třídě controller. V `HelloWorldController` třídy, nahraďte `Index` metoda následujícím kódem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Vrátí předchozí kód `View` objektu. Zobrazit šablonu používá ke generování odpovědi HTML do prohlížeče. Metody kontroleru (také známé jako metody akce), jako `Index` metody popsané výše, obecně vrátit [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) (nebo třídy odvozené od `ActionResult`), není typu jako řetězec.
