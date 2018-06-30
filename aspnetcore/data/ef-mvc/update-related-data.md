---
title: Jádro ASP.NET MVC s EF Core - aktualizace související Data – 10 7
author: rick-anderson
description: V tomto kurzu aktualizujete souvisejících dat tím, že aktualizuje polí cizího klíče a navigační vlastnosti.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: ef8cb3916e5d1542e4d36cad694351462b94ed32
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093056"
---
# <a name="aspnet-core-mvc-with-ef-core---update-related-data---7-of-10"></a>Jádro ASP.NET MVC s EF Core - aktualizace související Data – 10 7

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).

V tomto kurzu předchozí zobrazených souvisejících dat; v tomto kurzu aktualizujete souvisejících dat tím, že aktualizuje polí cizího klíče a navigační vlastnosti.

Následující ilustrace znázorňuje některé stránek, které budete pracovat.

![Stránka upravit kurzu](update-related-data/_static/course-edit.png)

![Stránka upravit lektorem](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Přizpůsobení vytvořit a upravit stránky pro kurzy

Při vytvoření nové entity kurzu, musí mít relaci s existující oddělení. K provedení této automaticky generovaný kód obsahuje metody kontroleru a vytvořit a upravit zobrazení, které zahrnují rozevíracího seznamu pro výběr z oddělení. Nastaví rozevíracího seznamu `Course.DepartmentID` vlastností cizího klíče, a to je všechno rozhraní Entity Framework potřebuje, aby zatížení `Department` navigační vlastnost s odpovídající entity oddělení. Budete používat automaticky generovaný kód, ale to změnit mírně na přidání zpracování chyb a řazení rozevíracím seznamu.

V *CoursesController.cs*, odstraňte čtyři metody vytvoření a úpravy a nahraďte následujícím kódem:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

Po `Edit` metoda HttpPost vytvoření nové metody, která načte informace o oddělení pro rozevíracího seznamu.

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

`PopulateDepartmentsDropDownList` Metoda získá seznam všech oddělení seřazené podle názvu, vytvoří `SelectList` kolekci rozevíracího seznamu a předává kolekce pro zobrazení v `ViewBag`. Metodu je možné zadat nepovinný `selectedDepartment` parametr, který umožňuje volací kód, který zadejte položku, která bude vybrána při vykreslení rozevíracího seznamu. Zobrazení předá název "DepartmentID" `<select>` značky pomocné rutiny a pomocné rutiny pak zná k prohledání `ViewBag` objekt pro `SelectList` s názvem "DepartmentID".

Třídy MetadataExchangeClientMode `Create` volání metod `PopulateDepartmentsDropDownList` metoda bez nastavení vybrané položky, protože na nový kurz není oddělení ještě vytvořit:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

Třídy MetadataExchangeClientMode `Edit` metoda nastaví vybrané položky podle ID oddělení, které je již přiřazen ke kurzu upravovaný:

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

Metody HttpPost pro obě `Create` a `Edit` také obsahovat kód, který nastaví vybrané položky, když se znovu zobrazit stránku po chybě. Tím se zajistí, že když stránky se zobrazí znovu, chcete-li zobrazit chybová zpráva, ať oddělení nebyla vybrána zůstává vybrané.

### <a name="add-asnotracking-to-details-and-delete-methods"></a>Přidáte. Podrobnosti o AsNoTracking a odstraňte metody

Za účelem optimalizace výkonu podrobnosti o kurzu a odstranění stránky, přidejte `AsNoTracking` zavolá `Details` a třídy MetadataExchangeClientMode `Delete` metody.

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>Upravit zobrazení průběhu

V *Views/Courses/Create.cshtml*, přidejte možnost "Vyberte oddělení" **oddělení** rozevíracího seznamu změňte popisek z **DepartmentID** k  **Oddělení**a přidejte ověřovací zprávu.

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

V *Views/Courses/Edit.cshtml*, proveďte požadovanou změnu stejné pro pole oddělení, stejně jako ve *Create.cshtml*.

Také v *Views/Courses/Edit.cshtml*, přidání pole číslo kurzu před **název** pole. Protože číslo kurzu je primární klíč, se zobrazí, ale nelze ho změnit.

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

Již existuje skrytá pole (`<input type="hidden">`) pro číslo kurzu v okně Upravit. Přidání `<label>` pomocná značka nemá eliminují nutnost použití skryté pole, protože nezpůsobuje kurzu číslo, které má být součástí odeslaných dat, když uživatel klikne **Uložit** na **upravit** stránky.

V *Views/Courses/Delete.cshtml*, přidání kurzu číslo pole v horní části a změňte ID oddělení název oddělení.

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

V *Views/Courses/Details.cshtml*, proveďte požadovanou změnu stejné, který jste právě použili pro *Delete.cshtml*.

### <a name="test-the-course-pages"></a>Testování postupu stránek

Spuštění aplikace, vyberte **kurzy** , klikněte na **vytvořit nový**a zadejte data pro nový kurz:

![Stránka pro vytvoření kurzu](update-related-data/_static/course-create.png)

Klikněte na tlačítko **vytvořit**. S novou během přidán do seznamu se zobrazí stránka kurzy Index. Název oddělení v seznamu Index stránky pochází z navigační vlastnost, zobrazující, že byla správně navázat relaci.

Klikněte na tlačítko **upravit** v postupu v kurzech indexovou stránku.

![Stránka upravit kurzu](update-related-data/_static/course-edit.png)

Data na stránce změny a klikněte na tlačítko **Uložit**. Kurzy indexovou stránku se zobrazí s daty aktualizované kurzu.

## <a name="add-an-edit-page-for-instructors"></a>Přidat stránku upravit pro vyučující

Při úpravách záznamu lektorem chcete mít možnost aktualizovat lektorem office přiřazení. Entitu lektorem má vztah-nula nebo 1 s OfficeAssignment entity, což znamená, že má váš kód pro zpracování těchto situacích:

* Pokud uživatel zruší zaškrtnutí přiřazení office a původně hodnotu, odstraňte OfficeAssignment entity.

* Pokud uživatel zadá hodnotu přiřazení office a původně byla prázdná, vytvořte nové entity OfficeAssignment.

* Pokud uživatel změní hodnotu přiřazení office, změňte hodnotu ve stávající entitu OfficeAssignment.

### <a name="update-the-instructors-controller"></a>Aktualizaci řadiče vyučující

V *InstructorsController.cs*, změňte kód v třídy MetadataExchangeClientMode `Edit` metody, které se načte entitu lektorem `OfficeAssignment` navigační vlastnost a volání `AsNoTracking`:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

Nahraďte HttpPost `Edit` metoda následující kód pro zpracování přiřazení aktualizací office:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

Kód provede následující akce:

-  Změní název metody k `EditPost` protože podpis je nyní stejné jako třídy MetadataExchangeClientMode `Edit` – metoda ( `ActionName` atribut určuje, že `/Edit/` adresa URL je stále používán).

-  Získá aktuální entitu lektorem z databáze pomocí přes načítání `OfficeAssignment` navigační vlastnost. Je to stejné jako v třídy MetadataExchangeClientMode `Edit` metoda.

-  Aktualizuje načtenou entitu lektorem hodnotami z vazače modelu. `TryUpdateModel` Přetížení umožňuje povolených vlastnosti, které chcete zahrnout. To brání přečerpání účtování, jak je popsáno v [druhý kurzu](crud.md).

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```

-   Pokud umístění kanceláře je prázdné, nastaví vlastnost Instructor.OfficeAssignment na hodnotu null, takže související řádek v tabulce OfficeAssignment se odstraní.

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- Uloží změny do databáze.

### <a name="update-the-instructor-edit-view"></a>Aktualizace lektorem upravit zobrazení

V *Views/Instructors/Edit.cshtml*, přidat nové pole pro úpravy pobočce, na konci před **Uložit** tlačítko:

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

Spuštění aplikace, vyberte **vyučující** a pak klikněte **upravit** na lektorem. Změna **umístění kanceláře** a klikněte na tlačítko **Uložit**.

![Stránka upravit lektorem](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Přidejte na stránku lektorem upravit přiřazení kurzu

Vyučující může naučit libovolný počet kurzy. Nyní budete vylepšit stránce lektorem upravit přidáním možnost změnit přiřazení kurzu pomocí skupiny zaškrtávacích políček, jak je znázorněno na následujícím snímku obrazovky:

![Stránka upravit lektorem s kurzy](update-related-data/_static/instructor-edit-courses.png)

Vztah mezi kurzu a lektorem entity je m: n. Pokud chcete přidat a odebrat relace, abyste přidávat a odebírat entit do a ze sady entit CourseAssignments spojení.

Uživatelské rozhraní, které umožňuje změnit které kurzy lektorem je přiřazen je skupina zaškrtávacích políček. Zaškrtávací políčko pro každý kurz v databázi se zobrazí, a jsou ty, které lektorem je aktuálně přiřazen k vybrané. Uživatel může vyberte nebo zrušte zaškrtnutí políček, chcete-li změnit přiřazení kurzu. Pokud byly mnohem větší počet kurzy, by pravděpodobně chtít použít jinou metodu prezentace dat v zobrazení, ale stejnou metodu manipulace s spojení subjekt byste použili k vytvoření nebo odstranění relace.

### <a name="update-the-instructors-controller"></a>Aktualizaci řadiče vyučující

Pokud chcete data k zobrazení seznamu zaškrtávacích políček, použijete třídu modelu zobrazení.

Vytvoření *AssignedCourseData.cs* v *SchoolViewModels* složky a nahraďte existující kód následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

V *InstructorsController.cs*, nahraďte třídy MetadataExchangeClientMode `Edit` metoda následujícím kódem. Změny se zvýrazněnou.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

Kód přidá přes načítání pro `Courses` navigační vlastnost a volá novou `PopulateAssignedCourseData` metoda podat informace o použití pole zaškrtávací políčko `AssignedCourseData` zobrazit třídu modelu.

Kód `PopulateAssignedCourseData` metoda čte prostřednictvím všechny entity kurzu Chcete-li načíst seznam kurzů pomocí třídy modelu zobrazení. Pro každý kurz kód ověří, zda během existuje v lektorem `Courses` navigační vlastnost. Chcete-li vytvořit efektivní vyhledávání při kontrole, zda je přiřazen kurzu lektorem, jsou vložena kurzy přiřazené lektorem `HashSet` kolekce. `Assigned` Je nastavena na hodnotu true pro kurzy lektorem je přiřazena k. Zobrazení bude pomocí této vlastnosti k určení, které Kontrola polí musí být zobrazí jako vybraný. Nakonec je předán zobrazení v seznamu `ViewData`.

Dál přidejte kód, který je spuštěn, když uživatel klikne na **Uložit**. Nahraďte `EditPost` metoda s následující kód a přidat nové metody, která aktualizuje `Courses` navigační vlastnost lektorem entity.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

Podpis metody se liší od třídy MetadataExchangeClientMode teď `Edit` proto název metody změní z `EditPost` zpět na `Edit`.

Vzhledem k tomu, že zobrazení nemá kolekci entit kurzu, automaticky se nedá aktualizovat vazač modelu `CourseAssignments` navigační vlastnost. Místo použití vazače modelu k aktualizaci `CourseAssignments` navigační vlastnost, můžete udělat v novém `UpdateInstructorCourses` metoda. Proto musíte vyloučit `CourseAssignments` vlastnost z vazby modelu. To nevyžaduje všechny změny kód, který volá `TryUpdateModel` vzhledem k tomu, že používáte přetížení povolených a `CourseAssignments` není v seznamu zahrnout.

Pokud žádná kontrola byly zaškrtnutá políčka, kód v `UpdateInstructorCourses` inicializuje `CourseAssignments` navigační vlastnost s prázdnou kolekci a vrátí:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

Kód pak projde všechny kurzy v databázi a zkontroluje každý kurzu proti těm, které jsou přiřazeny k lektorem a ty, které byly vybrány v zobrazení. Pro usnadnění efektivní hledání, jsou uloženy pozdější dvě kolekce v `HashSet` objekty.

Pokud je zaškrtnuté políčko kurzu ale během není v `Instructor.CourseAssignments` navigační vlastnost během je přidat do kolekce ve vlastnosti navigace.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

Pokud není zaškrtnuté políčko kurzu, ale probíhá během `Instructor.CourseAssignments` navigační vlastnost, během je odebrána z navigační vlastnost.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>Aktualizujte zobrazení lektorem

V *Views/Instructors/Edit.cshtml*, přidat **kurzy** pole s pole zaškrtávacích políček přidáním následující kód bezprostředně po `div` prvky pro **Office**  pole a před `div` element pro **Uložit** tlačítko.

<a id="notepad"></a>
> [!NOTE]
> Když vložíte kód v sadě Visual Studio, změní se tak, aby dělí kód konce řádků.  Stisknutím kombinace kláves Ctrl + Z jednou vrátit zpět, automatického formátování.  To tak, aby zobrazují se jako to, co vidíte zde opraví konce řádků. Odsazení nemusí být úplně bez chyby, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, a `@:</tr>` řádky musí být na jeden řádek znázorněné nebo získáte Chyba za běhu. Blok vybrané nový kód a stiskněte klávesu Tab, třikrát na řádek kód nového existující kód. Stav tohoto problému můžete zkontrolovat [zde](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

Tento kód vytvoří tabulky jazyka HTML, který má tři sloupce. V každém sloupci je zaškrtávací políčko, za nímž následuje popisek, který se skládá z kurzu číslo a název. Všechna zaškrtávací políčka mají stejný název ("selectedCourses"), která informuje o vazač modelu jsou považovány za skupinu. Hodnota atributu každé zaškrtávací políčko je nastavena na hodnotu `CourseID`. Když je stránka vrácena, vazač modelu předá pole na řadič, který se skládá z `CourseID` hodnoty pro pouze zaškrtávací políčka, které jsou vybrány.

Když zaškrtnutí políček jsou původně vykresleno, ty, které jsou pro kurzy přiřazené lektorem jste zkontrolovali atributy, které vybere je (zobrazí se, které je zaškrtnutí).

Spuštění aplikace, vyberte **vyučující** a klikněte na **upravit** na lektorem zobrazíte **upravit** stránky.

![Stránka upravit lektorem s kurzy](update-related-data/_static/instructor-edit-courses.png)

Změna některých během přiřazení a klikněte na Uložit. Provedené změny se projeví na indexovou stránku.

> [!NOTE]
> Přístup zde použitý k jejich úpravě lektorem kurzu funguje dobře, pokud je omezený počet kurzy. Pro kolekce, které jsou mnohem větší by bylo zapotřebí různých uživatelského rozhraní a jinou metodu aktualizace.

## <a name="update-the-delete-page"></a>Odstranit stránku aktualizace

V *InstructorsController.cs*, odstraňte `DeleteConfirmed` metoda a vložte následující kód na příslušné místo.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

Tento kód provede tyto změny:

* Načítání eager nemá `CourseAssignments` navigační vlastnost.  Je nutné zahrnout to nebo EF nebude vědět o souvisejících `CourseAssignment` entity a nedojde k jejich odstranění.  Aby se zabránilo museli přečtěte si je zde lze nakonfigurovat kaskádové odstranění v databázi.

* Pokud lektorem k odstranění je přiřazen jako správce všech oddělení, odebere přiřazení lektorem z těchto oddělení.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Přidejte na stránku vytvořit umístění kanceláře a kurzy

V *InstructorsController.cs*, odstraňování třídy MetadataExchangeClientMode a HttpPost `Create` metody a poté přidejte následující kód v místě jejich:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

Tento kód je podobný postupu pro `Edit` metody s výjimkou které žádné kurzy jsou vybrány. Třídy MetadataExchangeClientMode `Create` volání metod `PopulateAssignedCourseData` metoda není, protože může být kurzy, ale ve vybrané pořadí zajistit pro prázdnou kolekci `foreach` smyčky v zobrazení (jinak kód zobrazení by throw výjimka odkazu s hodnotou null).

HttpPost `Create` metoda přidá každý vybraný kurz k `CourseAssignments` navigační vlastnost před zkontroluje chyby ověření a přidá nové lektorem do databáze. Kurzy jsou přidány, i když nejsou chyby modelu, takže pokud nejsou chyby modelu (například uživatele s klíči na neplatné datum) a stránce se zobrazí znovu, zobrazí se chybová zpráva, jsou automaticky obnovit kterýkoli výběr kurzu, které byly provedeny.

Všimněte si, že aby bylo možné přidat kurzy, které `CourseAssignments` navigační vlastnost, je nutné inicializovat vlastnost jako prázdnou kolekci:

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

Jako alternativu k to v kódu kontroleru lze nastavit v modelu lektorem změnou metoda getter vlastnosti pro automatické vytvoření kolekce Pokud neexistuje, jak je znázorněno v následujícím příkladu:

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

Pokud změníte `CourseAssignments` vlastnost tímto způsobem můžete odstranit kód inicializace explicitní vlastnost v kontroleru.

V *Views/Instructor/Create.cshtml*, přidejte office umístění textového pole a zaškrtněte políčka pro kurzy před tlačítko pro odeslání. Jako v případě stránce Upravit [opravte formátování Pokud Visual Studio přeformátuje kód při vkládání](#notepad).

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

Test spuštění aplikace a vytvoření lektorem.

## <a name="handling-transactions"></a>Zpracování transakcí

Jak je popsáno v [CRUD kurzu](crud.md), rozhraní Entity Framework implicitně implementuje transakce. Scénáře, kde můžete potřebovat více řídit – například pokud chcete takové operace, provést v transakci – mimo Entity Framework najdete v tématu [transakce](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="summary"></a>Souhrn

Teď jste dokončili úvod k práci s související data. V dalším kurzu se zobrazí způsobu řešení konfliktů souběžnosti.

::: moniker-end

> [!div class="step-by-step"]
> [Předchozí](read-related-data.md)
> [další](concurrency.md)
