---
title: "Jádro ASP.NET MVC s EF Core - Migrations - 4 10"
author: tdykstra
description: "V tomto kurzu začnete používat funkci migrace EF jádra pro správu změn datových modelů v aplikaci MVC rozhraní ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: 0959ebc0f566540ea8a43d4889bb0e4fa041bfd6
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>Migrace – základní EF s kurz k ASP.NET MVC jádra (4 10)

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).

V tomto kurzu začnete používat funkci migrace EF jádra pro správu změn datových modelů. V dalších kurzech přidáte více migrací mění datový model.

## <a name="introduction-to-migrations"></a>Úvod do migrace

Při vývoji nových aplikací datový model změní často a pokaždé, když změn modelu, získá synchronizována s databází. Tyto kurzy jste spustili nakonfigurováním rozhraní Entity Framework pro vytvoření databáze, pokud neexistuje. Pokaždé, když změníte datový model – přidat, odebrat, nebo změňte tříd entit nebo změnit vaší třídy DbContext – pak můžete odstranit databázi a EF vytvoří novou, který odpovídá modelu a doplňuje s testovacích datech.

Udržování databáze synchronizace s datovým modelem, tato metoda funguje dobře, dokud nasazení aplikace do produkčního prostředí. Když aplikace běží v produkčním prostředí se obvykle ukládá data, která chcete zachovat a nechcete ztratit všechno pokaždé, když provedete změny, jako je například přidávání nové sloupce. Funkce migrace základní EF tento problém řeší tím, že umožňuje EF aktualizace schématu databáze, místo vytvoření nové databáze.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Entity Framework Core NuGet balíčky pro migrace

Chcete-li pracovat s migrací, můžete použít **Konzola správce balíčků** (pomocí PMC) nebo rozhraní příkazového řádku (CLI).  Tyto kurzy ukazují, jak používat rozhraní příkazového řádku. Informace o pomocí PMC je na [konci tohoto kurzu](#pmc).

Nástroje EF pro rozhraní příkazového řádku (CLI) jsou uvedeny v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). K instalaci tohoto balíčku, přidejte ho do `DotNetCliToolReference` kolekce v *.csproj* souboru, jak je vidět. **Poznámka:** je nutné nainstalovat tento balíček úpravou *.csproj* souboru; nelze použít `install-package` příkaz nebo grafického uživatelského rozhraní Správce balíčků. Můžete upravit *.csproj* kliknutím pravým tlačítkem myši na název projektu v souboru **Průzkumníku řešení** a výběrem **upravit ContosoUniversity.csproj**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(Číslo verze v tomto příkladu byly aktuální v době kurzu byla zapsána.) 

## <a name="change-the-connection-string"></a>Změnit připojovací řetězec

V *appSettings.JSON určený* souboru, změňte název databáze v připojovacím řetězci ContosoUniversity2 nebo jiný název, který nepoužili na počítači, který používáte.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Tato změna nastaví projekt tak, aby první migrací vytvoří novou databázi. To není nutné začít s migrací, ale se zobrazí později proto je vhodné.

> [!NOTE]
> Jako alternativu k změnit název databáze můžete odstranit databázi. Použití **Průzkumník objektů systému SQL Server** (SSOX) nebo `database drop` rozhraní příkazového řádku příkaz:
> ```console
> dotnet ef database drop
> ```
> V následující části vysvětluje, jak spouštět příkazy příkazového řádku.

## <a name="create-an-initial-migration"></a>Vytvoření počáteční migrace

Uložte změny a sestavte projekt. Potom otevřete okno příkazového řádku a přejděte do složky projektu. Tady je rychlý způsob, jak to udělat:

* V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte možnost **otevřít v Průzkumníku souborů** v místní nabídce.

  ![Otevřít v Průzkumníkovi souborů položky nabídky](migrations/_static/open-in-file-explorer.png)

* Na panelu Adresa zadejte "cmd" a stiskněte klávesu Enter.

  ![Otevřete příkazové okno](migrations/_static/open-command-window.png)

Do příkazového řádku zadejte následující příkaz:

```console
dotnet ef migrations add InitialCreate
```

Zobrazí výstup v příkazovém okně takto:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Pokud se zobrazí chybová zpráva *žádný spustitelný soubor nalezen odpovídající příkaz "dotnet-ef"*, najdete v části [tomto příspěvku na blogu](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) pomoc při řešení potíží.

Pokud se zobrazí chybová zpráva "*nemůže přistupovat k souboru... ContosoUniversity.dll vzhledem k tomu, že je stále používán jiným procesem.* ", najít na službě IIS Express ikonu na hlavním panelu systému Windows a pravým tlačítkem myši a pak klikněte na tlačítko **ContosoUniversity > Stop lokality**.

## <a name="examine-the-up-and-down-methods"></a>Zkontrolujte nahoru a dolů metody

