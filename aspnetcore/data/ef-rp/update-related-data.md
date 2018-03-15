---
title: "Stránky Razor s EF jádra ASP.NET Core - aktualizace související Data - 7, 8"
author: rick-anderson
description: "V tomto kurzu aktualizujete souvisejících dat tím, že aktualizuje polí cizího klíče a navigační vlastnosti."
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/update-related-data
ms.openlocfilehash: fe76405c67297891351aba2495a4d7ce22c6195b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a>Stránky Razor s EF jádra ASP.NET Core - aktualizace související Data - 7, 8

Podle [tní Dykstra](https://github.com/tdykstra), a [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Tento kurz představuje aktualizaci související data. Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).

Následující ilustrace znázorňuje dokončené stránky.

![Upravit stránku kurzu](update-related-data/_static/course-edit.png)
![lektorem upravit stránku](update-related-data/_static/instructor-edit-courses.png)

Zkontrolujte a testovat stránky postupu vytvoření a úpravy. Vytvořte nový kurz. Oddělení je vybrána jako svůj primární klíč (celé číslo), nikoli jeho název. Upravte nový kurz. Po dokončení testování odstraňte nový kurz.

## <a name="create-a-base-class-to-share-common-code"></a>Vytvořte základní třídu sdílet společný kód

Stránky kurzy a vytvořit a kurzy či upravit třeba seznam názvů oddělení. Vytvořit *Pages/Courses/DepartmentNamePageModel.cshtml.cs* základní třída pro vytvoření a úpravy stránky:

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

Předchozí kód vytvoří [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) obsahovat seznam názvů oddělení. Pokud `selectedDepartment` není zadaný, oddělení je vybrána v `SelectList`.

Vytvořit a upravit třídy modelu stránky odvodí z `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Přizpůsobení kurzy stránky

Při vytvoření nové entity kurzu, musí mít relaci s existující oddělení. Při vytváření kurzu přidat oddělení, obsahuje základní třídu pro vytvoření a úpravy rozevíracího seznamu pro výběr z oddělení. Rozevíracím seznamu sad `Course.DepartmentID` vlastnosti cizího klíče (Cizíklíč). Základní EF používá `Course.DepartmentID` Cizíklíč načíst `Department` navigační vlastnost.

![Vytvořit kurz](update-related-data/_static/ddl.png)

Aktualizace modelu vytvořit stránku s následujícím kódem:

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

Předchozí kód:

* Odvozená z `DepartmentNamePageModel`.
* Používá `TryUpdateModelAsync` aby [overposting](xref:data/ef-rp/crud#overposting).
* Nahradí `ViewData["DepartmentID"]` s `DepartmentNameSL` (ze základní třídy).

`ViewData["DepartmentID"]` nahradí se silnými typy `DepartmentNameSL`. Silného typu modely jsou upřednostňovaná přes slabě typované. Další informace najdete v tématu [slabě typované data (ViewData a ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Kurzy vytvořit stránku aktualizace

Aktualizace *Pages/Courses/Create.cshtml* s následující kód:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

Předchozí kód provede tyto změny:

* Změní titulek z **DepartmentID** k **oddělení**.
* Nahradí `"ViewBag.DepartmentID"` s `DepartmentNameSL` (ze základní třídy).
* Přidá možnost "Vyberte oddělení". Tato změna vykreslí "Vyberte oddělení" místo první oddělení.
* Přidá ověřovací zprávu, pokud není vybraná oddělení.

Stránka Razor používá [vyberte pomocná značky](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Testovací stránka pro vytvoření. Na stránce vytvořit zobrazuje název oddělení, nikoli ID oddělení.

### <a name="update-the-courses-edit-page"></a>Aktualizujte kurzy upravit stránku.

Aktualizace modelu stránky upravit následujícím kódem:

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

Změny jsou podobné těm, které jsou provedeny v modelu vytvořit stránku. V předchozí kód `PopulateDepartmentsDropDownList` předává v oddělení ID, které oddělení, zadaný v rozevíracím seznamu vyberte.

Aktualizace *Pages/Courses/Edit.cshtml* s následující kód:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

Předchozí kód provede tyto změny:

* Zobrazí ID kurzu. Obecně se nezobrazí, primární klíč (Primárníklíč) entity. PKs jsou obvykle smysl pro uživatele. V takovém případě primárnímu Klíči je číslo kurzu.
* Změní titulek z **DepartmentID** k **oddělení**.
* Nahradí `"ViewBag.DepartmentID"` s `DepartmentNameSL` (ze základní třídy).
* Přidá možnost "Vyberte oddělení". Tato změna vykreslí "Vyberte oddělení" místo první oddělení.
* Přidá ověřovací zprávu, pokud není vybraná oddělení.

Tato stránka obsahuje skryté pole (`<input type="hidden">`) pro číslo kurzu. Přidání `<label>` značky pomocnou metodu s `asp-for="Course.CourseID"` není eliminují nutnost použití skryté pole. `<input type="hidden">` je vyžadována pro kurzu číslo, které má být součástí odeslaných dat, když uživatel klikne **Uložit**.

Otestujte aktualizovaný kódu. Vytvářet, upravovat a odstraňovat kurzu.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Podrobnosti o přidání AsNoTracking a odstraňovat modely stránky

[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) můžete zvýšit výkon při sledování není povinné. Přidat `AsNoTracking` stránky modelu odstranit a podrobnosti. Následující kód ukazuje v aktualizovaném modelu. odstranění stránky:

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Aktualizace `OnGetAsync` metoda v *Pages/Courses/Details.cshtml.cs* souboru:

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Upravovat stránky odstranit a podrobnosti

Aktualizujte stránku odstranit Razor následující kód:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Proveďte stejné změny na stránku podrobností.

### <a name="test-the-course-pages"></a>Testování postupu stránek

Test vytvořit, upravit, podrobnosti a odstranění.

## <a name="update-the-instructor-pages"></a>Aktualizovat lektorem stránky

V následujících částech aktualizovat lektorem stránky.

### <a name="add-office-location"></a>Přidat umístění kanceláře

Při úpravě záznamu lektorem, můžete aktualizovat lektorem office přiřazení. `Instructor` Entita má vztah jeden pro žádná nebo jedna s `OfficeAssignment` entity. Kód lektorem musí zpracovat:

* Pokud uživatel zruší zaškrtnutí přiřazení office, odstraňte `OfficeAssignment` entity.
* Pokud uživatel zadá přiřazení office a byla prázdná, vytvořte novou `OfficeAssignment` entity.
* Pokud uživatel změní přiřazení office, aktualizovat `OfficeAssignment` entity.

Aktualizace modelu vyučující upravit stránku s následujícím kódem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

Předchozí kód:

- Získá aktuální `Instructor` entity z databáze pomocí přes načítání pro `OfficeAssignment` navigační vlastnost.
- Aktualizace načtený `Instructor` entity hodnotami z vazače modelu. `TryUpdateModel` zabraňuje [overposting](xref:data/ef-rp/crud#overposting).
- Pokud umístění kanceláře je prázdné, nastaví `Instructor.OfficeAssignment` na hodnotu null. Když `Instructor.OfficeAssignment` je null, související řádek v `OfficeAssignment` tabulka odstraněna.

### <a name="update-the-instructor-edit-page"></a>Aktualizovat stránku lektorem úpravy

Aktualizace *Pages/Instructors/Edit.cshtml* umístěním office:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Ověřte, že můžete změnit umístění sady vyučující office.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Přidejte na stránku upravit lektorem kurzu přiřazení

Vyučující může naučit libovolný počet kurzy. V této části přidáte možnost změnit přiřazení kurzu. Následující obrázek ukazuje aktualizované lektorem upravit stránky:

![Stránka upravit lektorem s kurzy](update-related-data/_static/instructor-edit-courses.png)

`Course` a `Instructor` je v relaci m: n. Pokud chcete přidat a odebrat relace, přidávat a odebírat entity z `CourseAssignments` připojení sady entit.

Zaškrtávací políčka povolte změny kurzy, který je přiřazen lektorem. Zaškrtávací políčko se zobrazí na každé kurz v databázi. Kurzy, které je přiřazen lektorem jsou zaškrtnutá políčka. Uživatel může vyberte nebo zrušte zaškrtnutí políček, chcete-li změnit přiřazení kurzu. Kdyby mnohem větší počet kurzů:

* Pravděpodobně použijete jiné uživatelské rozhraní pro zobrazení kurzy.
* Metoda manipulace s spojení entity k vytvoření nebo odstranění relace by nezměnila.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Přidání třídy pro podporu vytvářet a upravovat stránky lektorem

Vytvoření *SchoolViewModels/AssignedCourseData.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

`AssignedCourseData` Třída obsahuje dat a vytvořte zaškrtnutí políček u přiřazené kurzy podle lektorem.

Vytvořte *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* základní třídy:

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` Je základní třídou budete používat pro úpravy a vytváření modelů stránky. `PopulateAssignedCourseData` načte všechny `Course` entity k naplnění `AssignedCourseDataList`. Pro každý kurz nastaví kód `CourseID`, název a zda je lektorem přiřazen ke kurzu. A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) se používá k vytvoření efektivní vyhledávání.

