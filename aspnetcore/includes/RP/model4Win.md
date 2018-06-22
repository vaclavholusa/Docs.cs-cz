<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="bb6a6-101">Vygenerované uživatelské rozhraní film modelu</span><span class="sxs-lookup"><span data-stu-id="bb6a6-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="bb6a6-102">Spusťte následující z příkazového řádku (v adresáři projektu, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory):</span><span class="sxs-lookup"><span data-stu-id="bb6a6-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="bb6a6-103">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="bb6a6-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="bb6a6-104">Chyba který předchází se stane, když se do nesprávného adresáře.</span><span class="sxs-lookup"><span data-stu-id="bb6a6-104">The preceeding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="bb6a6-105">Otevřete příkazové prostředí do adresáře projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory), a poté spusťte příkaz který předchází.</span><span class="sxs-lookup"><span data-stu-id="bb6a6-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceeding command.</span></span>

<span data-ttu-id="bb6a6-106">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="bb6a6-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="bb6a6-107">Visual Studio ukončete a spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="bb6a6-107">Exit Visual Studio and run the command again.</span></span>
