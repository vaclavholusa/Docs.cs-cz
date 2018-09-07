<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Přidat vygenerované uživatelské rozhraní nástroje a provádět počáteční migraci

Přidejte následující řádky do *RazorPagesMovie.csproj* souboru, těsně před uzavírací `</Project>` značky:

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.1.0-preview1-final"/>
</ItemGroup>
```  
Z příkazového řádku spusťte následující příkazy rozhraní příkazového řádku .NET Core:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`DotNetCliToolReference` Elementu a `add package` příkaz nainstalovat nástroje, které jsou potřeba ke spouštění modulu generování uživatelského rozhraní.

`ef migrations add InitialCreate` Příkaz vygeneruje kód pro vytvoření schématu počáteční databáze. Schéma je založen na zadaném v modelu `DbContext` (v *Models/MovieContext.cs* souboru). `InitialCreate` Argument se používá k pojmenování migrace. Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci. Zobrazit [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.

`ef database update` Příkaz spustí `Up` metodu *migrace /\<časové razítko > _InitialCreate.cs* soubor, který vytvoří databázi.
