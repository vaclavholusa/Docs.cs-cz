## <a name="implement-the-other-crud-operations"></a>Implementace dalších operací CRUD

Přidáme `Create`, `Update`, a `Delete` metody pro kontroler. Tyto jsou varianty motiv, tak I budete jenom zobrazit kód a zvýrazněte hlavní rozdíly. Sestavení projektu po přidání nebo změna kódu.

### <a name="create"></a>Create

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Toto je metoda HTTP POST, uvedené [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atribut. [ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atribut informuje MVC k získání hodnoty položky úkolů z textu požadavku HTTP.

`CreatedAtRoute` Metoda vrátí odpověď 201, což je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru. `CreatedAtRoute`také přidá do odpovědi hlavičku umístění. Hlavička umístění Určuje identifikátor URI položky nově vytvořený úkolů. V tématu [10.2.2 201 vytvořit](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Použití Postman k odeslání požadavku na vytvořit

![Konzole postman](../../tutorials/first-web-api/_static/pmc.png)

* Nastavte jako metodu HTTP`POST`
* Vyberte **textu** přepínač
* Vyberte **nezpracovaná** přepínač
* Nastavte typ do formátu JSON
* V editoru klíč hodnota zadejte položky Todo 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Vyberte **odeslat**

* Vyberte kartu hlavičky v dolním podokně a zkopírujte **umístění** hlavičky:

![Karta hlavičky konzoly Postman](../../tutorials/first-web-api/_static/pmget.png)

Hlavička umístění URI můžete použít pro přístup k prostředku, který jste právě vytvořili. Odvolat `GetById` vytvořili `"GetTodo"` s názvem trasy:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a>Aktualizace

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update`je podobná `Create`, ale používá HTTP PUT. Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Podle specifikace protokolu HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovanou entitu, nikoli pouze rozdíly. Chcete-li podporovat částečné aktualizace, použijte HTTP PATCH.

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Odstranit

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Odpověď [204 (ne obsahu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](../../tutorials/first-web-api/_static/pmd.png)
