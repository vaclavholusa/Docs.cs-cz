<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="324f7-101">Vygenerované uživatelské rozhraní film modelu</span><span class="sxs-lookup"><span data-stu-id="324f7-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="324f7-102">Spusťte následující z příkazového řádku (v adresáři projektu, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory):</span><span class="sxs-lookup"><span data-stu-id="324f7-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="324f7-103">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="324f7-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="324f7-104">Otevřete příkazové prostředí do adresáře projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="324f7-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

<span data-ttu-id="324f7-105">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="324f7-105">If you get the error:</span></span>
  ```
  The process cannot access the file
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll'
  because it is being used by another process.
  ```

<span data-ttu-id="324f7-106">Visual Studio ukončete a spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="324f7-106">Exit Visual Studio and run the command again.</span></span>
