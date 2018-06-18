<span data-ttu-id="f0557-101">Generovaný kód Identity databáze vyžaduje [Entity Framework Core migrace](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="f0557-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="f0557-102">Vytvořte migrace a aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="f0557-102">Create a migration and update the database.</span></span> <span data-ttu-id="f0557-103">Například spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f0557-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0557-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0557-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f0557-105">V sadě Visual Studio **Konzola správce balíčků**:</span><span class="sxs-lookup"><span data-stu-id="f0557-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f0557-106">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="f0557-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

<span data-ttu-id="f0557-107">Parametr name "CreateIdentitySchema" pro `Add-Migration` je libovolný příkaz.</span><span class="sxs-lookup"><span data-stu-id="f0557-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="f0557-108">`"CreateIdentitySchema"` Popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="f0557-108">`"CreateIdentitySchema"` describes the migration.</span></span>