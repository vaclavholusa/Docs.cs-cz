## <a name="add-initial-migration-and-update-the-database"></a>Přidejte počáteční migrace a aktualizaci databáze

* Otevřete příkazový řádek a přejděte do adresáře projektu. (Obsahující adresář *Startup.cs* souboru).

* V příkazovém řádku spusťte následující příkazy:

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET core](https://docs.microsoft.com/dotnet/core/tools/index) je implementace a platformy .NET. Tady je co dělat, tyto příkazy:

  * [obnovení DotNet](/dotnet/core/tools/dotnet-restore): stáhne balíčky NuGet zadaný v *.csproj* souboru.
  * `dotnet ef migrations add Initial` Spustí příkaz migrace Entity Framework .NET Core rozhraní příkazového řádku a vytvoří počáteční migrace. Parametr po "Přidat" je název, který přiřadíte k migraci. Zde můžete jste pojmenování migrace "Počáteční" vzhledem k tomu, že je migrace počáteční databáze. Tato operace vytvoří *dat nebo migrace nebo\<datum a čas > _Initial.cs* souboru, který obsahuje příkazy migrace pro přidání *film* tabulky do databáze.
  * `dotnet ef database update`  Aktualizuje databázi pomocí migrace, který jsme právě vytvořili.

V dalším kurzu získáte informace o databázi a připojovací řetězec. Získáte informace o změn datových modelů v [přidat pole](xref:tutorials/first-mvc-app/new-field) kurzu.
