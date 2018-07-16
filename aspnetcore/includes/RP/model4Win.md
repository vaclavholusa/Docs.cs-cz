<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="80f3c-101">Vygenerované uživatelské rozhraní Video modelu</span><span class="sxs-lookup"><span data-stu-id="80f3c-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="80f3c-102">Spusťte následující příkaz z příkazového řádku (v adresáři projektu, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory):</span><span class="sxs-lookup"><span data-stu-id="80f3c-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="80f3c-103">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="80f3c-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="80f3c-104">Předchozí chybě dojde, když jsou do nesprávného adresáře.</span><span class="sxs-lookup"><span data-stu-id="80f3c-104">The preceeding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="80f3c-105">Otevřete příkazové okno k adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory), a poté spusťte příkaz when.</span><span class="sxs-lookup"><span data-stu-id="80f3c-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceeding command.</span></span>

<span data-ttu-id="80f3c-106">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="80f3c-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="80f3c-107">Ukončení sady Visual Studio a spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="80f3c-107">Exit Visual Studio and run the command again.</span></span>
