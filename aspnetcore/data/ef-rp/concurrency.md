---
title: "Stránky Razor EF základní - souběžnosti - 8 8"
author: rick-anderson
description: "Tento kurz ukazuje způsobu řešení konfliktů, když se více uživatelů aktualizace stejné entity ve stejnou dobu."
keywords: "ASP.NET Core Entity Framework Core, souběžnosti"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: 0c49376fd1b602fe03ef2a152d19b58513ae2710
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
en-us /

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a>Zpracování konfliktů souběžnosti – základní EF s stránky Razor (8 8)

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [tní Dykstra](https://github.com/tdykstra), a [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Tento kurz ukazuje způsobu řešení konfliktů při více uživatelů aktualizovat entitu, souběžně (ve stejnou dobu). Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).

## <a name="concurrency-conflicts"></a>Konflikty souběžnosti

Dojde ke konfliktu souběžnosti při:

* Uživatel přejde na stránku upravit pro entitu.
* Jiný uživatel aktualizuje na stejnou entitu před první uživatel změnu je zapsán do databáze.

Pokud detekce souběžnosti není povolena, když dojde k Souběžná aktualizace:

* Poslední aktualizace služby wins. To znamená poslední aktualizace hodnoty se uloží do databáze.
* První z aktuální aktualizace budou ztraceny.

### <a name="optimistic-concurrency"></a>Optimistickou metodu souběžného zpracování

Optimistickou metodu souběžného umožňuje konfliktů souběžnosti provést, a pak reaguje správně při dělají. Například Jana navštíví stránce Upravit oddělení a mění nároky pro angličtinu oddělení z $350,000.00 0,00 Kč.

![Změna nároky na 0](concurrency/_static/change-budget.png)

Před kliknutím na Jana **Uložit**, Jan navštíví stejné stránce a změny pole Počáteční datum 9/1/2013 z 9/1/2007.

![Změna počáteční datum na 2013](concurrency/_static/change-date.png)

Jana klikne **Uložit** první a zobrazí ji změnit, když prohlížeč zobrazí indexovou stránku.

![Nároky změnit tak, aby nula.](concurrency/_static/budget-zero.png)

Jan klikne **Uložit** na stránku úpravy, která se zobrazuje nároky $350,000.00. Co se stane dále je určen jak zpracování konfliktů souběžnosti.

Optimistickou metodu souběžného zahrnuje následující možnosti:

* Můžete udržovat přehled o vlastností, které uživatel změnil a aktualizovat na odpovídající sloupce v databázi.

 Ve scénáři bude ztracena žádná data. Různé vlastnosti byly aktualizovány dva uživatelé. Při příštím někdo umožňuje anglické oddělení, se zobrazí na Jana i Jan pro změny. Tato metoda aktualizace může snížit počet konfliktům, ke kterým může dojít ke ztrátě dat. Tento postup: * nelze nedošlo ke ztrátě dat. Pokud konkurenční změn stejnou vlastnost.
        * Je obecně není praktické ve webové aplikaci. To vyžaduje údržbu významné stavu k uchovávání informací o všech načtených hodnoty a nové hodnoty. Správa velkých objemů stavu může ovlivnit výkon aplikace.
        * Může zvýšit složitost aplikace ve srovnání s detekce souběžnosti na entitu.

* Můžete je nechat Jan pro změnu Jana změna přepsána.

 Při příštím někdo umožňuje anglické oddělení, se zobrazí 9/1/2013 a načtených $350,000.00 hodnotu. Tato metoda se nazývá *klienta Wins* nebo *poslední ve službě Wins* scénář. (Všechny hodnoty z klienta přednost co je v úložišti.) Pokud neprovedete žádné kódování pro zpracování souběžnosti, Wins, klient se automaticky.

* Jan pro změnu může zabránit aktualizaci v databázi. Obvykle se aplikace by: * Zobrazí se chybová zpráva.
        * Zobrazit aktuální stav data.
        * Povolí uživateli znovu použít změny.

 Tento postup se nazývá *Wins úložiště* scénář. (Hodnoty úložiště dat mají přednost před odeslané klientem hodnoty.) V tomto kurzu implementujete scénář úložiště služby Wins. Tato metoda zajišťuje, že žádné změny budou přepsána bez uživatele se zobrazí upozornění.

## <a name="handling-concurrency"></a>Zpracování souběžnosti 

Pokud je vlastnost nakonfigurovaný jako [token souběžnosti](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):

* EF základní ověřuje, že vlastnost neupravoval po se načetla. Kontrola dojde při [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) nebo [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) je volána.
* Pokud vlastnost byla změněna od jeho tabulky se načetla, [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) je vyvolána výjimka. 

Databáze a datového modelu musí být nakonfigurované pro podporu vyvolání `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Zjišťování konfliktů souběžnosti u vlastnosti

Může být zjistil konflikt souběžnosti na úrovni vlastnost s [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) atribut. Atribut je použít pro více vlastností v modelu. Další informace najdete v tématu [Data poznámky-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).

`[ConcurrencyCheck]` Atribut není použit v tomto kurzu.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Zjišťování konfliktů souběžnosti na řádek

Ke zjišťování konfliktů souběžnosti, [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) sledování sloupce se přidá do modelu.  `rowversion` :

* SQL Server se konkrétní. Ostatní databáze nemusí poskytují podobné funkce.
* Slouží k určení, že entita nebyla změněna od načtení z databáze. 

Databáze generuje sekvenční `rowversion` číslo, které se zvýší, řádek pokaždé, když se aktualizuje. V `Update` nebo `Delete` příkaz, `Where` klauzule zahrnuje načtených hodnotu `rowversion`. Pokud došlo ke změně řádku:

 * `rowversion`neodpovídá hodnotě načtených.
 * `Update` Nebo `Delete` příkazů nelze nalézt řádek, protože `Where` klauzule zahrnuje načtených `rowversion`.
 * A `DbUpdateConcurrencyException` je vyvolána výjimka.

V EF Core, když žádné řádky bylo aktualizováno správcem `Update` nebo `Delete` příkaz, je vyvolána výjimka souběžnosti.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Přidat vlastnost sledování do oddělení entity

V *Models/Department.cs*, přidejte sledování vlastnost s názvem RowVersion:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[Časové razítko](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) atribut určuje, zda je tento sloupec součástí `Where` klauzuli `Update` a `Delete` příkazy. Atribut se nazývá `Timestamp` protože předchozí verze systému SQL Server používá SQL `timestamp` datového typu než SQL `rowversion` typ nahradí ji.

Rozhraní fluent API můžete také určit vlastnosti sledování:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

Následující kód ukazuje část vytvářené EF jádra, když se aktualizuje název oddělení T-SQL:

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

Podle předchozích zvýrazněná kód ukazuje `WHERE` obsahující klauzuli `RowVersion`. Pokud databáze `RowVersion` se nerovná `RowVersion` parametr (`@p2`), jsou aktualizovány žádné řádky.

Následující zvýrazněný kód ukazuje T-SQL, která ověřuje, že byla aktualizována přesně jeden řádek:

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) vrátí počet řádků, které jsou ovlivněné poslední příkaz. V žádné řádky jsou aktualizovány, vyvolá EF jádra `DbUpdateConcurrencyException`.

Uvidíte, že generuje základní EF T-SQL v okně výstupu sady Visual Studio.

### <a name="update-the-db"></a>Aktualizace databáze

Přidávání `RowVersion` změny vlastností DB modelu, který vyžaduje migrace.

Sestavte projekt. V okně příkazového řádku zadejte následující údaje:

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

Předchozí příkazy:

* Přidá *migrace / {čas stamp}_RowVersion.cs* souboru migrace.
* Aktualizace *Migrations/SchoolContextModelSnapshot.cs* souboru. Aktualizace přidává následující zvýrazněný kód do `BuildModel` metoda:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* Spustí migrace k aktualizaci databáze.

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>Vygenerované uživatelské rozhraní modelu oddělení

* Ukončete aplikaci Visual Studio.
* Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).
* Spusťte následující příkaz:

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

Předchozí příkaz scaffold `Department` modelu. Otevřete projekt v sadě Visual Studio.

Sestavte projekt. Sestavení generuje chyby takto:

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Globálně změnit `_context.Department` k `_context.Departments` (tedy "s" přidat do `Department`). 7 výskytů jsou vyhledána a aktualizovat.

### <a name="update-the-departments-index-page"></a>Aktualizace oddělení indexovou stránku

Modul generování uživatelského rozhraní, který je vytvořen `RowVersion` sloupec indexovou stránku, ale toto pole by se neměly zobrazovat. V tomto kurzu, poslední bajt `RowVersion` se zobrazí pro lepší porozumění tomu souběžnosti. Poslední bajt není musí být jedinečný. Skutečné aplikaci, nezobrazí se `RowVersion` nebo poslední bajt `RowVersion`.

Aktualizace indexovou stránku:

* Nahraďte Index oddělení.
* Nahraďte kód obsahující `RowVersion` s poslední bajt `RowVersion`.
* Nahraďte FirstMidName FullName.

Následující kód ukazuje aktualizovanou stránku:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Aktualizace modelu stránky úpravy

Aktualizace *pages\departments\edit.cshtml.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Ke zjištění souběžnosti problém, [původní hodnota](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) je aktualizován `rowVersion` se načetla byla hodnota z entity. Základní EF generuje příkazu SQL UPDATE s klauzulí WHERE, která obsahuje původní `RowVersion` hodnota. Pokud se příkaz UPDATE žádné řádky (žádné řádky mají původní `RowVersion` hodnotu), `DbUpdateConcurrencyException` je vyvolána výjimka.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

V předchozí kód `Department.RowVersion` je hodnota, když se načetla entity. `OriginalValue`je hodnota v databázi při `FirstOrDefaultAsync` byla volána v této metodě.

Následující kód získá hodnoty klienta (hodnoty odeslány této metody) a hodnoty DB:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

Toto kód přidá vlastní chybové zprávy pro každý sloupec, který má DB hodnoty liší od co byla odeslána do `OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Následující zvýrazněnou sady kódu `RowVersion` hodnotu na novou hodnotu načíst z databáze. Při příštím kliknutí na tlačítko **Uložit**, pouze souběžného zpracování chyb, které dojít, protože vzniká, poslední zobrazení stránky upravit.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

`ModelState.Remove` Příkaz není nutná, protože `ModelState` má starý `RowVersion` hodnotu. Na stránce Razor `ModelState` hodnota pole má přednost před hodnoty vlastností modelu, pokud obě existuje.

## <a name="update-the-edit-page"></a>Upravit stránku aktualizace

Aktualizace *Pages/Departments/Edit.cshtml* s následující kód:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Předchozí kód:

* Aktualizace `page` direktivy z `@page` k `@page "{id:int}"`.
* Přidá verze skrytá řádku. `RowVersion`je nutné přidat tak post zpět váže hodnotu.
* Zobrazí poslední bajt `RowVersion` pro účely ladění.
* Nahradí `ViewData` s silného typu `InstructorNameSL`.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Test je v konfliktu s stránce Upravit souběžnosti

Otevřete dvě instance upravit prohlížeče, na angličtinu oddělení:

* Spusťte aplikaci a vyberte oddělení.
* Klikněte pravým tlačítkem myši **upravit** hypertextový odkaz pro angličtinu oddělení a vyberte **otevřít na nové kartě**.
* Na první kartě, klikněte **upravit** hypertextový odkaz pro angličtinu oddělení.

Karty dvě prohlížeč zobrazí stejné informace.

Změňte název první záložce prohlížeče a klikněte na tlačítko **Uložit**.

![Upravit oddělení stránka 1 po změně](concurrency/_static/edit-after-change-1.png)

Prohlížeč zobrazí indexovou stránku s změněné hodnoty a aktualizované rowVersion indikátoru. Všimněte si aktualizované rowVersion indikátoru, se zobrazí na druhém zpětné volání na jiné kartě.

Změňte jiné pole druhý záložce prohlížeče.

![Upravit oddělení stránka 2 po změně](concurrency/_static/edit-after-change-2.png)

Klikněte na tlačítko **Uložit**. Zobrazí chybové zprávy pro všechna pole, které se neshodují s hodnotami DB:

![Oddělení upravit stránku chybová zpráva](concurrency/_static/edit-error.png)

Chcete-li změnit název pole nechtěli tohoto okna prohlížeče. Zkopírujte a vložte aktuální hodnota (jazyky) do pole název. Klikněte na. Ověřování na straně klienta odebere chybovou zprávu.

![Oddělení upravit stránku chybová zpráva](concurrency/_static/cv.png)

Klikněte na tlačítko **Uložit** znovu. Hodnota, kterou jste zadali na kartě druhý prohlížeče je uložit. Zobrazí uložené hodnoty na stránce indexu.

## <a name="update-the-delete-page"></a>Odstranit stránku aktualizace

Aktualizace modelu odstranění stránek s následujícím kódem:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

Odstranit stránky rozpozná souběžnosti je v konfliktu, pokud entity se změnila po se načetla. `Department.RowVersion`verze řádku je, když se načetla entity. Když EF základní vytváří příkaz SQL odstranit, obsahuje klauzuli WHERE s `RowVersion`. Pokud vliv na výsledky příkazu SQL odstranit v nulový počet řádků:

* `RowVersion` V SQL odstranit příkaz neodpovídá `RowVersion` v databázi.
* Je vyvolána výjimka DbUpdateConcurrencyException.
* `OnGetAsync`je volána `concurrencyError`.

### <a name="update-the-delete-page"></a>Odstranit stránku aktualizace

Aktualizace *Pages/Departments/Delete.cshtml* následujícím kódem:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


Předchozí kód provede tyto změny:

* Aktualizace `page` direktivy z `@page` k `@page "{id:int}"`.
* Přidá chybovou zprávu.
* Nahradí FullName v FirstMidName **správce** pole.
* Změny `RowVersion` zobrazíte poslední bajt.
* Přidá verze skrytá řádku. `RowVersion`je nutné přidat tak post zpět váže hodnotu.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Test je v konfliktu s stránce odstranit souběžnosti

Vytvořte testovací oddělení.

Otevřete dvě instance prohlížeče odstranit na testovací oddělení:

* Spusťte aplikaci a vyberte oddělení.
* Klikněte pravým tlačítkem myši **odstranit** hypertextový odkaz pro test oddělení a vyberte **otevřít na nové kartě**.
* Klikněte **upravit** hypertextový odkaz pro test oddělení.

Karty dvě prohlížeč zobrazí stejné informace.

Nároky na první kartě prohlížeče a klikněte na tlačítko **Uložit**.

Prohlížeč zobrazí indexovou stránku s změněné hodnoty a aktualizované rowVersion indikátoru. Všimněte si aktualizované rowVersion indikátoru, se zobrazí na druhém zpětné volání na jiné kartě.

Odstraňte testovací oddělení z druhé karty. Chyba souběžnosti je zobrazení pomocí aktuálních hodnot z databáze. Kliknutím na tlačítko **odstranit** odstraní entitu, pokud `RowVersion` byl updated.department byl odstraněn.

V tématu [dědičnosti](xref:data/ef-mvc/inheritance) pokyny o tom, jak dědičnosti v datovém modelu.

### <a name="additional-resources"></a>Další zdroje

* [Tokeny souběžnosti v EF jádra](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [Zpracování souběžnost v EF jádra](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[Předchozí](xref:data/ef-rp/update-related-data)
