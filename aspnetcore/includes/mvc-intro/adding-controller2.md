Nahraďte obsah *Controllers/HelloWorldController.cs* následujícím kódem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Každý `public` metoda v kontroleru je volatelná jako koncový bod HTTP. V příkladu výše obě metody vrací řetězec.  Poznámka: před každou metodu komentáře.

Koncový bod HTTP, jako je targetable adresy URL ve webové aplikaci `http://localhost:1234/HelloWorld`a kombinuje protokol použitý: `HTTP`, umístění v síti webového serveru (včetně TCP port): `localhost:1234` a cílový identifikátor URI `HelloWorld`.

První komentář stavy, jedná se [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) metody, která je volána "/HelloWorld/" připojením k základní adrese URL. Určuje druhý komentář [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) metody, která je volána přidáním "/ HelloWorld/uvítací /" na adresu URL. Později v tomto kurzu použijete modul generování uživatelského rozhraní k vygenerování `HTTP POST` metody.

Spusťte aplikaci v režimu bez ladění a připojit "HelloWorld" na cestu v panelu Adresa. `Index` Metoda vrátí hodnotu typu string.

![Okno prohlížeče zobrazující reakce aplikace na tohoto objektu je Moje výchozí akce](~/tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC volá třídy kontroleru (a metody akce v nich) v závislosti na příchozí adrese URL. Výchozí hodnota [logiku směrování URL](xref:mvc/controllers/routing) používá MVC používá tento formát pro určení kódu, který má být vyvolán:

`/[Controller]/[ActionName]/[Parameters]`

Nastavení formátu pro směrování `Configure` metoda ve *Startup.cs* souboru.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Při spuštění aplikace a nechcete zadat všechny segmenty adres URL, použije se výchozí kontroler "Home" a "Index" metoda podle šablony řádek zvýrazněný výš.

První segment adresy URL určuje třída kontroleru pro spuštění. Takže `localhost:xxxx/HelloWorld` mapuje `HelloWorldController` třídy. Druhá část segment adresy URL určí metodu akce v třídě. Takže `localhost:xxxx/HelloWorld/Index` by způsobilo `Index` metodu `HelloWorldController` má třída spustit. Všimněte si, že jste museli procházet `localhost:xxxx/HelloWorld` a `Index` byla volána metoda ve výchozím nastavení. Důvodem je, že `Index` je výchozí metodou, která bude volána na řadiči, pokud nebyl explicitně zadán název metody. Třetí část segment adresy URL ( `id`) je pro data trasy. Zobrazí se vám data trasy, která dále v tomto kurzu.

Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda spustí a vrátí řetězec "Toto je metoda úvodní akce...". Pro tuto adresu URL kontroleru je `HelloWorld` a `Welcome` je metoda akce. Nebyly použity `[Parameters]` část adresy URL ještě.

![Okno prohlížeče zobrazující reakce aplikace na tohoto objektu je metoda úvodní akce](~/tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Upravte kód předat některé informace o parametrech z adresy URL kontroleru. Například `/HelloWorld/Welcome?name=Rick&numtimes=4`. Změnit `Welcome` tak, aby zahrnoval dva parametry, jak je znázorněno v následujícím kódu. 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Předchozí kód:

* Volitelný parametr funkce jazyka C# používá k označení, že `numTimes` parametr výchozí hodnotu 1, pokud není pro tento parametr předána žádná hodnota.
* Používá`HtmlEncoder.Default.Encode` k ochraně aplikace před zlými úmysly vstup (konkrétně JavaScript). 
* Používá [interpolovaných řetězců](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).

Spusťte aplikaci a přejděte do:

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Nahradit xxxx číslem portu.) Můžete vyzkoušet různé hodnoty pro `name` a `numtimes` v adrese URL. MVC [vazby modelu](xref:mvc/models/model-binding) systém automaticky mapují pojmenované parametry z řetězce dotazu do adresního řádku parametrům ve své metodě. Zobrazit [vazby modelu](xref:mvc/models/model-binding) Další informace.

![Okno prohlížeče zobrazující odpověď aplikace Hello Rick, je NumTimes: 4](~/tutorials/first-mvc-app/adding-controller/_static/rick4.png)

Na obrázku výš, segment adresy URL (`Parameters`) se nepoužívá, `name` a `numTimes` parametry jsou předány jako [řetězce dotazu](https://wikipedia.org/wiki/Query_string). `?` (Otazník) v adrese URL výše je oddělovač a postupujte podle řetězce dotazu. `&` Odděluje řetězce dotazu.

Nahradit `Welcome` metodu s následujícím kódem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Spusťte aplikaci a zadejte následující adresu URL:  `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![Okno prohlížeče zobrazující odpověď aplikace Hello Rick, ID: 3](~/tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

Tentokrát třetí segment adresy URL odpovídající parametr trasa `id`. `Welcome` Metody obsahuje parametr `id` odpovídající šablony do adresy URL `MapRoute` metoda. Koncové `?` (v `id?`) označuje, `id` parametr je nepovinný.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

V těchto příkladech kontroleru je to "VC" část MVC – to znamená, zobrazení a kontroler fungovat. Kontroler přímo vrací HTML. Obecně nechcete řadiče vrácení HTML přímo, protože, který se stane velmi náročný kód a udržovat. Místo toho budete obvykle používat samostatný soubor šablony zobrazení Razor ke generování odpovědi HTML. Můžete to udělat v dalším kurzu.
