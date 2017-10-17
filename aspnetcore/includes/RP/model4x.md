<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="00f21-101">Vygenerované uživatelské rozhraní film modelu</span><span class="sxs-lookup"><span data-stu-id="00f21-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="00f21-102">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="00f21-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="00f21-103">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="00f21-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```