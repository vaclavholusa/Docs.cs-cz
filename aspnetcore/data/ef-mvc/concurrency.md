---
title: "Jádro ASP.NET MVC s EF Core - souběžnosti - 8, 10"
author: tdykstra
description: "Tento kurz ukazuje způsobu řešení konfliktů, když se více uživatelů aktualizace stejné entity ve stejnou dobu."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 69ffafc7f92cda75c001fe1098275766063113fb
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a>Zpracování konfliktů souběžnosti – základní EF s kurz k ASP.NET MVC jádra (8 10)

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).

V dřívějších kurzy zjistili, jak aktualizovat data. Tento kurz ukazuje způsobu řešení konfliktů, když se více uživatelů aktualizace stejné entity ve stejnou dobu.

Vytvoříte webové stránky, které pracovat entity oddělení a zpracování chyb souběžnosti. Na následujících obrázcích je upravit a odstranit stránky, včetně některé zprávy, které se zobrazí, pokud dojde ke konfliktu souběžnosti.

![Stránka Upravit oddělení](concurrency/_static/edit-error.png)

![Odstranit stránky oddělení](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a>Konflikty souběžnosti

Concurrency dojde ke konfliktu jeden uživatel zobrazí entity data ji Pokud chcete upravit, a pak jiný uživatel aktualizuje stejné entity data před první uživatel změnu je zapsána do databáze. Pokud nepovolíte detekce takové konflikty, kdo aktualizuje databázi přepíše poslední změny jiného uživatele. V mnoha aplikacích, je přijatelné toto riziko: Pokud existuje několik uživatelů nebo několik aktualizací, nebo pokud není opravdu důležité, pokud jsou některé změny přepsány, náklady na programování pro concurrency vyváží výhody. V takovém případě nemáte konfigurace aplikace pro zpracování konfliktů souběžnosti.

### <a name="pessimistic-concurrency-locking"></a>Pesimistické souběžnosti (uzamčení)

Pokud aplikace třeba zabránit, aby před náhodnou ztrátou dat v concurrency scénáře, je jeden ze způsobů použití uzamčení databáze. Tomu se říká pesimistické souběžnosti. Například předtím, než se pustíte do čtení řádek z databáze, můžete požádat o zámku pro jen pro čtení nebo pro přístup k aktualizaci. Pokud zamknete řádek pro přístup k aktualizaci, je povoleno uzamčení řádek buď pro žádní jiní uživatelé jen pro čtení nebo aktualizace přístup, protože by získají kopii dat, která je právě probíhá změnit. Pokud zamknete řádek pro oprávnění jen pro čtení, ostatní také možné zamknout ho pro přístup jen pro čtení, ale není pro aktualizaci.

Správa zámky má nevýhody. Může být složité programu. Vyžaduje významné databáze správy prostředků, a jako počet uživatelů, aplikace, může to způsobit problémy s výkonem zvyšuje. Z těchto důvodů ne všechny databázové systémy podporovat pesimistické souběžnosti. Entity Framework Core poskytuje žádné integrovanou podporu pro něj, a v tomto kurzu není ukazují, jak ho implementovat.

### <a name="optimistic-concurrency"></a>Optimistickou metodu souběžného zpracování

Alternativa k pesimistické souběžnosti je optimistickou metodu souběžného. Optimistickou metodu souběžného znamená povolení konfliktů souběžnosti provést a reaktivní správně, pokud tomu tak je. Například Jana navštíví oddělení upravit stránku a změní velikost nároky pro angličtinu oddělení z $350,000.00 na 0,00 Kč.

![Změna nároky na 0](concurrency/_static/change-budget.png)

Před kliknutím na Jana **Uložit**, Jan navštíví stejné stránce a změny pole Počáteční datum 9/1/2013 z 9/1/2007.

![Změna počáteční datum na 2013](concurrency/_static/change-date.png)

Jana klikne **Uložit** první a zobrazí ji změnit, když prohlížeč vrátí na indexovou stránku.

![Nároky změnit tak, aby nula.](concurrency/_static/budget-zero.png)

Pak klikne na tlačítko Jan **Uložit** na stránku úpravy, která se zobrazuje nároky $350,000.00. Co se stane dále je určen jak zpracování konfliktů souběžnosti.

Mezi možnosti patří následující:

* Můžete udržovat přehled o vlastností, které uživatel změnil a aktualizovat na odpovídající sloupce v databázi.

     V ukázkovém scénáři by byl žádná data ztratí, protože různé vlastnosti aktualizoval dva uživatele. Při příštím někdo umožňuje anglické oddělení, se zobrazí na Jana i Jan pro změny – počáteční datum 9/1/2013 a nároky nulové dolarů. Tato metoda aktualizace může snížit počet konfliktům, ke kterým může dojít ke ztrátě dat, ale nemůže vyhnuli ztrátě dat, pokud konkurenční změn stejnou vlastnost entity. Jestli Entity Framework funguje takto závisí na tom, jak implementovat aktualizace kódu. Je často není praktické ve webové aplikaci, protože může vyžadovat, aby udržení přehledu o všechny původní hodnoty vlastností pro entitu a také nové hodnoty udržovat velkých objemů stavu. Zachování velkých objemů stavu může ovlivnit výkon aplikace, protože buď vyžaduje prostředky serveru nebo musí být součástí webové stránky (například v skrytá pole) nebo do souboru cookie.

* Můžete je nechat Jan pro změnu Jana změna přepsána.

     Při příštím někdo umožňuje anglické oddělení, se zobrazí 9/1/2013 a obnovený $350,000.00 hodnotu. Tento postup se nazývá *klienta Wins* nebo *poslední ve službě Wins* scénář. (Všechny hodnoty z klienta přednost co je v úložišti.) Jak jsme uvedli v Úvod do této části, pokud tak učiníte jakéhokoli kódování pro zpracování souběžnosti, to se stane automaticky.

* Jan pro změnu může zabránit aktualizaci v databázi.

     By obvykle, zobrazí se chybová zpráva, zobrazit jeho aktuální stav data a povolit mu jeho změny znovu použijte, pokud chce je provést. Tento postup se nazývá *Wins úložiště* scénář. (Hodnoty úložiště dat mají přednost před odeslané klientem hodnoty.) V tomto kurzu budete implementovat scénář úložiště služby Wins. Tato metoda zajišťuje, že jsou bez uživatele se zobrazí upozornění, na co se děje přepsat žádné změny.

### <a name="detecting-concurrency-conflicts"></a>Zjišťování konfliktů souběžnosti

Můžete vyřešit konflikty zpracování `DbConcurrencyException` výjimky, které vyvolá rozhraní Entity Framework. Chcete-li vědět, kdy má být vyvolána tyto výjimky, musí být schopna zjistit konflikty rozhraní Entity Framework. Proto musíte nakonfigurovat databázi a datový model správně. Některé možnosti aktivace zjišťování konfliktů, patří:

* V tabulce databáze patří sledování sloupec, který slouží k určení, kdy se změnil na řádek. Potom můžete nakonfigurovat rozhraní Entity Framework zahrnovat tento sloupec v Where klauzule SQL aktualizace nebo odstranění příkazy.

     Datový typ sloupce sledování je obvykle `rowversion`. `rowversion` Hodnota je pořadové číslo, které se zvýší pokaždé, když se aktualizuje na řádek. V příkazu Update nebo Delete Where klauzule obsahuje původní hodnota sloupce sledování (původní verze řádku). Pokud se změnila řádek aktualizován jiným uživatelem, hodnota v `rowversion` sloupec je jiná než původní hodnota, takže příkaz Update nebo Delete nelze najít řádek, abyste aktualizovat z důvodu Where klauzule. Když najde Entity Framework, že byly aktualizovány žádné řádky aktualizace nebo odstranění příkazů (to znamená, když počet ovlivněných řádků je nulová), interpretuje, jako konflikt souběžnosti.

* Nakonfigurujte rozhraní Entity Framework zahrnují původní hodnoty každý sloupec v tabulce v Where klauzuli Update a Delete.

     Jako první možnost, pokud se nic v řádku změnila vzhledem k tomu, že byl řádek nejdřív přečíst Where klauzule nevrátí řádek aktualizace, které rozhraní Entity Framework interpretuje jako konflikt souběžnosti. Pro tabulky databáze, které mají mnoho sloupců, tento přístup má za následek velké tam, kde klauzule a může vyžadovat udržovat velkých objemů stavu. Jak již bylo uvedeno dříve, údržbu velkých objemů stavu může ovlivnit výkon aplikace. Proto se obecně nedoporučuje tento přístup, a není to metoda použitá v tomto kurzu.

     Pokud chcete implementovat tento přístup k concurrency, je nutné označit všechny vlastnosti primárního klíče entity, kterou chcete sledovat souběžnosti pro přidáním `ConcurrencyCheck` je atribut. Tato změna umožňuje zahrnout všechny sloupce v klauzuli Where příkazu SQL příkazy Update a Delete rozhraní Entity Framework.

Ve zbývající části tohoto kurzu přidáte `rowversion` sledování vlastnost entity oddělení, vytvořte řadič a zobrazení a otestovat a ověřit, že všechno funguje správně.

## <a name="add-a-tracking-property-to-the-department-entity"></a>Přidat vlastnost sledování do oddělení entity

V *Models/Department.cs*, přidejte sledování vlastnost s názvem RowVersion:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

`Timestamp` Atribut určuje, že v tomto sloupci, budou zahrnuty v Where klauzuli Update a Delete odeslal do databáze. Atribut se nazývá `Timestamp` protože předchozí verze systému SQL Server používá SQL `timestamp` datového typu než SQL `rowversion` jej nahradit. Typ formátu .NET pro `rowversion` je bajtové pole.

Pokud dáváte přednost použijte rozhraní fluent API, můžete použít `IsConcurrencyToken` – metoda (v *Data/SchoolContext.cs*) k určení vlastnosti, sledování, jak je znázorněno v následujícím příkladu:

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

Přidáním vlastnosti jste změnili model databáze, takže je třeba provést další migraci.

Uložte změny a sestavte projekt a potom zadejte následující příkazy v příkazovém okně:

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a>Vytvořit řadič oddělení a zobrazení

Stejně jako dříve pro studenty, kurzy a vyučující vygenerujte řadič oddělení a zobrazení.

![Oddělení vygenerované uživatelské rozhraní](concurrency/_static/add-departments-controller.png)

V *DepartmentsController.cs* souboru, změňte všechny čtyři výskyty "FirstMidName" na "FullName" tak, aby oddělení správce rozevírací seznamy bude obsahovat celý název lektorem a nikoli pouze poslední název.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a>Aktualizace zobrazení Index oddělení

Generování uživatelského rozhraní stroj vytvořil RowVersion sloupec v indexu zobrazení, ale toto pole by se neměly zobrazovat.

Nahraďte kód v *Views/Departments/Index.cshtml* následujícím kódem.

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

To změní záhlaví "Oddělení", odstraní sloupec RowVersion a zobrazuje úplný název místo křestní jméno pro správce.

## <a name="update-the-edit-methods-in-the-departments-controller"></a>Aktualizace metod úpravy v kontroleru oddělení

V obou třídy MetadataExchangeClientMode `Edit` metoda a `Details` metody přidat `AsNoTracking`. V třídy MetadataExchangeClientMode `Edit` metody přidat přes načítání pro správce.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

Nahraďte stávající kód httppost `Edit` metoda následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

Kód začíná při pokusu o čtení oddělení aktualizovat. Pokud `SingleOrDefaultAsync` metoda vrátí hodnotu null, z oddělení byla odstraněna jiným uživatelem. V takovém případě kód používá hodnoty odeslaného formuláře vytvořit entitu oddělení tak, aby stránce Upravit můžete zobrazí znovu, zobrazí se chybová zpráva. Jako alternativu nebude muset znovu vytvořit entitu oddělení, pokud se zobrazí pouze se chybová zpráva bez opakované zobrazování pole oddělení.

Zobrazení ukládá původní `RowVersion` obdrží tuto hodnotu v hodnotě ve skrytém poli a tato metoda `rowVersion` parametr. Před voláním `SaveChanges`, budete muset uvést, původní `RowVersion` hodnoty vlastností v `OriginalValues` kolekce pro entitu.

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

Pak když rozhraní Entity Framework vytvoří příkaz SQL aktualizace, tento příkaz bude obsahovat klauzuli WHERE, která vypadá pro řádek, který má původní `RowVersion` hodnota. Pokud se příkaz UPDATE žádné řádky (žádné řádky mají původní `RowVersion` hodnotu), vyvolá rozhraní Entity Framework `DbUpdateConcurrencyException` výjimka.

Kód v bloku catch pro této výjimky získá ovlivněných oddělení entita, která má aktualizovanými hodnotami z `Entries` vlastnost na objekt výjimky.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

`Entries` Kolekce budou mít pouze jeden `EntityEntry` objektu.  Tento objekt můžete získat nové hodnoty zadané uživatelem a hodnot v aktuální databázi.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

Kód přidá vlastní chybové zprávy pro každý sloupec, který má jinou hodnot v databázi z jaké zadané uživatelem na úpravy stránky (pouze jedno pole je tady zobrazené jako stručný výtah).

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

Nakonec kód nastaví `RowVersion` hodnotu `departmentToUpdate` na novou hodnotu načtena z databáze. Tento nový `RowVersion` hodnota bude uložena ve skrytém poli, když úpravy stránka se zobrazí znovu a další čas uživatel klikne na **Uložit**, pouze souběžného zpracování chyb, které dojít, protože vzniká, redisplay upravit stránky.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

`ModelState.Remove` Příkaz není nutná, protože `ModelState` má starý `RowVersion` hodnotu. V zobrazení `ModelState` hodnota pole má přednost před hodnoty vlastností modelu, pokud obě existuje.

## <a name="update-the-department-edit-view"></a>Aktualizace zobrazení upravit oddělení

V *Views/Departments/Edit.cshtml*, proveďte následující změny:

* Přidání skrytá pole Uložit `RowVersion` hodnotu vlastnosti, hned za skryté pole pro `DepartmentID` vlastnost.

* Přidejte do seznamu rozevíracího seznamu možnost "Vyberte Administrator".

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a>Test souběžnosti konfliktů na stránce Upravit

Spusťte aplikaci a přejděte na stránku oddělení Index. Klikněte pravým tlačítkem myši **upravit** hypertextový odkaz pro angličtinu oddělení a vyberte **otevřít na nové kartě**, klikněte **upravit** hypertextový odkaz pro angličtinu oddělení. Karty dvě prohlížeče zobrazuje teď stejné informace.

Změňte pole na první kartě prohlížeče a klikněte na tlačítko **Uložit**.

![Upravit oddělení stránka 1 po změně](concurrency/_static/edit-after-change-1.png)

Prohlížeč zobrazí stránku Index s změněné hodnoty.

Změňte pole v druhé kartu prohlížeče.

![Upravit oddělení stránka 2 po změně](concurrency/_static/edit-after-change-2.png)

Klikněte na tlačítko **Uložit**. Zobrazí chybovou zprávu:

![Oddělení upravit stránku chybová zpráva](concurrency/_static/edit-error.png)

Klikněte na tlačítko **Uložit** znovu. Hodnota, kterou jste zadali na kartě druhý prohlížeče je uložit. Zobrazí uložené hodnoty, když se zobrazí stránka indexu.

## <a name="update-the-delete-page"></a>Odstranit stránku aktualizace

Rozhraní Entity Framework pro stránku odstranit zjistí souběžnosti konflikty způsobené někdo jinak úpravy oddělení podobným způsobem. Když třídy MetadataExchangeClientMode `Delete` metoda zobrazí potvrzení zobrazení, zobrazení zahrnuje původní `RowVersion` hodnota ve skrytém poli. Hodnota je pak možné HttpPost `Delete` metoda, která je volána, když uživatel potvrdí odstranění. Rozhraní Entity Framework vytvoří příkaz SQL odstranit, obsahuje klauzuli WHERE s původní `RowVersion` hodnota. Pokud výsledky příkazu v nulový počet řádků (tj. řádek byl změněn, jakmile se zobrazí stránka potvrzení odstranění), je vyvolána výjimka souběžnosti a třídy MetadataExchangeClientMode `Delete` metoda je volána k chybě příznak nastaven na hodnotu true, chcete-li znovu zobrazit potvrzovací stránku s chybovou zprávou. Je také možné, že byly ovlivňuje nulový počet řádků, protože řádek byl odstraněn jiným uživatelem, tak v takovém případě se zobrazí žádná chybová zpráva.

### <a name="update-the-delete-methods-in-the-departments-controller"></a>Aktualizace metod odstranit v kontroleru oddělení

V *DepartmentsController.cs*, nahraďte třídy MetadataExchangeClientMode `Delete` metoda následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

Metodu je možné zadat volitelný parametr, který označuje, zda stránky se se zobrazí znovu po chybě souběžnosti. Pokud tento příznak má hodnotu true a oddělení zadaný už existuje, byla odstraněna jiným uživatelem. V takovém případě kód přesměruje na indexovou stránku.  Pokud tento příznak má hodnotu true a oddělení neexistuje, bylo změněno jiným uživatelem. V takovém případě kód odešle chybovou zprávu pomocí zobrazení `ViewData`.  

Nahraďte kód v HttpPost `Delete` – metoda (s názvem `DeleteConfirmed`) s následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

V automaticky generovaný kód, který právě nahrazen tato metoda povoleny pouze ID záznamu:


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

Změnili jste tento parametr k oddělení instanci entity vytvořené vazač modelu. To umožňuje přístup EF hodnota vlastnosti RowVersion kromě klíč záznamu.

```csharp
public async Task<IActionResult> Delete(Department department)
```

Také jste změnili název metody akce z `DeleteConfirmed` k `Delete`. Automaticky generovaný kód používá název `DeleteConfirmed` umožnit metoda HttpPost jedinečný podpis. (Modulu CLR vyžaduje přetížené metody, mít parametry jinou metodu.) Teď, když podpisy jsou jedinečné, můžete přilepit s konvence MVC a použít pro odstranění metody HttpPost a třídy MetadataExchangeClientMode stejný název.

Pokud oddělení byl již odstraněn, `AnyAsync` metoda vrátí hodnotu false a aplikace právě přejde zpět na metodu Index.

Pokud je chyba souběžnosti zachycena, kód znovu zobrazí stránka potvrzení odstranění a poskytuje příznak, které označují, že by měl zobrazit chybovou zprávu souběžnosti.

### <a name="update-the-delete-view"></a>Aktualizace zobrazení odstranění

V *Views/Departments/Delete.cshtml*, nahraďte následující kód, který přidá na pole zpráva Chyba a skrytá pole vlastností DepartmentID a RowVersion automaticky generovaný kód. Změny se zvýrazněnou.

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

Díky následující změny:

* Přidá chybovou zprávu mezi `h2` a `h3` záhlaví.

* Nahradí FullName v FirstMidName **správce** pole.

* Odebere pole RowVersion.

* Přidá skryté pole pro `RowVersion` vlastnost.

Spusťte aplikaci a přejděte na stránku oddělení Index. Klikněte pravým tlačítkem myši **odstranit** hypertextový odkaz pro angličtinu oddělení a vyberte **otevřít na nové kartě**, na první kartě klikněte na **upravit** hypertextový odkaz pro angličtinu oddělení.

V prvním okně, změňte jednu z hodnot a klikněte na tlačítko **Uložit**:

![Stránka Upravit oddělení po změně před odstranění](concurrency/_static/edit-after-change-for-delete.png)

Na kartě druhý klikněte na tlačítko **odstranit**. Zobrazí se zpráva Chyba souběžnosti a hodnoty oddělení aktualizovaly s co je aktuálně v databázi.

![Stránka potvrzení odstranění oddělení s chybou souběžnosti](concurrency/_static/delete-error.png)

Pokud kliknete na tlačítko **odstranit** znovu, budete přesměrováni na indexovou stránku, která ukazuje, že byla odstraněna z oddělení.

## <a name="update-details-and-create-views"></a>Podrobné informace o aktualizaci a vytvořit zobrazení

Můžete volitelně vyčistit automaticky generovaný kód v podrobnostech a vytvořit zobrazení.

Nahraďte kód v *Views/Departments/Details.cshtml* odstranění RowVersion sloupce a zobrazit úplný název tohoto správce.

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

Nahraďte kód v *Views/Departments/Create.cshtml* pro přidání do rozevíracího seznamu vyberte možnost.

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a>Souhrn

Tím dokončíte Úvod pro zpracování konfliktů souběžnosti. Další informace o způsobu zpracování souběžnost v EF jádra najdete v tématu [konfliktů souběžnosti](https://docs.microsoft.com/ef/core/saving/concurrency). Další kurz ukazuje, jak implementovat tabulky za hierarchie dědičnosti pro lektorem a Student entity.

>[!div class="step-by-step"]
[Předchozí](update-related-data.md)
[další](inheritance.md)  
