---
title: Stránky Razor s EF jádra ASP.NET Core - Migrations - 4 8
author: rick-anderson
description: V tomto kurzu začnete používat funkci migrace EF jádra pro správu změn datových modelů v aplikaci ASP.NET MVC jádra.
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: 690beaabeab098cf9b764730b1bf1bd04bf6b003
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Stránky Razor s EF jádra ASP.NET Core - Migrations - 4 8

Podle [tní Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

V tomto kurzu se používá funkci EF základní migrace pro správu změn datových modelů.

Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Když je vyvinut novou aplikaci, model dat často změny. Pokaždé, když změny modelu modelu získá synchronizována s databází. Tento kurz spuštění nakonfigurováním rozhraní Entity Framework pro vytvoření databáze, pokud neexistuje. Pokaždé, když datový model změny:

* Databáze je vyřazeno.
* EF vytvoří novou, který neodpovídá modelu.
* Aplikace doplňuje databáze s testovacích datech.

Tento přístup k udržování databáze synchronizace s datovým modelem funguje dobře, dokud můžete aplikaci nasadit do produkčního prostředí. Když aplikace běží v produkčním prostředí, je obvykle ukládá data, která je třeba zachovat. Aplikace nemůže začínat testu DB pokaždé, když dojde ke změně (jako je například přidávání nové sloupce). Tento problém řeší funkci migrace základní EF povolením EF základní aktualizovat schéma databáze místo vytvoření nové databáze.

Namísto vyřadit a znovu vytvořit databázi, když datový model změny, migrace aktualizace schématu a uchovává existující data.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Entity Framework Core NuGet balíčky pro migrace

Chcete-li pracovat s migrací, použijte **Konzola správce balíčků** (pomocí PMC) nebo rozhraní příkazového řádku (CLI). Tyto kurzy ukazují, jak používat rozhraní příkazového řádku. Informace o pomocí PMC je na [konci tohoto kurzu](#pmc).

Nástroje EF jádra pro rozhraní příkazového řádku (CLI) jsou uvedeny v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). K instalaci tohoto balíčku, přidejte ho do `DotNetCliToolReference` kolekce v *.csproj* souboru, jak je vidět. **Poznámka:** tento balíček musí být nainstalována úpravou *.csproj* souboru. `install-package` Příkaz nebo Správce balíčků grafické uživatelské rozhraní nelze použít k instalaci tohoto balíčku. Upravit *.csproj* kliknutím pravým tlačítkem myši na název projektu v souboru **Průzkumníku řešení** a výběrem **upravit ContosoUniversity.csproj**.

Následující kód ukazuje aktualizovaný *.csproj* soubor s EF základní rozhraní příkazového řádku nástroje zvýrazněná:

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
Čísla verzí v předchozím příkladu byly aktuální v době kurzu byla zapsána. Použijte stejnou verzi pro EF základní rozhraní příkazového řádku nástroje v dalších balíčků.

## <a name="change-the-connection-string"></a>Změnit připojovací řetězec

V *appSettings.JSON určený* souboru, změňte název databáze v připojovacím řetězci ContosoUniversity2.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Změna názvu DB v připojovacím řetězci způsobí, že první migrace k vytvoření nové databáze. Nové databáze je vytvořit, protože s tímto názvem neexistuje. Změna připojovací řetězec není nutné u Začínáme s migrací.

Alternativu ke změně názvu databáze je odstranění databáze. Použití **Průzkumník objektů systému SQL Server** (SSOX) nebo `database drop` rozhraní příkazového řádku příkaz:

 ```console
 dotnet ef database drop
 ```

V následující části vysvětluje, jak spouštět příkazy příkazového řádku.

## <a name="create-an-initial-migration"></a>Vytvoření počáteční migrace

Sestavte projekt.

Otevřete okno příkazového řádku a přejděte do složky projektu. Obsahuje složky projektu *Startup.cs* souboru.

Zadejte v příkazovém okně:

```console
dotnet ef migrations add InitialCreate
```

Příkazové okno se zobrazí podobná následující informace:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Pokud migrace selže se zprávou "*nemůže přistupovat k souboru... ContosoUniversity.dll vzhledem k tomu, že je stále používán jiným procesem.* " Zobrazí se:

* Zastavte službu IIS Express.

   * Ukončete a restartujte Visual Studio, nebo
   * Najít ikonu IIS Express na hlavním panelu systému Windows.
   * Klikněte pravým tlačítkem na ikonu služby IIS Express a pak klikněte na tlačítko **ContosoUniversity > Zastavit lokality**.

Pokud chybová zpráva "sestavení se nezdařilo." Zobrazí se, spusťte příkaz znovu. Pokud se tato chyba, nechte poznámku v dolní části tohoto kurzu.

### <a name="examine-the-up-and-down-methods"></a>Zkontrolujte nahoru a dolů metody

Příkaz EF základní `migrations add` generovaného kódu k vytvoření databáze z. Tento kód migrace je v *migrace\<časové razítko > _InitialCreate.cs* souboru. `Up` Metodu `InitialCreate` třída vytvoří DB tabulky, které odpovídají sady dat modelu entity. `Down` Metoda odstraní, jak je znázorněno v následujícím příkladu:

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Migrace volání `Up` metody k implementaci změny modelu dat pro migraci. Když zadáte příkaz k vrácení aktualizace, migrace volání `Down` metoda.

Předchozí kód je počáteční migrace. Tento kód byl vytvořen, když `migrations add InitialCreate` byl spuštěn příkaz. Parametr name migrace ("InitialCreate" v příkladu) se používá pro název souboru. Název migrace může být jakýkoli název platný soubor. Doporučujeme vybrat slovo nebo frázi, která shrnuje, co probíhá při migraci. Například migrace, který přidat tabulku oddělení může být například "AddDepartmentTable."

Pokud počáteční migrace je vytvořen a existuje databáze:

* Generování kódu vytvoření databáze.
* Kód pro vytvoření databáze není nutné spustit, protože databáze již odpovídá datový model. Pokud kód vytvoření DB běží, nemá proveďte požadované změny, protože databáze již odpovídá datový model.

Pokud chcete aplikaci nasadit do nového prostředí, pro vytvoření databáze musíte spustit kód pro vytvoření databáze.

Připojovací řetězec dříve bylo změněno používat nový název databáze. Zadaná databáze neexistuje, vytvoří migrace databáze.

### <a name="the-data-model-snapshot"></a>Snímek dat modelu

Vytvoří migrace *snímku* z aktuální schéma databáze v *Migrations/SchoolContextModelSnapshot.cs*. Když přidáte migrace, EF Určuje, co se změnilo tak, že porovnáte datový model, který soubor snímku.

Při odstraňování migrace, použijte [odebrat dotnet ef migrace](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) příkaz. `dotnet ef migrations remove` Odstraní migrace a zajišťuje, že je správně obnovení snímku.

V tématu [EF základní migrace v prostředích Team](/ef/core/managing-schemas/migrations/teams) Další informace o tom, jak se používá soubor snímku.

## <a name="remove-ensurecreated"></a>Odebrat EnsureCreated

Pro včasné vývoj `EnsureCreated` příkaz nebyl použit. V tomto kurzu se používá migrace. `EnsureCreated` má následující omezení:

* Obchází migrace a vytvoří databáze a schéma.
* Nelze vytvořit tabulku migrace.
* Můžete *není* použít s migrací.
* Je určený pro testování nebo rychlé vytváření prototypů kde je databáze vyřadit a znovu vytvořit často.

Odebrat následující řádek z `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Použití migrace k databázi v vývoj

V okně příkazového řádku zadejte následující příkaz pro vytvoření databáze a tabulky.

```console
dotnet ef database update
```

Poznámka: Pokud `update` příkaz vrátí chybu "Sestavení se nezdařilo.":

* Příkaz spusťte znovu.
* Pokud se znovu nezdaří, Visual Studio ukončete a spusťte `update` příkaz.
* Ponechte zprávu v dolní části stránky.

Výstup z tohoto příkazu je podobná `migrations add` příkaz výstupu. V předchozím příkazu se zobrazí protokoly pro příkazy SQL, které nastavení databáze. Většina protokolů se tento parametr vynechán následující ukázkový výstup:

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

Chcete-li snížit úroveň podrobností ve zprávách protokolu, změňte úrovní záznamu do protokolu v *appsettings. Development.JSON* souboru. Další informace najdete v tématu [Úvod k protokolování](xref:fundamentals/logging/index).

Použití **Průzkumník objektů systému SQL Server** Kontrola databáze. Všimněte si, přidání `__EFMigrationsHistory` tabulky. `__EFMigrationsHistory` Tabulky uchovává informace o migrace, které byly použity k databázi. Zobrazení dat v `__EFMigrationsHistory` tabulky, zobrazuje jeden řádek na první migraci. V poslední protokolu v předchozím příkladu výstupu rozhraní příkazového řádku se zobrazuje příkaz INSERT, která vytváří tento řádek.

Spusťte aplikaci a ověřte, že všechno funguje.

## <a name="applying-migrations-in-production"></a>Použití migrace v produkčním prostředí

Doporučujeme, abyste měli produkční aplikace **není** volání [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) při spuštění aplikace. `Migrate` nelze volat z aplikace v serverové farmě. Například pokud aplikace bylo cloudové nasazení se Škálováním na více systémů (více instancí aplikace běží).

V rámci nasazení a řízené způsobem se má provést migrace databáze. Provozní databáze migrace přístupy patří:

* Pomocí migrace vytvořit skripty SQL a pomocí skriptů SQL v nasazení.
* Spuštění `dotnet ef database update` z řízené prostředí.

Základní EF používá `__MigrationsHistory` tabulce najdete, pokud žádné migrace muset spustit. Pokud je aktuální databáze, je spustit žádné migrace.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Rozhraní příkazového řádku (CLI) vs. Konzola správce balíčků (pomocí PMC)

Základní EF nástrojů pro správu migrace najdete na webu:

* .NET core rozhraní příkazového řádku.
* Rutiny prostředí PowerShell v sadě Visual Studio **Konzola správce balíčků** okno (pomocí PMC).

Tento kurz ukazuje, jak používat rozhraní příkazového řádku, někteří vývojáři přednost používání pomocí PMC.

Základní EF příkazů pro pomocí PMC jsou v [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) balíčku. Tento balíček je součástí [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, takže není nutné ji nainstalovat.

**Důležité:** tento není stejného balíčku jako instalace pro rozhraní příkazového řádku úpravou *.csproj* souboru. Název touto končí v `Tools`, na rozdíl od název balíčku rozhraní příkazového řádku, které končí na `Tools.DotNet`.

Další informace o rozhraní příkazového řádku najdete v tématu [.NET Core rozhraní příkazového řádku](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Další informace o příkazech pomocí PMC najdete v tématu [Konzola správce balíčků (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Poradce při potížích

Stažení [dokončené aplikace pro tuto fázi](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Aplikace generuje následující výjimky:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Řešení: spuštění `dotnet ef database update`

Pokud `update` příkaz vrátí chybu "Sestavení se nezdařilo.":

* Příkaz spusťte znovu.
* Ponechte zprávu v dolní části stránky.

> [!div class="step-by-step"]
> [Předchozí](xref:data/ef-rp/sort-filter-page)
> [další](xref:data/ef-rp/complex-data-model)
