---
title: Stránky Razor s EF jádra ASP.NET Core - Migrations - 4 8
author: rick-anderson
description: V tomto kurzu začnete používat funkci migrace EF jádra pro správu změn datových modelů v aplikaci ASP.NET MVC jádra.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: 15e3bc57e98b249cbefc394bbe1a136a709a03a7
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37089955"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Stránky Razor s EF jádra ASP.NET Core - Migrations - 4 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Podle [tní Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

V tomto kurzu se používá funkci EF základní migrace pro správu změn datových modelů.

Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

Když je vyvinut novou aplikaci, model dat často změny. Pokaždé, když změny modelu modelu získá synchronizována s databází. Tento kurz spuštění nakonfigurováním rozhraní Entity Framework pro vytvoření databáze, pokud neexistuje. Pokaždé, když datový model změny:

* Databáze je vyřazeno.
* EF vytvoří novou, který neodpovídá modelu.
* Aplikace doplňuje databáze s testovacích datech.

Tento přístup k udržování databáze synchronizace s datovým modelem funguje dobře, dokud můžete aplikaci nasadit do produkčního prostředí. Když aplikace běží v produkčním prostředí, je obvykle ukládá data, která je třeba zachovat. Aplikace nemůže začínat testu DB pokaždé, když dojde ke změně (jako je například přidávání nové sloupce). Tento problém řeší funkci migrace základní EF povolením EF základní aktualizovat schéma databáze místo vytvoření nové databáze.

Namísto vyřadit a znovu vytvořit databázi, když datový model změny, migrace aktualizace schématu a uchovává existující data.

## <a name="drop-the-database"></a>Odpojení databáze

Použití **Průzkumník objektů systému SQL Server** (SSOX) nebo `database drop` příkaz:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

V **Konzola správce balíčků** (pomocí PMC), spusťte následující příkaz:

```PMC
Drop-Database
```

Spustit `Get-Help about_EntityFrameworkCore` z pomocí PMC získat informace nápovědy.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Otevřete okno příkazového řádku a přejděte do složky projektu. Obsahuje složky projektu *Startup.cs* souboru.

Zadejte v příkazovém okně:

 ```console
 dotnet ef database drop
 ```

------

## <a name="create-an-initial-migration-and-update-the-db"></a>Vytváření počáteční migrace a aktualizaci databáze

Sestavte projekt a vytvořte první migrace.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

------

### <a name="examine-the-up-and-down-methods"></a>Zkontrolujte nahoru a dolů metody

Základní EF `migrations add` příkaz vygeneruje kód k vytvoření databáze. Tento kód migrace je v *migrace\<časové razítko > _InitialCreate.cs* souboru. `Up` Metodu `InitialCreate` třída vytvoří DB tabulky, které odpovídají sady dat modelu entity. `Down` Metoda odstraní, jak je znázorněno v následujícím příkladu:

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

Migrace volání `Up` metody k implementaci změny modelu dat pro migraci. Když zadáte příkaz k vrácení aktualizace, migrace volání `Down` metoda.

Předchozí kód je počáteční migrace. Tento kód byl vytvořen, když `migrations add InitialCreate` byl spuštěn příkaz. Parametr name migrace ("InitialCreate" v příkladu) se používá pro název souboru. Název migrace může být jakýkoli název platný soubor. Doporučujeme vybrat slovo nebo frázi, která shrnuje, co probíhá při migraci. Například migrace, který přidat tabulku oddělení může být například "AddDepartmentTable."

Pokud počáteční migrace je vytvořen a existuje databáze:

* Generování kódu vytvoření databáze.
* Kód pro vytvoření databáze není nutné spustit, protože databáze již odpovídá datový model. Pokud kód vytvoření DB běží, nemá proveďte požadované změny, protože databáze již odpovídá datový model.

Pokud chcete aplikaci nasadit do nového prostředí, pro vytvoření databáze musíte spustit kód pro vytvoření databáze.

Dříve databáze byla vyřazena a neexistuje, takže migrace vytvoří novou databázi.

### <a name="the-data-model-snapshot"></a>Snímek dat modelu

Vytvoření migrace *snímku* z aktuální schéma databáze v *Migrations/SchoolContextModelSnapshot.cs*. Když přidáte migrace, EF Určuje, co se změnilo tak, že porovnáte datový model, který soubor snímku.

K odstranění migrace, použijte následující příkaz:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Odebrat migrace

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

Další informace najdete v tématu [odebrat dotnet ef migrace](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).

------

Příkaz remove migrace odstraní migrace a zajišťuje, že je správně obnovení snímku.

### <a name="remove-ensurecreated-and-test-the-app"></a>Odeberte EnsureCreated a testování aplikací

Pro včasné vývoj `EnsureCreated` byl použit. V tomto kurzu se používají migrace. `EnsureCreated` má následující omezení:

* Obchází migrace a vytvoří databáze a schéma.
* Nelze vytvořit tabulku migrace.
* Můžete *není* použít s migrací.
* Je určený pro testování nebo rychlé vytváření prototypů kde je databáze vyřadit a znovu vytvořit často.

Odebrat následující řádek z `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

Spusťte aplikaci a ověřte, že databáze je nasadí.

### <a name="inspect-the-database"></a>Zkontrolujte databáze

Použití **Průzkumník objektů systému SQL Server** Kontrola databáze. Všimněte si, přidání `__EFMigrationsHistory` tabulky. `__EFMigrationsHistory` Tabulky uchovává informace o migrace, které byly použity k databázi. Zobrazení dat v `__EFMigrationsHistory` tabulky, zobrazuje jeden řádek na první migraci. V poslední protokolu v předchozím příkladu výstupu rozhraní příkazového řádku se zobrazuje příkaz INSERT, která vytváří tento řádek.

Spusťte aplikaci a ověřte, že všechno funguje.

## <a name="applying-migrations-in-production"></a>Použití migrace v produkčním prostředí

Doporučujeme, abyste měli produkční aplikace **není** volání [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) při spuštění aplikace. `Migrate` nelze volat z aplikace v serverové farmě. Například pokud aplikace bylo cloudové nasazení se Škálováním na více systémů (více instancí aplikace běží).

V rámci nasazení a řízené způsobem se má provést migrace databáze. Provozní databáze migrace přístupy patří:

* Pomocí migrace vytvořit skripty SQL a pomocí skriptů SQL v nasazení.
* Spuštění `dotnet ef database update` z řízené prostředí.

Základní EF používá `__MigrationsHistory` tabulce najdete, pokud žádné migrace muset spustit. Pokud je aktuální databáze, je spustit žádné migrace.

## <a name="troubleshooting"></a>Poradce při potížích

Stažení [dokončené aplikace](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Aplikace generuje následující výjimky:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Řešení: spuštění `dotnet ef database update`

### <a name="additional-resources"></a>Další zdroje

* [.NET core rozhraní příkazového řádku](/ef/core/miscellaneous/cli/dotnet).
* [Konzola Správce balíčků (Visual Studio)](/ef/core/miscellaneous/cli/powershell)

::: moniker-end

> [!div class="step-by-step"]
> [Předchozí](xref:data/ef-rp/sort-filter-page)
> [další](xref:data/ef-rp/complex-data-model)