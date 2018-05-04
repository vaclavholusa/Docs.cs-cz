---
title: Jádro ASP.NET MVC s EF Core - rozšířené – 10 z 10
author: tdykstra
description: Tento kurz představuje užitečné témata pro více než se základy vývoje webové aplikace ASP.NET Core, které používají Entity Framework Core.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/advanced
ms.openlocfilehash: 655f60116cbfe1dd81b7e2855906446b919b6489
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="aspnet-core-mvc-with-ef-core---advanced---10-of-10"></a>Jádro ASP.NET MVC s EF Core - rozšířené – 10 z 10

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).

V tomto kurzu předchozí implementována tabulky na hierarchii dědičnosti. V tomto kurzu zavádí několik témat, které jsou vhodné pro zajímat, když přejdete mimo základní informace o vývoj webových aplikací ASP.NET Core, které používají základní Entity Framework.

## <a name="raw-sql-queries"></a>Nezpracovaná SQL dotazy

Jednou z výhod použití rozhraní Entity Framework je, že zabraňuje příkazů kódu příliš úzce na konkrétní metodu ukládání dat. Dělá to pomocí generování SQL dotazy a příkazy, které také s není pro zápis sami. Ale existují výjimečných scénáře, kdy budete muset spustit konkrétní dotazy SQL, které jste vytvořili ručně. Pro tyto scénáře prvního rozhraní API sady Entity Framework kód obsahuje metody, které vám umožní předat příkazy SQL přímo do databáze. Ve verzi 1.0 základní EF máte následující možnosti:

* Použití `DbSet.FromSql` metoda pro dotazy, které vracejí typy entit. Vrácených objektů musí být na typ očekávaný `DbSet` objektu a jste automaticky sleduje kontext databáze Pokud jste [vypnout sledování](crud.md#no-tracking-queries).

* Použití `Database.ExecuteSqlCommand` pro příkazy nejsou dotazem.

Pokud potřebujete spustit dotaz, který vrátí typy, které nejsou entity, můžete pomocí připojení k databázi poskytované EF ADO.NET. Vrácená data není sledována kontext databáze, i když používáte tuto metodu pro načtení typů entit.

Jak platí vždy při spuštění příkazů SQL ve webové aplikaci, je nutné provést opatření k ochraně vaší lokality před útoky Injektáž SQL. Jeden způsob, jak to udělat, je použití parametrických dotazů a ujistěte se, že řetězců odeslána webová stránka se nedá interpretovat jako příkazy SQL. V tomto kurzu budete používat parametrizované dotazy při integraci uživatelský vstup do dotazu.

## <a name="call-a-query-that-returns-entities"></a>Volání dotaz, který vrací entity

`DbSet<TEntity>` Třída poskytuje metodu, která můžete použít k provedení dotazu, který vrací entity typu `TEntity`. Pokud chcete zobrazit, jak to funguje můžete budete změnit kód v `Details` metoda řadiče oddělení.

V *DepartmentsController.cs*v `Details` metoda, nahraďte kód, který načte oddělení s `FromSql` volání metody, jak je znázorněno v následující zvýrazněný kód:

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

Chcete-li ověřit, že nový kód funguje správně, vyberte **oddělení** kartu a potom **podrobnosti** pro jednu z oddělení.

![Podrobnosti o oddělení](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a>Volání dotaz, který vrátí jiné typy

Dříve jste vytvořili mřížka student statistiky o stránky, která vám ukázal, počet studenty pro každé datum registrace. Vy máte data ze sady entit studenty (`_context.Students`) a použít LINQ do projektu výsledky do seznamu `EnrollmentDateGroup` zobrazit objekty modelu. Předpokládejme, že chcete zapsat SQL samotné místo pomocí LINQ. Chcete-li provést, je nutné ke spuštění dotazu SQL, vrátí něco jiného než entity objektů. Jeden způsob, jak to udělat v EF základní 1.0, je napsat kód ADO.NET a získat připojení k databázi z EF.

V *HomeController.cs*, nahraďte `About` metoda následujícím kódem:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

Přidat pomocí příkazu:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

Spusťte aplikaci a přejděte na stránku o. Zobrazuje stejná data, která předtím.

![O stránku](advanced/_static/about.png)

## <a name="call-an-update-query"></a>Volání příkazu update jazyka

Předpokládejme, že chcete provést globální změny v databázi, jako je například změna číslo kredity u každé kurzu Contoso univerzity správci. Pokud univerzity má velký počet kurzy, je neefektivní je načtou všechny jako entity a měnit je jednotlivě. V této části budete implementovat na webové stránce, která umožňuje uživateli zadat faktor, podle kterého chcete změnit číslo kredity u všech kurzů a budete proveďte požadovanou změnu spuštěním příkazu SQL UPDATE. Webová stránka bude vypadat jako na následujícím obrázku:

![Kredity během stránku aktualizace](advanced/_static/update-credits.png)

V *CoursesContoller.cs*, přidejte metody UpdateCourseCredits pro třídy MetadataExchangeClientMode a HttpPost:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

Pokud kontroler zpracovává požadavek třídy MetadataExchangeClientMode, nic nevrátí v `ViewData["RowsAffected"]`, a zobrazení zobrazí prázdné textové pole a tlačítko pro odeslání, jak je vidět na předchozím obrázku.

Když **aktualizace** po kliknutí na tlačítko, volání metody HttpPost a násobitel má hodnota zadaná v textovém poli. Kód pak provede SQL, který aktualizuje kurzy a vrátí počet ovlivněných řádků zobrazení v `ViewData`. Při zobrazení získá `RowsAffected` hodnotu, zobrazí počet aktualizované řádky.

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *zobrazení/kurzy* složku a pak klikněte na tlačítko **Přidat > Nová položka**.

V **přidat novou položku** dialogové okno, klikněte na tlačítko **ASP.NET** pod **nainstalovaná** v levém podokně klikněte na **stránka zobrazení MVC**a název nového zobrazení  *UpdateCourseCredits.cshtml*.

V *Views/Courses/UpdateCourseCredits.cshtml*, nahraďte kód šablony s následujícím kódem:

[!code-html[](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

Spustit `UpdateCourseCredits` metoda výběrem **kurzy** kartě následným přidáním "/ UpdateCourseCredits" na konec adresy URL v adresním řádku prohlížeče (například: `http://localhost:5813/Courses/UpdateCourseCredits`). Zadejte číslo do textového pole:

![Kredity během stránku aktualizace](advanced/_static/update-credits.png)

Klikněte na tlačítko **aktualizace**. Zobrazí počet ovlivněných řádků:

![Počet ovlivněných řádků kredity během stránku aktualizace](advanced/_static/update-credits-rows-affected.png)

Klikněte na tlačítko **zpět do seznamu** zobrazíte seznam kurzy revidovaný počet kredity.

Všimněte si, že produkčním kódu by zajistěte, aby, aktualizuje vždy vést platná data. Kód zjednodušené zobrazeny zde může vynásobte počet kredity dostatečně za následek větší než 5 číslic. ( `Credits` Vlastnost má `[Range(0, 5)]` atributů.) Aktualizace dotazu by fungovat, ale neplatná data může vést k neočekávaným výsledkům v dalších částech tohoto systému, které předpokládají, že počet kredity je 5 nebo méně.

Další informace o nezpracovanou dotazy SQL najdete v tématu [nezpracovaná dotazy SQL](https://docs.microsoft.com/ef/core/querying/raw-sql).

## <a name="examine-sql-sent-to-the-database"></a>Zkontrolujte odeslal do databáze SQL

V některých případech je vhodné se moci zobrazit skutečné dotazy SQL, které se odesílají do databáze. Funkce integrované protokolování pro ASP.NET Core automaticky použije EF základní zápis protokolů, které obsahují SQL pro dotazy a aktualizace. V této části se zobrazí některé příklady protokolování SQL.

Otevřete *StudentsController.cs* a v `Details` metoda nastavit zarážky `if (student == null)` příkaz.

Spuštění aplikace v režimu ladění a přejděte na stránku podrobností pro student.

Přejděte na **výstup** okno zobrazení ladění výstupu a zobrazit dotaz:

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

Můžete si všimnout něco tady, ať vás může překvapí: až 2 řádků vybere SQL (`TOP(2)`) z tabulky osob. `SingleOrDefaultAsync` Metoda se nepřekládá na 1 řádek na serveru. Tady je důvod, proč:

* Pokud dotaz vrátí více řádků, metoda vrátí hodnotu null.
* Pokud chcete zjistit, jestli dotaz vrátí více řádků, EF je nutné zkontrolovat, když vrátí alespoň 2.

Všimněte si, že nemusíte používat režim ladění a zastavit zarážku získat výstup protokolování v **výstup** okno. Je jenom pohodlný způsob zastavení protokolování v místě, které se chcete podívat na výstup. Pokud jste si udělat, pokračuje protokolování a budete muset posuňte se zpět k vyhledání částí, které vás zajímají.

## <a name="repository-and-unit-of-work-patterns"></a>Úložiště a jednotky pracovních vzorů

Celá řada vývojářů napsat kód pro implementaci úložiště a jednotky pracovních vzorů jako obálku kolem kód, který funguje s platformou Entity Framework. Tyto vzory jsou určeny k vytvoření vrstvy abstrakce mezi vrstva přístupu k datům a vrstvu obchodní logiky aplikace. Implementace tyto vzory pomůžou izolovat aplikace od změny v úložišti dat a mohou usnadnit automatizované testování částí nebo testy řízený vývoj (TDD). Ale zápis další kód implementovat tyto vzory není vždy je nejlepší pro aplikace, které používají EF, z několika důvodů:

* Vlastní třídy kontextu EF insulates kódu z daného obchodu data kódu.

* Třída kontextu EF může fungovat také třídu pracovní jednotky pro databázi aktualizace, který používáte pomocí EF.

* EF obsahuje funkce pro implementaci TDD bez nutnosti psaní kódu úložiště.

Informace o tom, jak implementovat úložiště a jednotky pracovních vzorů najdete v tématu [verze Entity Framework 5 tohoto kurzu řady](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).

Entity Framework Core implementuje poskytovatele databáze v paměti, který lze použít pro testování. Další informace najdete v tématu [testu s InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).

## <a name="automatic-change-detection"></a>Detekce automatických změn

Rozhraní Entity Framework Určuje, jak došlo ke změně entity (a proto aktualizací, které potřebují k odeslání do databáze) tak, že porovnáte aktuální hodnoty entity s původními hodnotami. Původní hodnoty jsou uloženy, když se entita dotaz nebo připojené. Některé metody, které způsobí změnu automatické zjišťování jsou následující:

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker.Entries

Pokud sledujete velký počet entit a jednu z těchto metod zavoláte mnohokrát ve smyčce, můžete dosáhnout výrazné vylepšení výkonu dočasně vypnout pomocí zjišťování automatické změny `ChangeTracker.AutoDetectChangesEnabled` vlastnost. Příklad:

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a>Plány Entity Framework Core zdrojového kódu a vývoj

Zdroj Entity Framework Core je v [ https://github.com/aspnet/EntityFrameworkCore ](https://github.com/aspnet/EntityFrameworkCore). Úložiště EF základní obsahuje noční sestavení, sledování problémů, funkce specifikace, návrh poznámky, ze schůzky a [plán pro budoucí vývoj](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap). Můžete soubor nebo najít chyby a přispívat.

I když zdrojový kód je otevřená, Entity Framework Core je plně podporovaný jako produkt společnosti Microsoft. Týmem Microsoft Entity Framework udržuje řízení, po kterou jsou přijímány příspěvky a otestuje všechny změny kódu k zajištění kvality jednotlivými verzemi.

## <a name="reverse-engineer-from-existing-database"></a>Zpětné analýzy z existující databáze

Pro zpětnou datového modelu, včetně tříd entit z existující databáze, použijte [vygenerované uživatelské rozhraní dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) příkaz. Najdete v článku [kurzu Začínáme](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a>Použití dynamické LINQ ke zjednodušení řazení výběr kódu

[Třetí kurzu této série](sort-filter-page.md) ukazuje, jak napsat kód LINQ podle pevně kódováno názvy sloupců v `switch` příkaz. Se dvěma sloupci můžete vybírat funguje bez problémů, ale pokud máte mnoho sloupců kód může získat podrobné. Chcete-li tento problém vyřešit, můžete použít `EF.Property` metoda zadat název vlastnosti jako řetězec. Chcete-li vyzkoušet tento přístup, nahraďte `Index` metoda v `StudentsController` následujícím kódem.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a>Další kroky

Dokončení této série kurzů na použití Entity Framework Core v aplikaci ASP.NET MVC.

Další informace o základní EF najdete v tématu [Entity Framework základní dokumentace](https://docs.microsoft.com/ef/core). Knihy je také k dispozici: [Entity Framework Core v akci](https://www.manning.com/books/entity-framework-core-in-action).

Informace o tom, jak nasadit webovou aplikaci najdete v tématu [hostitele a nasazení](xref:host-and-deploy/index).

Informace o dalších témat souvisejících s ASP.NET MVC jádra, jako je například ověřování a autorizaci, najdete v článku [ASP.NET Core dokumentaci](xref:index).

## <a name="acknowledgments"></a>Potvrzování

Tní Dykstra a Rick Anderson (twitter @RickAndMSFT) napsali v tomto kurzu. Rowan Lukeš, Diego Vega a ostatní členové týmu rozhraní Entity Framework s asistencí s recenze kódu a pomohl ladění problémů, které vznikly při jsme byly pro kurzů k psaní kódu.

## <a name="common-errors"></a>Běžné chyby  

### <a name="contosouniversitydll-used-by-another-process"></a>ContosoUniversity.dll využívá jiný proces

Chybová zpráva:

> Nelze otevřít.... bin\Debug\netcoreapp1.0\ContosoUniversity.dll' pro zápis – ' proces nemá přístup k souboru '... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll, protože je stále používán jiným procesem.

Řešení:

Zastavte lokalitu ve službě IIS Express. Přejděte k panelu systému Windows najít službu IIS Express, klikněte pravým tlačítkem na ikonu, vyberte lokalitu, univerzity Contoso a potom klikněte **zastavit lokality**.

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>Migrace vygeneroval s žádný kód v nahoru a dolů metody

Možná příčina:

Příkazy rozhraní příkazového řádku EF není automaticky zavřete a uložte soubory kódu. Pokud máte neuložené změny při spuštění `migrations add` příkaz EF nebude možné najít změny.

Řešení:

Spustit `migrations remove` příkaz, uložte změny kódu a znovu spusťte `migrations add` příkaz.

### <a name="errors-while-running-database-update"></a>Při spuštěné aktualizaci databáze došlo k chybám

Je možné získat další chyby při provádění změn schématu v databázi, která obsahuje stávající data. Pokud dojde k chybám migrace, které nelze vyřešit, můžete změnit název databáze v připojovacím řetězci nebo odstraňte tuto databázi. S novou databázi nejsou žádná data k migraci a příkazu update-database je mnohem pravděpodobnější dokončeno bez chyb.

Nejjednodušší způsob je přejmenovat databázi v *appSettings.JSON určený*. Při příštím spuštění `database update`, bude vytvořena nová databáze.

Chcete-li odstranit databázi v SSOX, klikněte pravým tlačítkem na databázi, klikněte na **odstranit**a potom v **odstranit databázi** dialogové okno pole vyberte **ukončete stávající připojení** a klikněte na tlačítko  **OK**.

Chcete-li odstranit databázi pomocí rozhraní příkazového řádku, spusťte `database drop` rozhraní příkazového řádku příkaz:

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>Chyba vyhledáním instance systému SQL Server

Chybová zpráva:

> Při navazování připojení k systému SQL Server došlo k chybě související se sítí nebo konkrétní instancí. Server nebyl nalezen nebo není přístupný. Ověřte, zda je název instance správný a zda systém SQL Server je nakonfigurován k povolení vzdálených připojení. (Zprostředkovatel: rozhraní sítě systému SQL, chyba: 26 - chyba vyhledání Server nebo instanci zadáno)

Řešení:

Zkontrolujte připojovací řetězec. Pokud jste odstranili ručně databázový soubor, změňte název databáze v řetězci konstrukce začít znovu s novou databázi.

> [!div class="step-by-step"]
> [Předchozí](inheritance.md)
