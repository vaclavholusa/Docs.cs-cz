Generovaný kód Identity databáze vyžaduje [Entity Framework Core migrace](/ef/core/managing-schemas/migrations/). Vytvořte migrace a aktualizaci databáze. Například spusťte následující příkazy:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

V sadě Visual Studio **Konzola správce balíčků**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

Parametr name "CreateIdentitySchema" pro `Add-Migration` je libovolný příkaz. `"CreateIdentitySchema"` Popisuje migraci.