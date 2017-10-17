<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="c387e-101">Vygenerované uživatelské rozhraní film modelu</span><span class="sxs-lookup"><span data-stu-id="c387e-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="c387e-102">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="c387e-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="c387e-103">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c387e-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="c387e-104">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="c387e-104">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="c387e-105">Visual Studio ukončete a spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="c387e-105">Exit Visual Studio and run the command again.</span></span>