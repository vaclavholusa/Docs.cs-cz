## <a name="implement-the-other-crud-operations"></a>Implementace dalších operací CRUD

V následujících částech `Create`, `Update`, a `Delete` metody jsou přidány do kontroleru.

### <a name="create"></a>Create

Přidejte následující `Create` metoda:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Předchozí kód je metoda HTTP POST, jak [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut. [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atribut informuje MVC k získání hodnoty položky úkolů z textu požadavku HTTP.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Předchozí kód je metoda HTTP POST, jak [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut. MVC získá hodnotu položky úkolů z textu požadavku HTTP.
::: moniker-end

`CreatedAtRoute` Metoda:

* Vrátí 201 odpověď. HTTP 201 je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru.
* Přidá do odpovědi hlavičku umístění. Hlavička umístění Určuje identifikátor URI položky nově vytvořený úkolů. V tématu [10.2.2 201 vytvořit](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Vytvoří adresu URL pomocí "GetTodo" s názvem trasy. "GetTodo" s názvem trasy je definována v `GetById`:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Použití Postman k odeslání požadavku na vytvořit

* Spusťte aplikaci.
* Otevřete Postman.

![Konzole postman](../../tutorials/first-web-api/_static/pmc.png)

* Aktualizujte číslo portu v adrese URL místního hostitele.
* Nastavte jako metodu HTTP *POST*.
* Klikněte **textu** kartě.
* Vyberte **nezpracovaná** přepínač.
* Nastavte typ na *JSON (application/json)*.
* Zadejte obsah žádosti s úkol podobné následujícím kódu JSON:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Klikněte **odeslat** tlačítko.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Pokud žádná odpověď zobrazí po kliknutí na **odeslat**, zakažte **SSL certifikační ověření** možnost. To je v části **soubor** > **nastavení**. Klikněte **odeslat** tlačítko znovu po zakázání nastavení.
::: moniker-end

Klikněte na tlačítko **hlavičky** ve **odpovědi** podokně a zkopírujte **umístění** hodnota hlavičky:

![Karta hlavičky konzoly Postman](../../tutorials/first-web-api/_static/pmc2.png)

Hlavička umístění URI slouží pro přístup k novou položku.

### <a name="update"></a>Aktualizace

Přidejte následující `Update` metoda:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` je podobná `Create`, s výjimkou používá HTTP PUT. Odpověď [204 (ne obsahu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Podle specifikace protokolu HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovanou entitu, nikoli pouze rozdíly. Chcete-li podporovat částečné aktualizace, použijte HTTP PATCH.

Použijte Postman k aktualizaci název položky úkolů vás "cat":

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Odstranit

Přidejte následující `Delete` metoda:

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete` Odpověď je [204 (ne obsahu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Pomocí Postman odstranit položku seznamu úkolů:

![Postman konzola znázorňující 204 odpovědi (ne obsahu)](../../tutorials/first-web-api/_static/pmd.png)
