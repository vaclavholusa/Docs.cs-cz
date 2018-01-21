---
title: "Stránky Razor EF základní - číst související Data - 6, 8"
author: rick-anderson
description: "V tomto kurzu číst a zobrazení souvisejících dat – to znamená, data, která rozhraní Entity Framework se načte do navigační vlastnosti."
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: d0cdb5aaa4b1129c3f2404d069e9781ca16260b7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a>Čtení související data – základní EF s stránky Razor (6 8)

Podle [tní Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

V tomto kurzu související data načíst a zobrazit. Související data jsou data, která načte EF základní do navigační vlastnosti.

Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).

Následující ilustrace znázorňuje dokončené stránky v tomto kurzu:

![Kurzy indexovou stránku](read-related-data/_static/courses-index.png)

![Index stránky vyučující](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Přes, explicitní a opožděného načítání souvisejících dat

Že EF základní můžete načíst související data do navigační vlastnosti entity několika způsoby:

* [Přes načítání](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading). Přes načítání je při dotazu pro jeden typ entity také načtení entit v relaci. Při čtení je entita, související data načtena. To obvykle vede jednoho připojení dotaz, který načte všechna data, která je potřeba. Základní EF vydá pro některé typy přes načítání více dotazů. Vydání více dotazů může být efektivnější než v případě pro některé dotazy v EF6 natolik, že jeden dotaz. Je zadaný přes načítání `Include` a `ThenInclude` metody.

 ![Příklad přes načítání](read-related-data/_static/eager-loading.png)
 
 Přes načítání pošle více dotazů, když je součástí nvavigation kolekce:

 * Jeden dotaz pro hlavní dotaz 
 * Jeden dotaz pro každou kolekci "edge" ve stromové struktuře zatížení.

* Samostatné dotazy s `Load`: v samostatné dotazy může být načtena data a základní EF "oprav" navigační vlastnosti. "opravy nahoru" znamená, že základní EF automaticky naplní navigační vlastnosti. Samostatné dotazy s `Load` se víc podobá explict načítání než přes načítání.

 ![Příklad samostatné dotazy](read-related-data/_static/separate-queries.png)

 Poznámka: Základní EF automaticky opravuje navigační vlastností s jinými entitami, které byly dříve načteny do instance kontextu. I když se data pro navigační vlastnost *není* výslovně zahrnuty, vlastnost pořád naplněný, pokud některé nebo všechny související entity byly dříve načteny.

* [Explicitní načítání](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading). Když je nejdřív přečíst entity, není načíst související data. Kód musí být zapsané do načíst související data, když je to potřeba. Explicitní načítání s samostatné dotazy za následek více dotazů odesílaných do databáze. S explicitní načítání, určuje kód navigační vlastnosti, které mají být načtena. Použití `Load` metoda udělat explicitní načítání. Příklad:

 ![Příklad explicitní načítání](read-related-data/_static/explicit-loading.png)

* [Opožděného načítání](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading). [Základní EF aktuálně nepodporuje opožděného načítání](https://github.com/aspnet/EntityFrameworkCore/issues/3797). Když je nejdřív přečíst entity, není načíst související data. Při prvním přístupu k navigační vlastnost, lze data potřebná pro tuto navigační vlastnost je automaticky načte. K databázi. pokaždé, když navigační vlastnost přistupuje poprvé bude odeslán dotaz.

* `Select` Operátor načte pouze související data, které jsou potřeba.

## <a name="create-a-courses-page-that-displays-department-name"></a>Vytvořte stránku kurzy, která zobrazí název oddělení

Navigační vlastnost, která obsahuje zahrnuje entity kurzu `Department` entity. `Department` Oddělení, které během je přiřazena k obsahuje entity.

Zobrazí se název přiřazené oddělení v seznamu kurzů:

* Získat `Name` vlastnost z `Department` entity.
* `Department` Entity pochází z `Course.Department` navigační vlastnost.

![ourse.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>Vygenerované uživatelské rozhraní během modelu

* Ukončete aplikaci Visual Studio.
* Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).
* Spusťte následující příkaz:

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

Předchozí příkaz scaffold `Course` modelu. Otevřete projekt v sadě Visual Studio.

Sestavte projekt. Sestavení generuje chyby takto:

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Globálně změnit `_context.Course` k `_context.Courses` (tedy "s" přidat do `Course`). 7 výskytů jsou vyhledána a aktualizovat.

Otevřete *Pages/Courses/Index.cshtml.cs* a prozkoumat `OnGetAsync` metoda. Přes načítání pro zadaný modul generování uživatelského rozhraní `Department` navigační vlastnost. `Include` Metoda určuje přes načítání.

Spusťte aplikaci a vyberte **kurzy** odkaz. Zobrazuje sloupec oddělení `DepartmentID`, což není užitečné.

Aktualizace `OnGetAsync` metoda následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Předchozí kód přidá `AsNoTracking`. `AsNoTracking`zvyšuje výkon, protože nejsou sledovat entity vrátila. Entity, které nejsou sledovat, protože se neaktualizují v aktuálním kontextu.

Aktualizace *Views/Courses/Index.cshtml* s následující zvýrazněný kód:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Na automaticky generovaný kód byly provedeny následující změny:

* Změnit záhlaví od indexu na kurzy.
* Přidat **číslo** sloupec, který ukazuje `CourseID` hodnotu vlastnosti. Ve výchozím nastavení nejsou vygeneroval primární klíče, protože jsou obvykle smysl pro koncové uživatele. Ale v takovém případě primární klíč má smysl.
* Změnit **oddělení** sloupec, který se zobrazí název oddělení. Kód zobrazí `Name` vlastnost `Department` entity, který je načten do `Department` navigační vlastnost:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Spusťte aplikaci a vyberte **kurzy** karty zobrazíte seznam s názvy oddělení.

![Kurzy indexovou stránku](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>Načítají se související data pomocí vyberte

`OnGetAsync` Metoda načte související data se `Include` metoda:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`Select` Operátor načte pouze související data, které jsou potřeba. Pro jednotlivé položky jako `Department.Name` používá SQL vnitřního spojení. Pro kolekce používá jiný přístup k databázi, ale tak.`Include` operátor u kolekcí.

Následující kód načte související data se `Select` metoda:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

V tématu [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) a [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) kompletní příklad.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Vytvořit stránku vyučující, která zobrazuje kurzy a registrace

V této části se vytvoří vyučující stránky.

<a name="IP"></a>
![Index stránky vyučující](read-related-data/_static/instructors-index.png)

Tato stránka načte a zobrazí související data následujícími způsoby:

* Zobrazí seznam vyučující souvisejících dat z `OfficeAssignment` entity (Office na předchozím obrázku). `Instructor` a `OfficeAssignment` entity jsou ve vztahu-nula nebo 1. Přes načítání se používá pro `OfficeAssignment` entity. Přes načítání je obvykle efektivnější, když související data se mají zobrazit. V takovém případě se zobrazí přiřazení office pro vyučující.
* Když uživatel vybere lektorem (Harui na předchozím obrázku), související `Course` entity jsou zobrazeny. `Instructor` a `Course` jsou entit v relaci m: n. Načítání pro nápovědy eager `Course` entity a jejich související `Department` entity se používá. Samostatné dotazy v tomto případě může být efektivnější, protože jsou potřeba jenom kurzy pro vybrané lektorem. Tento příklad ukazuje způsob použití přes načítání pro navigační vlastnosti v entity, které jsou v navigační vlastnosti.
* Když uživatel vybere kurzu (chemie na předchozím obrázku), související data z `Enrollments` entity se zobrazí. Na předchozím obrázku se zobrazí název student a úrovni. `Course` a `Enrollment` entity jsou v vztah jeden mnoho.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Vytvoření modelu zobrazení pro Index lektorem zobrazení

Stránka vyučující zobrazuje data ze tří různých tabulek. Model zobrazení je vytvořen, který zahrnuje tři entity představující tři tabulky.

V *SchoolViewModels* složku vytvořit *InstructorIndexData.cs* následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Vygenerované uživatelské rozhraní lektorem modelu

* Ukončete aplikaci Visual Studio.
* Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).
* Spusťte následující příkaz:

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

Předchozí příkaz scaffold `Instructor` modelu. Otevřete projekt v sadě Visual Studio.

Sestavte projekt. Sestavení generuje chyby.

Globálně změnit `_context.Instructor` k `_context.Instructors` (tedy "s" přidat do `Instructor`). 7 výskytů jsou vyhledána a aktualizovat.

Spusťte aplikaci a přejděte na stránku vyučující.

Nahraďte *Pages/Instructors/Index.cshtml.cs* následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

`OnGetAsync` Metoda přijímá data volitelné trasy pro ID vybrané lektorem.

Vyhledejte dotaz na *Pages/Instructors/Index.cshtml* stránky:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

Dotaz má dva zahrnuje:

* `OfficeAssignment`: Zobrazených v [vyučující zobrazení](#IP).
* `CourseAssignments`: Což přináší v rámci přípravy výukové.


### <a name="update-the-instructors-index-page"></a>Aktualizace indexovou stránku vyučující

Aktualizace *Pages/Instructors/Index.cshtml* s následující kód:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Předchozí kód provede tyto změny:

* Aktualizace `page` direktivy z `@page` k `@page "{id:int?}"`. `"{id:int?}"`je šablonu trasy. Šablona trasy změny řetězce dotazu celé číslo v adrese URL data trasy. Například kliknete na **vyberte** odkaz lektorem při page – direktiva vytvoří adresu URL podobnou následující:

    `http://localhost:1234/Instructors?id=2`

    Pokud se jedná o direktivu stránky `@page "{id:int?}"`, je předchozí adresa URL:

    `http://localhost:1234/Instructors/2`

* Název stránky je **vyučující**.
* Přidat **Office** sloupec, který zobrazuje `item.OfficeAssignment.Location` pouze v případě `item.OfficeAssignment` není null. Protože tato relace-nula nebo 1, nemusí být související entity OfficeAssignment.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Přidat **kurzy** sloupec, který zobrazuje kurzy výukové podle jednotlivých lektorem. V tématu [explicitní přechod na řádek s `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) Další informace o tuto syntaxi razor.

* Přidání kódu, která dynamicky přidá `class="success"` k `tr` element vybrané lektorem. Toto nastaví barvu pozadí vybraného řádku použití Bootstrap třídy.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Přidat nový hypertextový odkaz s názvem bez přípony **vyberte**. Tento odkaz odešle vybraný lektorem ID, který má `Index` metoda a nastaví barvu pozadí.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Spusťte aplikaci a vyberte **vyučující** kartě. Na stránce se zobrazuje `Location` (office) z související `OfficeAssignment` entity. Pokud OfficeAssignment' je null, se zobrazí buňky prázdná tabulka.

![Vyučující indexovou stránku nic zvolené](read-related-data/_static/instructors-index-no-selection.png)

Klikněte na **vyberte** odkaz. Změny styl řádku.

### <a name="add-courses-taught-by-selected-instructor"></a>Přidat kurzy výukové podle vybrané lektorem

Aktualizace `OnGetAsync` metoda v *Pages/Instructors/Index.cshtml.cs* následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

Zkontrolujte aktualizované dotazu:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

Přidá předchozího dotazu `Department` entity.

Následující kód provede, když je vybrána lektorem (`id != null`). Vybrané lektorem se načítají ze seznamu vyučující v zobrazení modelu. Model zobrazení `Courses` vlastnosti je načtena s `Course` entity z této lektorem `CourseAssignments` navigační vlastnost.

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

`Where` Metoda vrátí kolekci. V předchozím `Where` metoda jediným `Instructor` se vrátí entity. `Single` Metoda převede kolekci do jednoho `Instructor` entity. `Instructor` Entity poskytuje přístup k `CourseAssignments` vlastnost. `CourseAssignments`poskytuje přístup k související `Course` entity.

![M:M lektorem kurzy](complex-data-model/_static/courseassignment.png)

`Single` Metoda se používá na kolekci, pokud kolekce obsahuje pouze jednu položku. `Single` Metoda vyvolá výjimku, pokud je kolekce prázdná, nebo pokud existuje více než jednu položku. Alternativou je `SingleOrDefault`, která vrací výchozí hodnotu (null v tomto případě) Pokud je kolekce prázdná. Pomocí `SingleOrDefault` na prázdnou kolekci:

* Výsledkem výjimku (z pokusu o vyhledání `Courses` vlastnost na hodnotu Null).
* Zpráva o výjimce by méně jasně ukazovat na příčinu problému.

Následující kód naplní model zobrazení `Enrollments` vlastnost, pokud je vybrána kurzu:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Přidejte následující kód do konce *Pages/Courses/Index.cshtml* Razor stránky:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

Předchozí kód zobrazí seznam kurzů související s lektorem, pokud je vybrána lektorem.

Testování aplikací. Klikněte na **vyberte** na stránce vyučující odkaz.

![Vyučující Index stránky lektorem vybrané](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Zobrazit data pro studenty

V této části je aktualizována aplikace na student data pro vybrané kurzu.

Aktualizace dotazu ve `OnGetAsync` metoda v *Pages/Instructors/Index.cshtml.cs* následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Aktualizace *Pages/Instructors/Index.cshtml*. Na konec souboru přidejte následující kód:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

Předchozí kód zobrazí seznam studenty, kteří jsou zaregistrované v vybraný kurz.

Aktualizujte stránku a vyberte lektorem. Vyberte kurz chcete zobrazit seznam registrovaných studenty a jejich tříd.

![Vyučující Index stránky lektorem a kurzu vybrán](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Pomocí jednoho

`Single` Metoda můžete předat `Where` podmínku namísto volání `Where` metoda samostatně:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

Podle předchozích `Single` přístup poskytuje žádné výhod oproti použití `Where`. Někteří vývojáři raději `Single` přístupu stylu.

## <a name="explicit-loading"></a>explicitní načítání

Určuje aktuálního kódu přes načítání pro `Enrollments` a `Students`:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Předpokládejme, že uživatelé chtějí zřídka najdete v části registrace v kurzu. V takovém případě optimalizace bude pouze načíst data zápisu, pokud se vyžaduje. V této části `OnGetAsync` je aktualizovat, a použít explicitní načítání `Enrollments` a `Students`.

Aktualizace `OnGetAsync` následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Předchozí kód zahodí *ThenInclude* metoda volá pro registraci a student data. Pokud je vybrána kurzu, načte zvýrazněný kód:

* `Enrollment` Entity pro vybraný kurz.
* `Student` Entity pro každou `Enrollment`.

Všimněte si předchozí kód komentáře se `.AsNoTracking()`. Navigační vlastnosti mohou být explicitně načteny pouze u sledovaných entit.

Testování aplikací. Z hlediska uživatelů aplikace se chová stejně jako se jeho předchozí verze.

Další kurz ukazuje, jak aktualizovat data v relaci.

>[!div class="step-by-step"]
>[Předchozí](xref:data/ef-rp/complex-data-model)
>[další](xref:data/ef-rp/update-related-data)
