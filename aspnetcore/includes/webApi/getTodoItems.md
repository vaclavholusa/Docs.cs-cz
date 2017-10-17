[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Předchozí kód:

* Definuje třídu prázdný kontroler. V následujících částech přidáme metody k implementaci rozhraní API.
* Používá konstruktoru [vkládání závislostí](xref:fundamentals/dependency-injection) vložení kontext databáze (`TodoContext `) do kontroleru. Kontext databáze se používá v každé z [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metody v kontroleru.
* Konstruktor přidá položku do databáze v paměti, pokud neexistuje.

## <a name="getting-to-do-items"></a>Získávání položkami seznamu úkolů

Chcete-li získat položkami seznamu úkolů, přidejte následující metody, které `TodoController` třídy.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Tyto metody implementovat tyto dvě metody GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Zde je odpověď příklad HTTP pro `GetAll` metoda:

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

Později v tomto kurzu I zobrazí, jak můžete zobrazit pomocí odpovědi HTTP [Postman](https://www.getpostman.com/) nebo [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).

### <a name="routing-and-url-paths"></a>Směrování a adresy URL cesty

`[HttpGet]` Atribut určuje metody GET protokolu HTTP. Cesta URL pro každou metodu má následující formát:

* Proveďte řetězec šablony v atributu kontroleru trasy:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Nahraďte název kontroleru, což je název třídy kontroleru minus příponou "Controller" "[Controller]". Tato ukázka je název třídy kontroleru **Todo**řadiče a názvu kořenové je "úkolů". ASP.NET Core [směrování](xref:mvc/controllers/routing) není velká a malá písmena.
* Pokud `[HttpGet]` atribut má šablonu trasy (například `[HttpGet("/products")]`, která připojení k cestě. Tato ukázka nepoužívá šablony. V tématu [atribut směrování s atributy Http [akce]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) Další informace.

V `GetById` metoda:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

`"{id}"`je proměnná zástupný symbol pro ID `todo` položky. Když `GetById` je vyvolána, přiřadí hodnotu "{id} v adrese URL pro metodu `id` parametr.

`Name = "GetTodo"`Vytvoří pojmenovanou trasu a umožňuje propojit tato trasa v odpovědi HTTP. I objasníme ho s příklad později. V tématu [směrování do akce Kontroleru](xref:mvc/controllers/routing) podrobné informace.

### <a name="return-values"></a>Vrácené hodnoty

`GetAll` Metoda vrátí `IEnumerable`. MVC automaticky serializuje objekt, který má [JSON](http://www.json.org/) a zapisuje JSON do textu zprávy s odpovědí. Kód odpovědi pro tuto metodu je 200, za předpokladu, že nejsou žádné neošetřené výjimky. (Neošetřených výjimek jsou převedeny do 5xx chyby).

Naproti tomu `GetById` metoda vrátí další Obecné `IActionResult` typu, který představuje širokou škálu návratové typy. `GetById`má dva různé návratové typy:

* Pokud žádná položka odpovídá požadovanému ID, metoda vrátí chybu 404.  K tomu je potřeba vrácení `NotFound`.

* Metoda, jinak vrátí hodnotu 200 s těla odpovědi JSON. K tomu je potřeba návrat`ObjectResult`
