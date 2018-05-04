Nahraďte obsah *Controllers/HelloWorldController.cs* následujícím kódem:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Každý `public` metoda v kontroleru je možné volat jako koncový bod HTTP. V ukázce výše obě metody vrátí řetězec.  Poznámka: před každou metodu komentáře.

Koncový bod HTTP je targetable adresy URL ve webové aplikaci, například `http://localhost:1234/HelloWorld`a kombinuje protokol použitý: `HTTP`, umístění v síti webového serveru (včetně TCP port): `localhost:1234` a cílový identifikátor URI `HelloWorld`.

První komentář stavy, jedná se [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) metody, která je volána připojením "/HelloWorld/" pro základní adresu URL. Určuje druhý komentář [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) metody, která je volána připojením "/ HelloWorld/Vítá /" na adresu URL. Později v tomto kurzu použijete modul generování uživatelského rozhraní pro generování `HTTP POST` metody.

Spuštění aplikace v režimu bez ladění a připojte "HelloWorld" na cestu v panelu Adresa. `Index` Metoda vrátí řetězec.

![Okno prohlížeče zobrazující odpověď aplikací tohoto objektu je můj výchozí akci.](../../tutorials/first-mvc-app/adding-controller/_static/hell1.png)

Rozhraní MVC volá třídy controller (a metody akce v nich), v závislosti na adresy URL příchozích. Výchozí hodnota [směrování URL logic](xref:mvc/controllers/routing) používá MVC používá tento formát pro určení kódu, která bude vyvolána:

`/[Controller]/[ActionName]/[Parameters]`

Nastavení formátu pro směrování v `Configure` metoda v *Startup.cs* souboru.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Při spuštění aplikace a useanymigrationsubnet všechny segmenty adres URL, výchozí hodnota řadičem "Domů" a "Index" metoda zadaný v řádku šablony uvedených výše.

První segment adresy URL určuje třídy kontroleru ke spuštění. Proto `localhost:xxxx/HelloWorld` se mapuje `HelloWorldController` třídy. Druhá část adresy URL, který určuje metody akce v třídě. Proto `localhost:xxxx/HelloWorld/Index` by způsobilo `Index` metodu `HelloWorldController` třídy ke spuštění. Všimněte si, že jste museli vyhledejte `localhost:xxxx/HelloWorld` a `Index` byla volána metoda ve výchozím nastavení. Důvodem je, že `Index` je výchozí metodou, který bude volán na řadič, pokud není výslovně zadán název metody. Třetí součást segment adresy URL ( `id`) je pro data trasy. Zobrazí se data trasy později v tomto kurzu.

Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda spustí a vrátí řetězec "Toto je metoda úvodní akce...". Pro tuto adresu URL, že je řadič `HelloWorld` a `Welcome` je metoda akce. Nebyly použity `[Parameters]` součástí ještě adresu URL.

![Okno prohlížeče zobrazující odpověď aplikací tohoto objektu je metoda úvodní akce](../../tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Upravte kód, který předat některé informace o parametrech z adresy URL kontroleru. Například `/HelloWorld/Welcome?name=Rick&numtimes=4`. Změna `Welcome` tak, aby zahrnoval dva parametry, jak je znázorněno v následujícím kódu. 

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Předchozí kód:

* Používá funkci volitelný parametr C# k označení, že `numTimes` parametr výchozí hodnotu 1, pokud pro tento parametr není předána žádná hodnota.
* Používá`HtmlEncoder.Default.Encode` k ochraně aplikace před zlými úmysly vstup (konkrétně JavaScript). 
* Používá [interpolované řetězce](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).

Spuštění aplikace a přejděte do:

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Nahraďte xxxx vaše číslo portu.) Můžete použít různé hodnoty pro `name` a `numtimes` v adrese URL. MVC [model vazby](xref:mvc/models/model-binding) systému automaticky mapuje pojmenované parametry z řetězce dotazu v panelu Adresa parametry ve své metodě. V tématu [vazby modelu](xref:mvc/models/model-binding) Další informace.

![Okno prohlížeče zobrazující odpověď aplikace z Hello Rick, je NumTimes: 4](../../tutorials/first-mvc-app/adding-controller/_static/rick4.png)

Na obrázku výš, segment adresy URL (`Parameters`) se nepoužívá, `name` a `numTimes` parametry se jí předávají jako [řetězce dotazu](https://wikipedia.org/wiki/Query_string). `?` (Otazník) v výše uvedenou adresu URL je oddělovač a postupujte podle řetězce dotazu. `&` Odděluje řetězce dotazu.

Nahraďte `Welcome` metoda následujícím kódem:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Spusťte aplikaci a zadejte následující adresu URL:  `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![Okno prohlížeče zobrazující odpověď aplikace z Hello Rick, ID: 3](../../tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

Tentokrát třetí segment adresy URL odpovídá parametru trasy `id`. `Welcome` Metoda obsahuje parametr `id` odpovídající zadaným šablony adresa URL `MapRoute` metoda. Konci `?` (v `id?`) označuje `id` parametr je nepovinný.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

V těchto příkladech řadičem má byla to část "VC" MVC – to znamená, zobrazení a kontroler fungovat. Řadičem přímo vrací HTML. Obecně nechcete, aby řadiče vrácení HTML přímo, vzhledem k tomu, který se stane velmi náročná code a údržbu. Místo toho můžete obvykle použít samostatný soubor šablony zobrazení syntaxe Razor při generování odpovědi HTML. Můžete to udělat v dalším kurzu.
