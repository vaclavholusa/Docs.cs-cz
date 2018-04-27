## <a name="call-the-web-api-with-jquery"></a>Volání webového rozhraní API s jQuery

V této části se přidá stránku HTML, která používá jQuery k volání webového rozhraní API. jQuery zahájí požadavek a aktualizuje stránce s podrobnostmi z odpovědi rozhraní API.

Konfigurace projektu chcete poskytovat statické soubory a povolit výchozí mapování souboru. To se provádí volání [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) rozšiřující metody v *Startup.Configure*. Další informace najdete v tématu [pracovat s statické soubory v ASP.NET Core](xref:fundamentals/static-files).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseDefaultFiles();
    app.UseStaticFiles();

    app.UseMvc();
}
```

Do projektu přidejte stránku HTML pomocí následujících kroků:

* Klikněte pravým tlačítkem myši *wwwroot* adresář a zaškrtněte možnost **přidat** > **nová položka** > **stránku HTML**.
* Název souboru *index.html*a zahrnout následující kód do souboru:

[!code-html[Main](samples/sample3.html)]

Ke změně nastavení spuštění projektu ASP.NET Core může být nutné testovací stránce HTML místně. Otevřete *launchSettings.json* v *vlastnosti* adresáři projektu. Odeberte `launchUrl` vlastnosti tak, aby se aplikace otevřít *index.html*&mdash;výchozí soubor projektu.

Chcete-li získat jQuery několika způsoby. V předchozím fragmentu kódu je knihovny načten z název CDN. Tato ukázka je kompletní příklad CRUD volání rozhraní API s jQuery. V této ukázce aby bohatší možnosti existují další funkce. Níže jsou vysvětlení kolem volání do rozhraní API.

### <a name="get-a-list-of-to-do-items"></a>Získat seznam položek úkolů

Pokud chcete získat seznam položek úkolů, odeslání požadavku HTTP GET na */api/todo*.

JQuery [ajax](https://api.jquery.com/jquery.ajax/) funkce odešle požadavek AJAX na rozhraní API, která vrátí hodnotu reprezentující na objekt nebo pole JSON. Tato funkce může zpracovat všechny formy interakce HTTP, odeslání požadavku HTTP do zadané `url`. `GET` slouží jako `type`. `success` Zpětného volání funkce je volána, pokud neproběhne. V zpětné volání modelu DOM je doplněné o informace úkolů.

[!code-javascript[Main](samples/sample4.html)]

### <a name="add-a-to-do-item"></a>Přidat položku seznamu úkolů

Pokud chcete přidat položku seznamu úkolů, odeslání požadavku HTTP POST na */api/todo/*. Text žádosti by měl obsahovat objekt úkolů. [Ajax](https://api.jquery.com/jquery.ajax/) používá funkce `POST` pro volání rozhraní API. Pro `POST` a `PUT` požádá, těle žádosti představuje data odeslaná do rozhraní API. Rozhraní API očekává text požadavku JSON. `accepts` a `contentType` možnosti jsou nastaveny na `application/json` klasifikovat typ média se přijatých a odeslaných, v uvedeném pořadí. Data jsou převedeny na objekt JSON pomocí [ `JSON.stringify` ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Když se rozhraní API vrátí úspěšné stavový kód, `getData` funkce je volána k aktualizaci tabulky HTML.

[!code-javascript[Main](samples/sample5.js)]

### <a name="update-a-to-do-item"></a>Aktualizujte položku seznamu úkolů

Aktualizace položku úkolu je velmi podobný jako při přidávání jednoho, protože oba tyto nástroje spoléhají na obsah žádosti. Pouze skutečné rozdílem mezi těmito dvěma v tomto případě je, že `url` změny přidat jedinečný identifikátor položky a `type` je `PUT`.

[!code-javascript[Main](samples/sample6.js)]

### <a name="delete-a-to-do-item"></a>Odstranit položku seznamu úkolů

Odstranění položky seznamu úkolů se provádí nastavením `type` při volání ajax `DELETE` a zadali položka je jedinečný identifikátor v adrese url.

[!code-javascript[Main](samples/sample6.js)]
