## <a name="implement-the-other-crud-operations"></a>Provádění jiných operací CRUD

V následujících částech `Create`, `Update`, a `Delete` metody se přidají do kontroleru.

### <a name="create"></a>Create

Přidejte následující `Create` metody:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Předchozí kód je metoda HTTP POST, je určeno [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut. [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atribut oznamuje MVC k získání hodnoty položky seznamu úkolů z textu požadavku HTTP.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Předchozí kód je metoda HTTP POST, je určeno [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atribut. MVC získá hodnotu položky úkolů z textu požadavku HTTP.

::: moniker-end

`CreatedAtRoute` Metody:

* Vrátí odezvě 201. HTTP 201 je standardní odpověď pro metodu POST protokolu HTTP, která vytvoří nový prostředek na serveru.
* Přidá do odpovědi hlavičku umístění. Hlavička umístění Určuje identifikátor URI nově vytvořeného úkolu položky. Zobrazit [10.2.2 201 vytvořili](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* K vytvoření adresy URL používá "GetTodo" s názvem trasy. "GetTodo" s názvem trasy je definována v `GetById`:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Odeslat požadavek na vytvoření pomocí nástroje Postman

* Spusťte aplikaci.
* Otevřete nástroj Postman.

![Konzola nástroje postman](../../tutorials/first-web-api/_static/pmc.png)

* Aktualizujte číslo portu adresy URL místního hostitele.
* Nastavte jako metodu HTTP *příspěvek*.
* Klikněte na tlačítko **tělo** kartu.
* Vyberte **nezpracovaná** přepínač.
* Nastavte typ *JSON (application/json)*.
* Zadejte text požadavku s položku úkolu připomínající následující JSON:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Klikněte na tlačítko **odeslat** tlačítko.

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> Pokud žádná odpověď zobrazí po kliknutí na tlačítko **odeslat**, zakažte **ověření certifikace SSL** možnost. Se nachází v části **souboru** > **nastavení**. Klikněte na tlačítko **odeslat** tlačítko znovu po zakázání nastavení.

::: moniker-end

Klikněte na tlačítko **záhlaví** kartu **odpovědi** podokně a zkopírujte **umístění** hodnota hlavičky:

![Karta hlavičky z konzoly nástroje Postman](../../tutorials/first-web-api/_static/pmc2.png)

Hlavička umístění URI slouží pro přístup k nové položky.

### <a name="update"></a>Aktualizace

Přidejte následující `Update` metody:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

`Update` je podobný `Create`, s výjimkou používá HTTP PUT. Odpověď je [204 (žádný obsah)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Podle specifikace HTTP vyžaduje požadavek PUT klientovi umožní odeslat celý aktualizovaná entita, nikoli pouze rozdíly. Pro podporu částečné aktualizace, pomocí HTTP PATCH.

Pomocí nástroje Postman aktualizovat název položky seznamu úkolů "procesem cat":

![Postman konzola znázorňující 204 (žádný obsah) odpovědi](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Odstranit

Přidejte následující `Delete` metody:

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete` Odpověď je [204 (žádný obsah)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Pomocí nástroje Postman odstranění položky seznamu úkolů:

![Postman konzola znázorňující 204 (žádný obsah) odpovědi](../../tutorials/first-web-api/_static/pmd.png)
