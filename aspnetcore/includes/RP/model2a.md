<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Přidat připojovací řetězec databáze

Přidat připojovací řetězec pro *appsettings.json* souboru.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Zaregistrujte kontext databáze

Zaregistrovat kontext databáze s [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru *Startup.cs* souboru.
