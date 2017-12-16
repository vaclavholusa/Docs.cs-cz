---
title: "Stránky Razor EF základní - CRUD - 2 8"
author: rick-anderson
description: "Ukazuje, jak vytvořit, číst, aktualizovat, odstraňovat s EF jádra"
keywords: "ASP.NET Core, Entity Framework Core, CRUD, vytvářet, číst, aktualizovat, odstranit"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: 163bc35afed0bf1d9236935d5ce60e6975356594
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a>Vytvářet, číst, aktualizovat a odstraňovat – základní EF s stránky Razor (2 8)

Podle [tní Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

V tomto kurzu vygenerované CRUD (vytvořit, číst, aktualizovat, odstraňovat) kód je zkontrolovat a upravit.

Poznámka: Minimalizovat složitost a zachovat tyto kurzy zaměřené na základní EF, EF základní kód se používá v souborech kódu stránky Razor. Někteří vývojáři použít k vytvoření abstraktní vrstvu mezi uživatelského rozhraní (Razor stránky) a vrstva přístupu k datům služby vrstvy nebo úložiště vzoru v.

V tomto kurzu, vytvořit, upravit, odstranit a stránky Razor podrobnosti v *Student* složky jsou upraveny.

Automaticky generovaný kód používá následující vzor pro Create, Edit a Delete stránky:

* Získání a zobrazit požadovaná data pomocí metody GET protokolu HTTP `OnGetAsync`.
* Uložit změny do data s metodou HTTP POST `OnPostAsync`.

Stránky indexu a podrobnosti o získání a jejich zobrazení požadovaná data pomocí metody GET protokolu HTTP`OnGetAsync`

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a>Nahraďte SingleOrDefaultAsync FirstOrDefaultAsync

Generovaný kód používá [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) načíst požadovaná entita. [FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) je efektivnější v načítání jedna entita:

* Pokud kód je potřeba ověřit, zda není více než jedna entita vrácená z dotazu. 
* `SingleOrDefaultAsync`načte více dat a nepotřebné funguje.
* `SingleOrDefaultAsync`vyvolá výjimku, pokud existuje více než jedna entita, která odpovídá část filtru.
*  `FirstOrDefaultAsync`není výjimku, pokud existuje více než jedna entita, která odpovídá část filtru.

Globálně nahradit `SingleOrDefaultAsync` s `FirstOrDefaultAsync`. `SingleOrDefaultAsync`se používá na 5 místech:

* `OnGetAsync`na stránce Podrobnosti.
* `OnGetAsync`a `OnPostAsync` na stránkách upravit a odstranit.

<a name="FindAsync"></a>
### <a name="findasync"></a>Asynchronně vyhledá

Hodně automaticky generovaný kód [asynchronně vyhledá](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) lze místě `FirstOrDefaultAsync` nebo `SingleOrDefaultAsync`. 

`FindAsync`:

* Vyhledá entitu s primární klíč (Primárníklíč). Pokud entity s primárnímu Klíči je sledován pomocí kontextu, je vrácen bez požadavek do databáze.
* Je jednoduchý a stručné sdělení.
* Je optimalizován pro vyhledání jedné entity.
* Můžete mít výhody výkonu v některých situacích, ale zřídka se do play pro scénáře webového normální.
* Implicitně používá [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) místo [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).
Ale pokud chcete zahrnout ostatní entity, pak najít již není vhodné. To znamená, že budete muset abandon najít a přesunout do dotazu průběhu vaší aplikace.

## <a name="customize-the-details-page"></a>Stránce s podrobnostmi o přizpůsobení

Přejděte do `Pages/Students` stránky. **Upravit**, **podrobnosti**, a **odstranit** generované odkazy [pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) v *stránek/studenty / Index.cshtml* souboru.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

Vyberte podrobnosti odkaz. Adresa URL je ve formátu `http://localhost:5000/Students/Details?id=2`. Student ID je předán pomocí řetězce dotazu (`?id=2`).

Aktualizovat úpravy, podrobnosti a odstranit stránky Razor k použití `"{id:int}"` šablonu trasy. Změňte direktivu stránky pro každou tyto stránek z `@page` k `@page "{id:int}"`.

Požadavek na stránku s šablonou cesty "{id: int}", která nemá **není** zahrnují celé číslo trasy hodnota vrátí protokolu HTTP (nenalezen) chybu 404. Například `http://localhost:5000/Students/Details` vrátí chybu 404. Chcete-li nastavit ID volitelný, připojte `?` pro dané omezení trasy:

 ```cshtml
@page "{id:int?}"
```

Spusťte aplikaci, klikněte na odkaz podrobnosti a ověřte adresu URL je předávání ID jako data trasy (`http://localhost:5000/Students/Details/2`).

Neměnit globálně `@page` k `@page "{id:int}"`, provádění Ano zalomení odkazy na domovský uzel a vytvořte stránky.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Přidání souvisejících dat

Automaticky generovaný kód pro studenty indexovou stránku nezahrnuje `Enrollments` vlastnost. V této části, obsah `Enrollments` kolekce se zobrazí na stránce Podrobnosti.

`OnGetAsync` Metodu *Pages/Students/Details.cshtml.cs* používá `FirstOrDefaultAsync` metoda pro načtení jedné `Student` entity. Přidejte následující zvýrazněný kód:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

`Include` a `ThenInclude` metody způsobit kontext k načtení `Student.Enrollments` navigační vlastnost a v rámci každé registrace `Enrollment.Course` navigační vlastnost. Tyto metody jsou examinied podrobně v tomto kurzu data související s čtení.

`AsNoTracking` Metoda zvyšuje výkon v situacích, když entity vrátil se neaktualizují v aktuálním kontextu. `AsNoTracking`je popsána dále v tomto kurzu.

### <a name="display-related-enrollments-on-the-details-page"></a>Zobrazit související registrace na stránce podrobností

Otevřete *Pages/Students/Details.cshtml*. Přidejte následující zvýrazněný kód zobrazíte seznam registrace:

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

Pokud odsazení kód je nesprávný po se vloží kód, stiskněte CTRL-tisíc-D opravte ho.

Předchozí kód prochází entity v `Enrollments` navigační vlastnost. Pro každou registraci zobrazí název kurzu a úrovni. Název postupu načten z kurzu entita, která je uložena v `Course` navigační vlastnost entity registrace.

Spuštění aplikace, vyberte **studenty** a klikněte **podrobnosti** odkaz pro student. Zobrazí se seznam kurzů a tříd pro vybrané student.

## <a name="update-the-create-page"></a>Vytvořit stránku aktualizace

Aktualizace `OnPostAsync` metoda v *Pages/Students/Create.cshtml.cs* následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Zkontrolujte [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kódu:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

V předchozí kód `TryUpdateModelAsync<Student>` pokusit o aktualizaci `emptyStudent` objektu z hodnot odeslaného formuláře [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) vlastnost v [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0). `TryUpdateModelAsync`pouze aktualizace vlastností uvedených (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

V předchozím příkladu:

* Druhý argument (` "student", // Prefix`) je předponu používá pro vyhledávání hodnoty. Není velká a malá písmena.
* Hodnoty odeslaného formuláře jsou převedeny na typy v `Student` model pomocí [model vazby](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>
### <a name="overposting"></a>Overposting

Pomocí `TryUpdateModel` aktualizovat pole odeslaných hodnot je nejlepším postupem zabezpečení, protože zabraňuje overposting. Předpokládejme například, zahrnuje Student entity `Secret` vlastnost, která tato webová stránka nesmí aktualizovat nebo přidat:

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

I v případě, že aplikace nemá `Secret` pole na vytvoření nebo aktualizaci stránky Razor, se hacker může nastavené `Secret` hodnoty overposting. Hackerům může použijte například nástroj Fiddler, nebo si napsat určitého kódu JavaScript ke zveřejnění `Secret` formuláři hodnotu. Původní kód není omezit pole, která při vytváření Student instance používá vazač modelu.

Ať hacker zadaný pro hodnotu `Secret` pole formuláře je aktualizována v databázi. Následující obrázek ukazuje přidání nástroj Fiddler `Secret` pole (s hodnotou "OverPost") na hodnoty odeslaného formuláře.

![Přidání tajný pole Fiddler](../ef-mvc/crud/_static/fiddler.png)

Hodnota "OverPost" úspěšně přidán do `Secret` vlastnost vloženého řádku. Návrhář aplikace nikdy určená `Secret` vlastnost na stránce vytvořit nastavit.

<a name="vm"></a>
### <a name="view-model"></a>Model zobrazení

Model zobrazení obvykle obsahuje podmnožinu vlastností obsažených v modelu používá aplikace. Aplikační model se často nazývá modelu domény. Model domény obvykle obsahuje všechny vlastnosti požadované odpovídající entita v databázi. Model zobrazení obsahuje pouze vlastnosti, které jsou nutné pro vrstvu uživatelského rozhraní (například stránka vytvořit). Kromě zobrazení modelu některé aplikace použít vazby modelu nebo vstupní modelu k předávání dat mezi třídou kódu stránky Razor a v prohlížeči. Vezměte v úvahu následující `Student` modelu zobrazení:

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

Zobrazit modely poskytnout alternativní způsob zabránit overposting. Model zobrazení obsahuje pouze vlastnosti zobrazení (zobrazení) a aktualizaci.

Následující kód používá `StudentVM` zobrazení modelu vytvářet nové student:

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

[SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) metoda nastaví hodnoty tohoto objektu ve čtení hodnoty z jiné [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) objektu. `SetValues`používá vlastnost s odpovídajícím názvem. Typ modelu zobrazení nemusí být související s typem modelu, pouze musí mít vlastnosti, které odpovídají.

Pomocí `StudentVM` vyžaduje [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) aktualizovat, a použít `StudentVM` místo `Student`.

Na stránkách Razor `PageModel` odvozené třídy je zobrazení modelu. 

## <a name="update-the-edit-page"></a>Upravit stránku aktualizace

Aktualizace souboru kódu stránky upravit:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Změny kódu jsou podobná stránce vytvořit s několika výjimkami:

* `OnPostAsync`má volitelný `id` parametr.
* Aktuální student jsou načtena z databáze, místo vytvoření prázdné student.
* `FirstOrDefaultAsync`byla nahrazena [asynchronně vyhledá](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0). `FindAsync`je vhodné použít při výběru entity z primární klíč. V tématu [asynchronně vyhledá](#FindAsync) Další informace.

### <a name="test-the-edit-and-create-pages"></a>Testování úpravy a vytvářet stránky

Vytvořit a upravit několik student entit.

## <a name="entity-states"></a>Stav entity

Kontext databáze uchovává informace o, jestli jsou synchronizované s jejich odpovídající řádky v databázi entity v paměti. Informace o databázi kontext synchronizace určuje, co se stane, když `SaveChanges` je volána. Například když novou entitu je předána `Add` metoda, která je nastavena do stavu entity `Added`. Když `SaveChanges` nazývá databáze kontextu problémy příkazu INSERT jazyka SQL.

Entita může být v jednom z následujících stavů:

* `Added`: Entita ještě neexistuje v databázi. `SaveChanges` Metoda problémy příkazu INSERT.

* `Unchanged`: Žádné změny měly být uloženy s tuto entitu. Entita nemá tento stav, pokud je pro čtení z databáze.

* `Modified`: Bylo upraveno některé nebo všechny hodnoty vlastností entity. `SaveChanges` Metoda problémy příkazu UPDATE.

* `Deleted`: Entita byla označena pro odstranění. `SaveChanges` Metoda problémy příkazech DELETE.

* `Detached`: Entity není sledována aplikací kontext databáze.

V aplikace na ploše změny stavu se obvykle nastavuje automaticky. Entita je pro čtení, změny a stav entity automaticky změnit tak, aby `Modified`. Volání metody `SaveChanges` vytvoří prohlášení aktualizace SQL, který aktualizuje pouze změněné vlastnosti.

Ve webové aplikaci `DbContext` entity a zobrazí data je zrušen po vykreslení stránky, který čte. Když stránkách `OnPostAsync` metoda je volána, Přišla žádost o nový web s novou instanci třídy `DbContext`. Znovu čtení entita v tomto kontextu nové simuluje plochy zpracování.

## <a name="update-the-delete-page"></a>Odstranit stránku aktualizace

V této části je přidán kód k implementaci vlastních chybových zpráv při volání `SaveChanges` selže. Přidejte řetězec tak, aby obsahovala possile chybové zprávy:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Nahraďte `OnGetAsync` metoda následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Předchozí kód obsahuje volitelný parametr `saveChangesError`. `saveChangesError`Určuje, zda byla volána metoda po selhání student objekt odstranit. Operace odstranění se nemusí podařit kvůli přechodným potížím se sítí. Přechodné chyby síťovým budou s větší pravděpodobností v cloudu. `saveChangesError`je hodnota false, pokud stránka odstranění `OnGetAsync` je volána z uživatelského rozhraní. Když `OnGetAsync` volá `OnPostAsync` (protože operace odstranění se nezdařila), `saveChangesError` parametr hodnotu true.

### <a name="the-delete-pages-onpostasync-method"></a>Metoda odstranění stránky OnPostAsync

Nahraďte `OnPostAsync` následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Předchozí kód načte vybrané entity, pak zavolá `Remove` metodu a nastavit stav entity `Deleted`. Když `SaveChanges` nazývá SQL odstranit se vygeneruje příkaz. Pokud `Remove` selže:

* Je DB výjimka zachycena.
* Odstranit stránky `OnGetAsync` metoda je volána s `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Aktualizace stránky Razor odstranění

Přidejte následující zvýrazněný chybová zpráva na stránce odstranit Razor.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Otestujte odstranit.

## <a name="common-errors"></a>Běžné chyby

Nefungují student domů nebo jiné odkazy:

Ověřte stránky Razor obsahuje správné `@page` – direktiva. Například by měla Student nebo Domovská stránka Razor **není** obsahovat šablonu trasy:

```cshtml
@page "{id:int}"
```

Musí zahrnovat každé stránce Razor `@page` – direktiva.

>[!div class="step-by-step"]
[Předchozí](xref:data/ef-rp/intro)
[další](xref:data/ef-rp/sort-filter-page)
