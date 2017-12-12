<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="33ed3-101">Vygenerované uživatelské rozhraní film modelu</span><span class="sxs-lookup"><span data-stu-id="33ed3-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="33ed3-102">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="33ed3-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="33ed3-103">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="33ed3-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```
  
<span data-ttu-id="33ed3-104">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="33ed3-104">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="33ed3-105">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="33ed3-105">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>