### <a name="instructors-edit-page-model"></a>Model stránky upravit vyučující

Aktualizace modelu lektorem upravit stránku s následujícím kódem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

Předchozí kód zpracovává změny v přiřazení office.

Aktualizace lektorem Razor zobrazení:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Když vložíte kód v sadě Visual Studio, zalomení řádků se mění tak, aby dělí kód. Stisknutím kombinace kláves Ctrl + Z jednou vrátit zpět, automatického formátování. CTRL + Z opravy konce řádků tak, aby zobrazují se jako to, co vidíte zde. Odsazení nemusí být úplně bez chyby, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, a `@:</tr>` řádky musí být na jeden řádek jak je vidět. Blok vybrané nový kód a stiskněte klávesu Tab, třikrát na řádek kód nového existující kód. Hlasovat o nebo zkontrolujte stav této chyby [s tímto odkazem](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

Předchozí kód vytvoří tabulky jazyka HTML, který má tři sloupce. Každý sloupec má zaškrtávací políčko a popisek obsahující kurzu číslo a název. Všechna zaškrtávací políčka mají stejný název ("selectedCourses"). Použití stejného názvu informuje vazač modelu považovat za skupinu. Atribut hodnota z každé zaškrtávací políčko je nastaven na `CourseID`. Když je stránka vrácena, vazač modelu předá pole, které se skládá z `CourseID` hodnoty pro pouze zaškrtávací políčka, které jsou vybrány.

Když zaškrtnutí políček jsou původně vykresleno, kurzy přiřazené lektorem jste zkontrolovali atributy.

Spusťte aplikaci a otestovat aktualizované vyučující upravit stránku. Změňte přiřazení některých kurzu. Změny se projeví na indexovou stránku.

Poznámka: Přístup zde použitý k jejich úpravě lektorem kurzu funguje dobře, pokud je omezený počet kurzy. Pro kolekce, které jsou mnohem větší jiný uživatelského rozhraní a jinou metodu aktualizace by efektivnější funkční a účinnější.

### <a name="update-the-instructors-create-page"></a>Aktualizace stránka pro vytvoření vyučující

Aktualizace modelu lektorem vytvořit stránku s následujícím kódem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

Předchozí kód je podobná *Pages/Instructors/Edit.cshtml.cs* kódu.

Aktualizujte stránku vytvořit Razor lektorem následující kód:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Testovací stránka pro vytvoření lektorem.

## <a name="update-the-delete-page"></a>Odstranit stránku aktualizace

Aktualizace modelu odstranění stránek s následujícím kódem:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

Předchozí kód provede tyto změny:

* Používá přes načítání pro `CourseAssignments` navigační vlastnost. `CourseAssignments` musí být zahrnut nebo nejsou odstraněny při odstranění lektorem. Abyste se vyhnuli nutnosti přečtěte si je, nakonfigurujte kaskádové odstranění v databázi.

* Pokud lektorem k odstranění je přiřazen jako správce všech oddělení, odebere přiřazení lektorem z těchto oddělení.

>[!div class="step-by-step"]
[Předchozí](xref:data/ef-rp/read-related-data)
[další](xref:data/ef-rp/concurrency)