Při provedení `migrations add` příkaz EF generovaného kódu, který vytvoří databázi od začátku. Tento kód je v *migrace* složku, v souboru s názvem  *\<časové razítko > _InitialCreate.cs*. `Up` Metodu `InitialCreate` třída vytvoří databázové tabulky, které odpovídají sady dat modelu entity a `Down` metoda odstraní, jak je znázorněno v následujícím příkladu.

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Migrace volání `Up` metody k implementaci změny modelu dat pro migraci. Když zadáte příkaz k vrácení aktualizace, migrace volání `Down` metoda.

Tento kód je pro počáteční migrace, která byla vytvořena, když jste zadali `migrations add InitialCreate` příkaz. Parametr name migrace ("InitialCreate" v příkladu) se používá pro název souboru a může být libovolně. Doporučujeme vybrat slovo nebo frázi, která shrnuje, co probíhá při migraci. Například můžete třeba pojmenovat novější migrace "AddDepartmentTable".

Pokud jste vytvořili počáteční migrace, když databáze již existuje, se vygeneruje kód pro vytvoření databáze, ale nemá spustit, protože databáze již odpovídá datový model. Když nasadíte aplikaci do jiného prostředí, kde databáze ještě neexistuje, tento kód spustí k vytvoření databáze, proto je vhodné se nejdřív otestovat. To je důvod, proč jste změnili název databáze v připojovacím řetězci dříve – tak, aby migrace můžete vytvořit novou od začátku.

## <a name="examine-the-data-model-snapshot"></a>Zkontrolujte snímku modelu dat

Migrace taky vytvoří *snímku* z aktuální schéma databáze v *Migrations/SchoolContextModelSnapshot.cs*. Tady je tento kód vypadá takto:

[!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Vzhledem k tomu, že aktuální schéma databáze je reprezentována v kódu, EF základní nemusí pracovat s databází pro vytvoření migrace. Když přidáte migrace, EF Určuje, co se změnilo tak, že porovnáte datový model, který soubor snímku. EF komunikuje s databází jenom v případě, že má k aktualizaci databáze. 

Soubor snímku musí být synchronizovány s migrací, které vytvářejí, takže nelze odebrat migrace právě odstraněním soubor s názvem  *\<časové razítko > _\<migrationname > .cs*. Pokud odstraníte tento soubor, zbývající migrace bude synchronizován s soubor snímku databáze. Chcete-li odstranit poslední migrace, který jste přidali, použijte [odebrat dotnet ef migrace](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) příkaz.

## <a name="apply-the-migration-to-the-database"></a>Použití migrace k databázi

V okně příkazového řádku zadejte následující příkaz pro vytvoření databáze a tabulky v ní.

```console
dotnet ef database update
```

Výstup z tohoto příkazu je podobná `migrations add` příkaz, s tím rozdílem, že pro příkazy SQL, která nastavení databáze naleznete v protokolech. Většina protokoly byly vynechány v následující ukázce výstupu. Pokud dáváte přednost není zobrazíte tato úroveň podrobností ve zprávách protokolu, můžete změnit úroveň protokolu v *appsettings. Development.JSON* souboru. Další informace najdete v tématu [Úvod k protokolování](xref:fundamentals/logging/index).

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

Použití **Průzkumník objektů systému SQL Server** Kontrola databáze, stejně jako v prvním kurzu.  Můžete si všimnout přidání tabulky __EFMigrationsHistory udržuje informace o migrace, které byly použity k databázi. Zobrazení dat v této tabulce a zobrazí jeden řádek na první migraci. (V poslední protokolu v předchozím příkladu výstupu rozhraní příkazového řádku se zobrazuje příkaz INSERT, která vytváří tento řádek.)

Spusťte aplikaci k ověření, že všechno funguje stále stejná jako předtím.

![Studenti, kteří indexovou stránku](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Rozhraní příkazového řádku (CLI) vs. Konzola správce balíčků (pomocí PMC)

EF nástrojů pro správu migrace je k dispozici z rozhraní .NET Core příkazového řádku nebo rutiny prostředí PowerShell v sadě Visual Studio **Konzola správce balíčků** okno (pomocí PMC). Tento kurz ukazuje, jak používat rozhraní příkazového řádku, ale můžete pomocí PMC Pokud dáváte přednost.

EF příkazů pro příkazy pomocí PMC jsou v [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) balíčku. Tento balíček je již součástí [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, takže není nutné ji nainstalovat.

**Důležité:** tento není stejného balíčku jako instalace pro rozhraní příkazového řádku úpravou *.csproj* souboru. Název touto končí v `Tools`, na rozdíl od název balíčku rozhraní příkazového řádku, které končí na `Tools.DotNet`.

Další informace o rozhraní příkazového řádku najdete v tématu [.NET Core rozhraní příkazového řádku](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet). 

Další informace o příkazech pomocí PMC najdete v tématu [Konzola správce balíčků (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Souhrn

V tomto kurzu jste viděli, jak vytvořit a použít první migrace. V dalším kurzu brzy prohlížení pokročilejší témata rozšířením datový model. Na této cestě můžete vytvářet a použijte další migrace.

>[!div class="step-by-step"]
[Předchozí](sort-filter-page.md)
[další](complex-data-model.md)  
