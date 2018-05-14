<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Vygenerované uživatelské rozhraní film modelu

* Spusťte následující z příkazového řádku (v adresáři projektu, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory):

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Pokud dojde k chybě:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Otevřete příkazové prostředí do adresáře projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).
