<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Přidat vygenerované uživatelské rozhraní nástroje a provedení počáteční migrace

Z příkazového řádku spusťte následující příkazy .NET Core rozhraní příkazového řádku:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`add package` Příkaz nainstaluje nástroje potřebné ke spuštění modulu generování uživatelského rozhraní.

`ef migrations add InitialCreate` Příkaz generuje kód pro vytvoření schématu počáteční databáze. Schéma je založena na zadaný ve model `DbContext` (v *Models/MovieContext.cs* souboru). `Initial` Argument se používá k pojmenování byla migrace. Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci. V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.

`ef database update` Příkaz spustí `Up` metoda v *migrace nebo\<časové razítko > _InitialCreate.cs* souboru, který vytvoří databázi.