## <a name="call-the-web-api-with-jquery"></a>Volání webového rozhraní API s jQuery

V této části se přidá stránku HTML, která používá jQuery k volání webového rozhraní API. jQuery zahájí požadavek a aktualizuje stránce s podrobnostmi z odpovědi rozhraní API.

Konfigurace projektu chcete poskytovat statické soubory a povolit výchozí mapování souboru. To se provádí volání [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) rozšiřující metody v *Startup.Configure*. Další informace najdete v tématu [statické soubory](xref:fundamentals/static-files).

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup2.cs?name=snippet_Configure&highlight=3-4)]

Přidání souboru HTML, s názvem *index.html*, do projektu *wwwroot* adresáře. Jeho obsah nahraďte následující kód:

[!code-html[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/index.html)]

Přidejte soubor JavaScript s názvem *site.js*, do projektu *wwwroot* adresáře. Nahraďte jeho obsah následujícím kódem:

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Ke změně nastavení spuštění projektu ASP.NET Core může být nutné testovací stránce HTML místně. Otevřete *launchSettings.json* v *vlastnosti* adresáři projektu. Odeberte `launchUrl` vlastnosti tak, aby se aplikace otevřít *index.html*&mdash;výchozí soubor projektu.

Chcete-li získat jQuery několika způsoby. V předchozím fragmentu kódu je knihovny načten z název CDN. Tato ukázka je kompletní příklad CRUD volání rozhraní API s jQuery. V této ukázce aby bohatší možnosti existují další funkce. Níže jsou vysvětlení kolem volání do rozhraní API.

### <a name="get-a-list-of-to-do-items"></a>Získat seznam položek úkolů

Pokud chcete získat seznam položek úkolů, odeslání požadavku HTTP GET na */api/todo*.

JQuery [ajax](https://api.jquery.com/jquery.ajax/) funkce odešle požadavek AJAX na rozhraní API, která vrátí hodnotu reprezentující na objekt nebo pole JSON. Tato funkce může zpracovat všechny formy interakce HTTP, odeslání požadavku HTTP do zadané `url`. `GET` slouží jako `type`. `success` Zpětného volání funkce je volána, pokud neproběhne. V zpětné volání modelu DOM je doplněné o informace úkolů.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Přidat položku seznamu úkolů

Pokud chcete přidat položku seznamu úkolů, odeslání požadavku HTTP POST na */api/todo/*. Text žádosti by měl obsahovat objekt úkolů. [Ajax](https://api.jquery.com/jquery.ajax/) používá funkce `POST` pro volání rozhraní API. Pro `POST` a `PUT` požádá, těle žádosti představuje data odeslaná do rozhraní API. Rozhraní API očekává text požadavku JSON. `accepts` a `contentType` možnosti jsou nastaveny na `application/json` klasifikovat typ média se přijatých a odeslaných, v uvedeném pořadí. Data jsou převedeny na objekt JSON pomocí [ `JSON.stringify` ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Když se rozhraní API vrátí úspěšné stavový kód, `getData` funkce je volána k aktualizaci tabulky HTML.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Aktualizujte položku seznamu úkolů

Aktualizace položku úkolu je velmi podobný jako při přidávání jednoho, protože oba tyto nástroje spoléhají na obsah žádosti. Pouze skutečné rozdílem mezi těmito dvěma v tomto případě je, že `url` změny přidat jedinečný identifikátor položky a `type` je `PUT`.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Odstranit položku seznamu úkolů

Odstranění položky seznamu úkolů se provádí nastavením `type` při volání AJAX `DELETE` a zadali položka je jedinečný identifikátor v adrese URL.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]
