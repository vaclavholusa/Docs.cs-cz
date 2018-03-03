---
title: "Jádro ASP.NET MVC s EF Core - CRUD - 2 10"
author: tdykstra
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/crud
ms.openlocfilehash: a586fdde07ecf349d7523d43a623501af62257a2
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a>Vytvářet, číst, aktualizovat a odstraňovat – základní EF s kurz k ASP.NET MVC jádra (2 10)

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).

V předchozích kurzu jste vytvořili aplikaci MVC, která uchovává a zobrazí data pomocí rozhraní Entity Framework a SQL Server LocalDB. V tomto kurzu budete zkontrolujte a přizpůsobit CRUD (vytvořit, číst, aktualizovat, odstraňovat) kód, který generování uživatelského rozhraní MVC pro vás automaticky vytvoří v kontrolery a zobrazení.

> [!NOTE] 
> Je běžnou praxí implementovat vzor úložiště před vytvořením vrstvu abstrakce mezi řadiči a vrstva přístupu k datům. Tyto kurzy zachovat jednoduché a zaměřují se na výuky použití rozhraní Entity Framework, samotné, nepoužívejte úložiště. Informace o úložiště s EF najdete v tématu [poslední kurzu této série](advanced.md).

V tomto kurzu budete pracovat s následujících webových stránek:

![Stránka Podrobnosti studenty](crud/_static/student-details.png)

![Stránka pro vytvoření studenty](crud/_static/student-create.png)

![Stránka upravit studenty](crud/_static/student-edit.png)

