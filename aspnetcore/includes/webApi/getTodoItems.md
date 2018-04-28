::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Předchozí kód definuje třídu kontroleru rozhraní API bez metod. V následujících částech se přidají metody k implementaci rozhraní API.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Předchozí kód definuje třídu kontroleru rozhraní API bez metod. V následujících částech se přidají metody k implementaci rozhraní API. Třída je opatřen poznámkou `[ApiController]` atribut pro povolení některé pohodlné funkce. Informace o funkcích, které jsou povolené v atributu najdete v tématu [opatřit poznámkami se ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).
::: moniker-end

Konstruktor kontroleru používá [vkládání závislostí](xref:fundamentals/dependency-injection) vložení kontext databáze (`TodoContext`) do kontroleru. Kontext databáze se používá v každé z [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metody v kontroleru. Konstruktor přidá položku do databáze v paměti, pokud neexistuje.

## <a name="get-to-do-items"></a>Získat položkami seznamu úkolů

Chcete-li získat položkami seznamu úkolů, přidejte následující metody, které `TodoController` třídy:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

Tyto metody implementovat tyto dvě metody GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Zde je ukázka odpovědi HTTP pro `GetAll` metoda:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

Později v tomto kurzu I zobrazí, jak lze zobrazit odpověď HTTP s [Postman](https://www.getpostman.com/) nebo [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).

### <a name="routing-and-url-paths"></a>Směrování a adresy URL cesty

`[HttpGet]` Atribut označuje metodu, která reaguje na požadavek HTTP GET. Cesta URL pro každou metodu má následující formát:

* Trvat řetězec šablony v kontroleru `Route` atribut:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* Nahraďte `[controller]` název kontroleru, což je název třídy kontroleru minus příponou "Controller". Tato ukázka je název třídy kontroleru **Todo**řadiče a názvu kořenové je "úkolů". ASP.NET Core [směrování](xref:mvc/controllers/routing) malá a velká písmena.
* Pokud `[HttpGet]` atribut má šablonu trasy (například `[HttpGet("/products")]`, která připojení k cestě. Tato ukázka nepoužívá šablony. Další informace najdete v tématu [atribut směrování s atributy Http [akce]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

V následujícím `GetById` metody `"{id}"` je proměnná zástupný symbol pro jedinečný identifikátor položky seznamu úkolů. Když `GetById` je vyvolána, přiřadí hodnota `"{id}"` v adrese URL pro metodu `id` parametr.

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

`Name = "GetTodo"` Vytvoří pojmenovanou trasu. Pojmenované trasy:

* Povolte aplikaci vytvořit propojení HTTP pomocí názvu trasy.
* Jsou vysvětleny později v tomto kurzu.

### <a name="return-values"></a>Vrácené hodnoty

`GetAll` Metoda vrátí kolekci `TodoItem` objekty. MVC automaticky serializuje objekt, který má [JSON](https://www.json.org/) a zapisuje JSON do textu zprávy s odpovědí. Kód odpovědi pro tuto metodu je 200, za předpokladu, že nejsou žádné neošetřené výjimky. Nezpracované výjimky jsou převedeny do 5xx chyby.

::: moniker range="<= aspnetcore-2.0"
Naproti tomu `GetById` metoda vrátí další Obecné [IActionResult typu](xref:web-api/action-return-types#iactionresult-type), která představuje širokou škálu návratové typy. `GetById` má dva různé návratové typy:

* Pokud žádná položka odpovídá požadovanému ID, metoda vrátí chybu 404. Vrácení [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) vrátí odpověď HTTP 404.
* Metoda, jinak vrátí hodnotu 200 s těla odpovědi JSON. Vrácení [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) výsledkem odpověď HTTP 200.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Naproti tomu `GetById` metoda vrátí [ActionResult\<T > typ](xref:web-api/action-return-types#actionresultt-type), která představuje širokou škálu návratové typy. `GetById` má dva různé návratové typy:

* Pokud žádná položka odpovídá požadovanému ID, metoda vrátí chybu 404. Vrácení [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) vrátí odpověď HTTP 404.
* Metoda, jinak vrátí hodnotu 200 s těla odpovědi JSON. Vrácení `item` výsledkem odpověď HTTP 200.
::: moniker-end