![Odstranit stránky studenty](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a>Stránce s podrobnostmi o přizpůsobení

Automaticky generovaný kód pro studenty indexovou stránku vynecháno `Enrollments` vlastnost, protože tato vlastnost obsahuje kolekci. V **podrobnosti** stránky, budete zobrazení obsahu kolekce do tabulky HTML.

V *Controllers/StudentsController.cs*, metoda akce podrobnosti o zobrazení používá `SingleOrDefaultAsync` metoda pro načtení jedné `Student` entity. Přidejte kód, který volá `Include`. `ThenInclude`, a `AsNoTracking` metod, jak je znázorněno v následující zvýrazněný kód.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

`Include` a `ThenInclude` metody způsobit kontext k načtení `Student.Enrollments` navigační vlastnost a v rámci každé registrace `Enrollment.Course` navigační vlastnost.  Dozvíte více o těchto metodách v [čtení souvisejících dat](read-related-data.md) kurzu.

`AsNoTracking` Metoda zlepšuje výkon v situacích, kde entity vrátil nebude aktualizován v aktuálním kontextu životnosti. Získáte další informace o `AsNoTracking` na konci tohoto kurzu.

### <a name="route-data"></a>Data trasy

Hodnota klíče, který je předán `Details` metoda pochází z *směrování dat*. Data trasy, která jsou data, která vazač modelu nalezen v segment adresy URL. Například výchozí trasa určuje kontroler, akci a id segmenty:

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

V následující adresu URL výchozí trasu mapuje lektorem jako řadič, Index jako akce a 1, jako je id; Toto jsou hodnot dat trasy.

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

Poslední část adresy URL ("? courseID = 2021") je hodnotu řetězce dotazu. Hodnota ID, která má také předá vazač modelu `Details` metoda `id` parametru předat jako hodnotu řetězce dotazu:

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

Na stránce Index adresy URL hyperlink vytváří značky příkazy pomocných objektů v zobrazení syntaxe Razor. V následujícím kódu Razor `id` parametr odpovídá výchozí trasu, takže `id` se přidá do data trasy.

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

Tím se vygeneruje následující HTML při `item.ID` 6:

```html
<a href="/Students/Edit/6">Edit</a>
```

V následujícím kódu Razor `studentID` neodpovídá parametr ve výchozí trasu, přidá se jako řetězec dotazu.

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

Tím se vygeneruje následující HTML při `item.ID` 6:

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

Další informace o značky pomocné rutiny najdete v tématu [značky pomocné rutiny v ASP.NET Core](xref:mvc/views/tag-helpers/intro).

### <a name="add-enrollments-to-the-details-view"></a>Přidat registrace k zobrazení podrobností

Otevřete *Views/Students/Details.cshtml*. Každé pole se zobrazí pomocí `DisplayNameFor` a `DisplayFor` pomocné rutiny, jak je znázorněno v následujícím příkladu:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

Po poslední pole a bezprostředně před uzavírací `</dl>` značky, přidejte následující kód k zobrazení seznamu registrace:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

Pokud kód odsazení nesprávný po vložte kód, stiskněte CTRL-tisíc-D opravte ho.

Tento kód projde entity v `Enrollments` navigační vlastnost. Pro každou registraci zobrazí název kurzu a úrovni. Název postupu načten z kurzu entita, která je uložena v `Course` navigační vlastnost entity registrace.

Spuštění aplikace, vyberte **studenty** a klikněte **podrobnosti** odkaz pro student. Zobrazí seznam kurzů a tříd pro vybrané student:

![Stránka Podrobnosti studenty](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>Vytvořit stránku aktualizace

V *StudentsController.cs*, upravte HttpPost `Create` metoda pomocí bloku try-catch – přidávání a odebírání ID z `Bind` atribut.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

Tento kód přidá Student entity vytvořené vazač modelu ASP.NET MVC v entitě studenty nastavení a pak uloží změny do databáze. (Vazač modelu odkazuje funkce ASP.NET MVC, která usnadňuje práci s data odeslaná formuláře; vazač modelu převádí hodnoty odeslaného formuláře pro typy CLR a předává je na metodu akce v parametrech. V tomto případě vazač modelu vytvoří instanci Student entity můžete pomocí hodnoty vlastností z kolekce formuláře.)

Můžete odebrat `ID` z `Bind` atributů, protože ID má hodnotu primárního klíče, který systému SQL Server bude nastavení automaticky při vložit řádek. Vstup od uživatele nemá nastavit hodnotu ID.

Jiné než `Bind` atribut try-catch – blok je pouze změny, které jste udělali automaticky generovaný kód. Pokud se výjimka, která je odvozena z `DbUpdateException` je zachycena, zatímco se ukládají změny, se zobrazí obecnou chybovou zprávu. `DbUpdateException` výjimky jsou někdy způsobeny něco externí do aplikací, nikoli chybě programování, takže uživatel se doporučuje a zkuste to znovu. I když není implementována v této ukázce, produkční kvality aplikace by zaprotokolování výjimky. Další informace najdete v tématu **protokolu přehledy** kapitoly [monitorování a Telemetrie (vytváření reálných cloudových aplikací s Azure)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).

`ValidateAntiForgeryToken` Atribut pomáhá zabránit útokům (proti útokům CSRF) padělání požadavku posílaného mezi weby. Token je automaticky vloženy do zobrazení pomocí [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) a je součástí formulář je odeslán uživatelem. Token je ověřen `ValidateAntiForgeryToken` atribut. Další informace o proti útokům CSRF najdete v tématu [požadavek proti padělání](../../security/anti-request-forgery.md).

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a>Poznámka k zabezpečení o overposting

`Bind` Atribut, který obsahuje automaticky generovaný kód na `Create` metoda je jedním ze způsobů pro ochranu proti overposting v vytvářet scénáře. Předpokládejme například, zahrnuje Student entity `Secret` vlastnost, která nechcete, aby tato webová stránka nastavení.

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

I v případě, že nemáte `Secret` pole na webové stránce, se hacker může použijte například nástroj Fiddler, nebo si napsat určitého kódu JavaScript ke zveřejnění `Secret` formuláři hodnotu. Bez `Bind` atribut omezení pole, která při vytváření instance studenty, vazač modelu používá vazač modelu se vyzvedávat, `Secret` tvoří hodnotu a použít ho k vytvoření Student entity instance. Pak ať hacker zadaný pro hodnotu `Secret` pole formuláře by aktualizována v databázi. Následující obrázek ukazuje přidání nástroj Fiddler `Secret` pole (s hodnotou "OverPost") na hodnoty odeslaného formuláře.

![Přidání tajný pole Fiddler](crud/_static/fiddler.png)

Hodnota "OverPost" by poté byl úspěšně přidán do `Secret` vlastnost vloženého řádku, i když nikdy určený, že webová stránka mohli nastavit tuto vlastnost.

Můžete zabránit overposting ve scénářích upravit tak, že nejprve čtení entity z databáze a pak volání `TryUpdateModel`, předejte v seznamu povolených explicitní vlastností. To je metoda použitá v těchto kurzech.

Alternativní způsob zabránit overposting, je mnoho vývojáři preferované je použití Zobrazit modely spíše než tříd entit s vazby modelu. Zahrnout pouze vlastnosti, které chcete aktualizovat v zobrazení modelu. Po dokončení vazač modelu MVC, zkopírujte do instance entity, volitelně pomocí nástroje, jako je AutoMapper zobrazit vlastnosti modelu. Použití `_context.Entry` na instanci entity k nastavení jeho stavu `Unchanged`a poté nastavte `Property("PropertyName").IsModified` na hodnotu true v každé vlastnosti entity, která je součástí modelu zobrazení. Tato metoda funguje v obou upravovat a vytvářet scénáře.

### <a name="test-the-create-page"></a>Testovací stránka pro vytvoření

Kód v *Views/Students/Create.cshtml* používá `label`, `input`, a `span` (pro ověřovacích zpráv) značky pomocníci pro každé pole.

Spuštění aplikace, vyberte **studenty** a klikněte na **vytvořit nový**.

Zadejte názvy a datum. Zkuste zadat neplatné datum, pokud prohlížeč umožňuje udělat. (Některé prohlížeče vynutit použijte Výběr data.) Pak klikněte na tlačítko **vytvořit** zobrazíte chybovou zprávu.

![Chyba ověření datum](crud/_static/date-error.png)

Toto je ověřování na straně serveru, kterou můžete získat ve výchozím nastavení; novější kurzu uvidíte, jak přidat atributy, které bude také generovat kód pro ověřování na straně klienta. Následující zvýrazněný kód ukazuje kontrolu ověření modelu v `Create` metoda.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

Změňte na platnou hodnotu data a klikněte na tlačítko **vytvořit** zobrazíte nové student se zobrazí v **Index** stránky.

## <a name="update-the-edit-page"></a>Upravit stránku aktualizace

V *StudentController.cs*, třídy MetadataExchangeClientMode `Edit` – metoda (, aniž byste `HttpPost` atribut) používá `SingleOrDefaultAsync` metoda pro načtení vybrané Student entity, jako jste viděli v `Details` metoda. Nemusíte tuto metodu změnit.

### <a name="recommended-httppost-edit-code-read-and-update"></a>Doporučená HttpPost upravit kód: čtení a aktualizaci

Metoda HttpPost upravit akce nahraďte následujícím kódem.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

Tyto změny implementovat zabránit overposting osvědčeným postupem zabezpečení. Scaffolder generované `Bind` atribut a přidat entity vytvořené vazač modelu pro sadu s entit `Modified` příznak. Pro mnoho scénářů není doporučeno kódu, protože `Bind` atribut vymaže všechny existující data v polích není uvedený v `Include` parametr.

Nový kód načte existující entity a volání `TryUpdateModel` aktualizovat pole v načtenou entitu [založené na vstup uživatele v datech odeslaného formuláře](xref:mvc/models/model-binding#how-model-binding-works). Rozhraní Entity Framework Automatická změna sledování nastaví `Modified` příznak na pole, které se mění vstup formuláře. Když `SaveChanges` metoda je volána, rozhraní Entity Framework vytvoří příkazy SQL pro aktualizaci řádku databáze. Konflikty souběžnosti jsou ignorovány a pouze sloupců tabulky, které byly aktualizovány uživatelem se aktualizují v databázi. (Novější kurz ukazuje, jak pro zpracování konfliktů souběžnosti.)

Jako osvědčený postup, aby se zabránilo overposting pole, která mají být možné aktualizovat pomocí **upravit** stránky jsou seznam povolených adres v `TryUpdateModel` parametry. (Prázdný řetězec v seznamu polí v seznamu parametrů předcházející je pro předponu pro použití s názvy pole formuláře.) Aktuálně neexistují žádná další pole, které chcete chránit, ale seznam polí, které chcete vazač modelu pro vazbu zajistí, že pokud přidáte pole do datového modelu v budoucnu, že jsou automaticky chráněné až je přidáte sem.

V důsledku těchto změn podpis metoda HttpPost `Edit` metoda je stejná jako třídy MetadataExchangeClientMode `Edit` metoda; proto jste přejmenovali metodu `EditPost`.

### <a name="alternative-httppost-edit-code-create-and-attach"></a>Alternativní HttpPost upravit kód: Vytvořte a připojte

Doporučené upravit kód HttpPost zajistí aktualizovat pouze změněné sloupce a uchovává data ve vlastnosti, které nechcete, aby zahrnuté pro vazbu modelu. Přístup pro čtení první však vyžaduje další databáze pro čtení a může mít za následek složitější kód pro zpracování konfliktů souběžnosti. Alternativou je připojit entity vytvořené vazač modelu pro kontext EF a označte ji jako upravená. (Neaktualizovat projekt s tímto kódem, ho se zobrazují pouze pro ilustraci volitelné přístup.) 

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

Tento postup můžete použít, pokud webová stránka uživatelského rozhraní obsahuje všechna pole v entitě a můžete aktualizovat některý z nich.

Automaticky generovaný kód používá metodu vytvořit a připojit, ale pouze zachytí `DbUpdateConcurrencyException` výjimky a vrátí 404 kódy chyb.  Ukazuje příklad zachytí všechny výjimky aktualizace databáze a zobrazí chybovou zprávu.

### <a name="entity-states"></a>Stav entity

Uchovává informace o kontextu databáze, zda jsou synchronizována s jejich odpovídající řádky v databázi entity v paměti, a tyto informace Určuje, co se stane, když zavoláte `SaveChanges` metoda. Například když předat do nové entity `Add` metoda, která je nastavena do stavu entity `Added`. Potom při volání `SaveChanges` metoda, kontext databáze problémy příkazu INSERT jazyka SQL.

Entita může být v jednom z následujících stavů:

* `Added`. Entita ještě neexistuje v databázi. `SaveChanges` Metoda problémy příkazu INSERT.

* `Unchanged`. Nic je nutné provést k této entitě pomocí `SaveChanges` metoda. Při čtení entity z databáze, entity začíná se tento stav.

* `Modified`. Některé nebo všechny hodnoty vlastností entity byly upraveny. `SaveChanges` Metoda problémy příkazu UPDATE.

* `Deleted`. Entita byla označena pro odstranění. `SaveChanges` Metoda problémy příkazech DELETE.

* `Detached`. Entity není sledována aplikací kontext databáze.

V aplikaci pracovní plochy změny stavu se obvykle nastavuje automaticky. Přečtěte si entitou a proveďte změny na některé jeho vlastnosti hodnoty. To způsobí, že stav entity automaticky změnit tak, aby `Modified`. Potom při volání `SaveChanges`, rozhraní Entity Framework vytvoří která aktualizuje jenom skutečné vlastnosti, které jste změnili prohlášení aktualizace SQL.

Ve webové aplikaci `DbContext` , který původně čte entity a zobrazí jeho data k provádění úprav je zrušen po vykreslení stránky. Když HttpPost `Edit` akce metoda je volána, Přišla žádost o nový web a máte novou instanci třídy `DbContext`. Pokud znovu načíst entita v tomto kontextu nové, můžete simulovat plochy zpracování.

Ale pokud nechcete provést operace čtení nadbytečné, budete muset použít objekt entity vytvořené vazač modelu.  Nejjednodušší způsob, jak to udělat je nastavení stavu entity na změněné, jak se provádí v dříve uvedeném alternativní HttpPost úpravy kódu. Potom při volání `SaveChanges`, rozhraní Entity Framework aktualizuje všechny sloupce řádku databáze, protože kontext nemá žádný způsob, jak zjistit vlastnosti, které jste změnili.

Pokud chcete, aby se zabránilo přístup pro čtení první, ale také chcete aktualizovat SQL příkaz k aktualizaci pouze pole, která uživatel ve skutečnosti změnit, je složitější kód. Budete muset uložit původní hodnoty nějakým způsobem (například pomocí skrytá pole) tak, že jsou k dispozici při HttpPost `Edit` metoda je volána. Potom můžete vytvořit Student entitu pomocí původní hodnoty, volání `Attach` metoda tento původní verzi entity, aktualizujte hodnoty entity na nové hodnoty a pak zavolají `SaveChanges`.

### <a name="test-the-edit-page"></a>Testovací stránka upravit

Spuštění aplikace, vyberte **studenty** a potom klikněte na tlačítko **upravit** hypertextový odkaz.

![Stránka upravit studenty](crud/_static/student-edit.png)

Některé dat a klikněte na tlačítko Změnit **Uložit**. **Index** otevře se stránka a zobrazí změněná data.

## <a name="update-the-delete-page"></a>Odstranit stránku aktualizace

V *StudentController.cs*, kód šablony pro třídy MetadataExchangeClientMode `Delete` metoda používá `SingleOrDefaultAsync` metoda pro načtení vybrané Student entity, jako jste viděli v podrobnosti a úpravy metody. Ale implementovat vlastní chybová zpráva při volání `SaveChanges` selže, přidáte některé funkce pro tuto metodu a jeho odpovídající zobrazení.

Protože jste viděli pro aktualizaci a vytvoření operací, operacemi odstranění vyžadují dvě metody akce. Metoda, která je volána v odpovědi na požadavek GET zobrazí zobrazení, které umožňuje uživateli možnost schválit nebo zrušit operaci odstranění. Pokud uživatel schválí jeho, vytvoří se požadavek POST. Pokud k tomu dojde, HttpPost `Delete` metoda je volána, a pak dané metody ve skutečnosti provádí operaci odstranění.

Blok try-catch – přidáte do HttpPost `Delete` metodu ke zpracování všechny chyby, které mohou nastat při aktualizaci databáze. Pokud dojde k chybě, volá metodu HttpPost Delete metodu Delete třídy MetadataExchangeClientMode, předání parametru, která určuje, že došlo k chybě. Metodu Delete třídy MetadataExchangeClientMode pak znovu zobrazí potvrzovací stránku spolu s chybovou zprávu, umožní uživateli možnost zrušit nebo akci opakujte.

Nahraďte třídy MetadataExchangeClientMode `Delete` metoda akce s následující kód, který spravuje zasílání zpráv o chybách.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

Tento kód je možné zadat volitelný parametr, který označuje, zda byla volána metoda po selhání uložte změny. Tento parametr je false, pokud třídy MetadataExchangeClientMode `Delete` metoda je volána bez předchozí chybě. Když je volána metodou HttpPost `Delete` metoda v odpovědi na chybu aktualizace databáze, tento parametr hodnotu true a chybovou zprávu, je předaná do zobrazení.

### <a name="the-read-first-approach-to-httppost-delete"></a>Přístup pro čtení první k odstranění HttpPost

Nahraďte HttpPost `Delete` metoda akce (s názvem `DeleteConfirmed`) s následujícím kódem, který provede operaci skutečného odstranění a zachytí všechny chyby aktualizace databáze.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

Tento kód načte vybrané entity, pak zavolá `Remove` metodu a nastavit stav entity `Deleted`. Když `SaveChanges` nazývá SQL odstranit se vygeneruje příkaz.

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>Vytvořit a připojit přístup k odstranění HttpPost

Pokud je zvýšení výkonu v aplikaci vysoký počet prioritu, se můžete vyhnout nepotřebných dotazů SQL vytváření instancí Student entitu pomocí pouze primární klíč, hodnotu a pak nastavení stavu entity na `Deleted`. To je všechno, která rozhraní Entity Framework potřebuje, aby odstranit entitu. (Neuvádějte tento kód ve vašem projektu, se zde pouze k objasnění alternativu.)

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

Pokud entita má související data, která má být rovněž odstraněn, ujistěte se, že kaskádové odstranění je nakonfigurovaný v databázi. S tímto přístupem k odstranění entity nemusí EF Uvědomte si, že se entit v relaci k odstranění.

### <a name="update-the-delete-view"></a>Aktualizace zobrazení odstranění

V *Views/Student/Delete.cshtml*, přidejte chybovou zprávu mezi záhlaví h2 a h3 záhlaví, jak je znázorněno v následujícím příkladu:

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

Spuštění aplikace, vyberte **studenty** a klikněte **odstranit** hypertextový odkaz:

![Odstranit potvrzovací stránku](crud/_static/student-delete.png)

Klikněte na tlačítko **odstranit**. Bez odstraněné student se zobrazí stránka indexu. (Uvidíte příklad kód v akci v tomto kurzu souběžnosti pro zpracování chyb.)

## <a name="closing-database-connections"></a>Ukončením připojení databáze

Tím se uvolní prostředky, které obsahuje připojení k databázi, instance kontextu je nutné odstranit co nejdříve po dokončení s ním. Předdefinované ASP.NET Core [vkládání závislostí](../../fundamentals/dependency-injection.md) postará tuto úlohu pro vás.

V *Startup.cs*, zavoláte [metoda rozšíření AddDbContext](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) zřídit `DbContext` třídy v kontejneru ASP.NET DI. Že metoda nastaví životnost služby `Scoped` ve výchozím nastavení. `Scoped` Doba života objektu kontextu se shoduje s webovou žádost životnosti znamená a `Dispose` automaticky volání metody na konci webový požadavek.

## <a name="handling-transactions"></a>Zpracování transakcí

Ve výchozím nastavení implementuje rozhraní Entity Framework implicitně transakce. Ve scénářích, kde více řádků nebo tabulky provést změny a pak zavolají `SaveChanges`, rozhraní Entity Framework automaticky zajišťuje, že všechny změny úspěšné nebo se budou všechny nezdaří. Pokud jsou nejprve provést některé změny a pak se stane chyba, tyto změny jsou automaticky vrácena zpět. Scénáře, kde můžete potřebovat více řídit – například pokud chcete takové operace, provést v transakci – mimo Entity Framework najdete v tématu [transakce](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="no-tracking-queries"></a>Ne sledování dotazy

Když kontext databáze načítá řádky tabulky, vytvoří objekty entity, které představují je ve výchozím nastavení se uchovává informace o jestli entity v paměti jsou synchronizované s co je v databázi. Data v paměti funguje jako mezipaměť a používá se při aktualizaci entity. Toto použití mezipaměti se často nepotřebné ve webové aplikaci protože kontextu instance jsou obvykle krátkodobou (nový jeden se vytvoří a uvolnění pro každý požadavek) a objektem context, který čte entitu je obvykle zveřejněn. před použitím dané entity znovu.

Zakážete sledování objekty entity v paměti voláním `AsNoTracking` metoda. Typické scénáře, ve kterých můžete chtít udělat, patří:

* Během životního cyklu kontextu není nutné aktualizovat všechny entity a nepotřebujete EF k [automaticky načíst navigační vlastnosti s entity načíst samostatné dotazy](read-related-data.md). Často splnění těchto podmínek v metody akce třídy MetadataExchangeClientMode kontroleru.

* Spustíte dotaz, který načte velký objem dat a bude aktualizován pouze malou část vrácená data. To může být efektivnější vypnout sledování pro velké dotaz a spusťte dotaz později na několik entit, které je nutné aktualizovat.

* Chcete připojit entity aktualizujte ji, ale starší načíst stejné entity pro jiný účel. Vzhledem k tomu, že entita je již sledován kontext databáze, nemůžete připojit entity, který chcete změnit. Jedním ze způsobů ke zpracování této situace je volání `AsNoTracking` na dřívější dotazu.

Další informace najdete v tématu [sledování vs. Ne sledování](https://docs.microsoft.com/ef/core/querying/tracking).

## <a name="summary"></a>Souhrn

Nyní máte úplnou sadu stránek, které provádějí jednoduché operace CRUD pro studenty entity. V dalším kurzu budete rozbalte funkce **Index** tak, že přidáte řazení, filtrování a stránkování.

>[!div class="step-by-step"]
[Předchozí](intro.md)
[další](sort-filter-page.md)  
