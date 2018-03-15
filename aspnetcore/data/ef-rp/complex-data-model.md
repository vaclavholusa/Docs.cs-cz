---
title: "Stránky Razor s EF jádra ASP.NET Core - Model dat – 5 8"
author: rick-anderson
description: "V tomto kurzu přidejte další entity a vztahy a přizpůsobit datový model zadáním formátování, ověření a pravidla mapování."
manager: wpickett
ms.author: riande
ms.date: 10/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 12c863c6eb4b4774853a94cf3001870b0d22e936
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a>Stránky Razor s EF jádra ASP.NET Core - Model dat – 5 8

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Předchozí kurzy pracovali s základní datový model, který se skládá z tři entity. V tomto kurzu:

* Se přidají další entity a vztahy.
* Datový model je přizpůsobit tak, že zadáte formátování, ověřování a pravidla mapování databáze.

Třídy entity pro dokončené datový model je vidět na následujícím obrázku:

![Entity diagram](complex-data-model/_static/diagram.png)

Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).

## <a name="customize-the-data-model-with-attributes"></a>Přizpůsobení datového modelu s atributy

V této části je datový model přizpůsobit pomocí atributů.

### <a name="the-datatype-attribute"></a>Datový typ atributu

Na stránkách student aktuálně zobrazí čas, datum registrace. Obvykle polí data zobrazit pouze datum a není čas.

Aktualizace *Models/Student.cs* s následujícími službami zvýrazněná kódu:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

[Datový typ](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) atribut určuje datový typ, který je specifičtější než vnitřní typ databáze. V tomto případě kterou má být zobrazen pouze data, není datum a čas. [Datový typ výčtu](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) poskytuje pro mnoho typů dat, jako je například datum, čas, telefonní číslo, měny, EmailAddress, atd. `DataType` Atributu můžete také povolit aplikaci automaticky získávat specifické pro typ funkce. Příklad:

* `mailto:` Propojení se automaticky vytvoří pro `DataType.EmailAddress`.
* Je k dispozici pro výběr data `DataType.Date` ve většině prohlížečů.

`DataType` Atribut vysílá standardu HTML 5 `data-` (výrazný data dash) atributy, které využívají standardu HTML 5 prohlížeče. `DataType` Atributy neposkytují ověření.

`DataType.Date` neuvádí formát data, které se zobrazí. Ve výchozím nastavení, zobrazí se pole datum podle výchozích formátů podle serveru [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).

`DisplayFormat` Atribut slouží k explicitnímu zadání formát data:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` Nastavení určuje, že formátování, které také bude použito pro úpravy uživatelského rozhraní. Některá pole neměli používat `ApplyFormatInEditMode`. Například by se neměly symbolu měny zobrazovat obecně do textového pole na Upravit.

`DisplayFormat` Atribut lze použít samostatně. Obecně je vhodné používat `DataType` atribut s `DisplayFormat` atribut. `DataType` Atribut přenese tak sémantika data a jak vykreslit ho na obrazovce. `DataType` Atribut poskytuje následující výhody, které nejsou k dispozici v `DisplayFormat`:

* V prohlížeči můžete povolit funkce HTML5. Například zobrazit ovládacího prvku kalendář, symbolu měny vhodné národního prostředí, e-mailu odkazy, vstupní ověřování na straně klienta, atd.
* Ve výchozím prohlížeči vykreslí data ve správném formátu podle národního prostředí.

Další informace najdete v tématu [ \<vstupní > Pomocník značky dokumentace](xref:mvc/views/working-with-forms#the-input-tag-helper).

Spusťte aplikaci. Přejděte na stránku studenty Index. Časy se už nezobrazují. Každé zobrazení, která používá `Student` modelu zobrazuje datum bez čas.

![Studenti, kteří index stránky zobrazující data bez časů](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>Atribut StringLength

S atributy lze zadat pravidla ověření dat a chybové zprávy ověření. [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) atribut určuje minimální a maximální délku znaků, které jsou v datovém poli. `StringLength` Atribut poskytuje také ověření na straně klienta a na straně serveru. Minimální hodnota nemá žádný vliv na schéma databáze.

Aktualizace `Student` modelu s následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

Předchozí kód omezuje názvy k více než 50 znaků. `StringLength` Atribut nemá uživatel zabránit v přechodu do prázdných znaků pro název. [Regulární výraz](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) atribut se používá k aplikování omezení na vstup. Například následující kód vyžaduje první znak, který má být velkými písmeny a zbývající znaků, které mají být abecední:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

Spuštění aplikace:

* Přejděte na stránku studenty.
* Vyberte **vytvořit nový**a zadejte název, který je delší než 50 znaků.
* Vyberte **vytvořit**, zobrazuje chybovou zprávu ověření na straně klienta.

![Studenti, kteří index stránky zobrazující chyby délka řetězce](complex-data-model/_static/string-length-errors.png)

V **Průzkumník objektů systému SQL Server** (SSOX), otevřete návrháře tabulky Student dvojitým kliknutím **Student** tabulky.

![Studenti, kteří tabulku v SSOX před migrací](complex-data-model/_static/ssox-before-migration.png)

Předchozí obrázek znázorňuje schéma pro `Student` tabulky. Název pole mít typ `nvarchar(MAX)` vzhledem k tomu, že migrace nebyl spuštěn v databázi. Když migrace se spustí později v tomto kurzu, názvu pole se stanou `nvarchar(50)`.

### <a name="the-column-attribute"></a>Atribut sloupce

Atributy můžete řídit, jak třídy a vlastnosti jsou mapované na databázi. V této části `Column` atribut se používá k mapování názvu `FirstMidName` vlastnost "FirstName" v databázi.

Při vytváření databáze názvy vlastností na modelu se používají pro názvy sloupců (s výjimkou, kdy `Column` je použit atribut).

`Student` Model používá `FirstMidName` pro první název pole, protože pole může obsahovat také křestní jméno.

Aktualizace *Student.cs* soubor s následující zvýrazněný kód:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

S předchozí změny `Student.FirstMidName` v aplikaci mapuje `FirstName` sloupec `Student` tabulky.

Přidání `Column` základní model, změní se atribut `SchoolContext`. Základní model `SchoolContext` již neodpovídá databázi. Pokud aplikace běží před použitím migrace, vygeneruje se následující výjimky:

```SQL
SqlException: Invalid column name 'FirstName'.
```
Aktualizace databáze:

* Sestavte projekt.
* Otevřete okno příkazového řádku ve složce projektu. Zadejte následující příkazy k vytvoření nové migrace a aktualizaci databáze:

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

`dotnet ef migrations add ColumnFirstName` Příkaz generuje následující upozornění:

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

Upozornění je vygenerovat, protože název pole jsou nyní omezena na 50 znaků. Pokud název v databázi více než 50 znaků, by dojít ke ztrátě 51 poslední znak.

* Testování aplikací.

Tabulka Student otevřete v SSOX:

![Studenti, kteří tabulky v SSOX po migrace](complex-data-model/_static/ssox-after-migration.png)

Před použitím migrace, byl měla název sloupce typu [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Název sloupce jsou nyní `nvarchar(50)`. Název sloupce změněn z `FirstMidName` k `FirstName`.

> [!Note]
> V následující části sestavení aplikace ve některé fázích generuje chyby kompilátoru. Podle pokynů určete, kdy k sestavení aplikace.

## <a name="student-entity-update"></a>Aktualizace entity studenty

![Student entity](complex-data-model/_static/student-entity.png)

Aktualizace *Models/Student.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Požadovaný atribut

`Required` Atribut je povinná pole název vlastnosti. `Required` Atribut není nutný pro použití hodnot Null typy, jako jsou typy hodnot (`DateTime`, `int`, `double`atd.). Typy, které nemůže mít hodnotu null, jsou automaticky považovány za povinná pole.

`Required` Atribut může být nahrazena minimální délku parametru v `StringLength` atribut:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Atribut zobrazení

`Display` Atribut určuje, že titulek pro do textových polí musí být "Jméno", "Příjmení", "Úplný název" a "Datum registrace." Titulky výchozí neobsahoval mezeru dělení slova, například "Příjmení".

### <a name="the-fullname-calculated-property"></a>Vlastnost FullName vypočítat

`FullName` je počítané vlastnosti, která vrátí hodnotu, která se vytvoří zřetězením dva další vlastnosti. `FullName` Nelze nastavit, že obsahuje pouze přistupující objekt get. Ne `FullName` sloupec se vytvoří v databázi.

## <a name="create-the-instructor-entity"></a>Vytvořit entitu lektorem

![Entitu lektorem](complex-data-model/_static/instructor-entity.png)

Vytvoření *Models/Instructor.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Všimněte si, že několik vlastností, které jsou stejné ve `Student` a `Instructor` entity. V tomto kurzu implementace dědičnosti později z této série tento kód je teď vyčleněný eliminovat redundance.

Více atributů může být na jednom řádku. `HireDate` Atributy může zapsat takto:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments a OfficeAssignment navigační vlastnosti

`CourseAssignments` a `OfficeAssignment` vlastnosti jsou navigační vlastnosti.

Lektorem můžete naučit libovolný počet kurzy, takže `CourseAssignments` je definována jako kolekce.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Pokud navigační vlastnost obsahuje více entit:

* Musí být typu seznamu kde položky lze přidat, odstranit a aktualizovat.

Navigační vlastnost typy patří:

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

Pokud `ICollection<T>` není zadaný, vytvoří základní EF `HashSet<T>` kolekce ve výchozím nastavení.

`CourseAssignment` Entity je popsáno v části u relací m: n.

Contoso univerzity obchodní pravidla stavu, že lektorem může mít maximálně jeden office. `OfficeAssignment` Vlastnost obsahuje jeden `OfficeAssignment` entity. `OfficeAssignment` má hodnotu null, pokud není přiřazena žádná office.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Vytvořit entitu OfficeAssignment

![OfficeAssignment entity](complex-data-model/_static/officeassignment-entity.png)

Vytvoření *Models/OfficeAssignment.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Klíč atributu

`[Key]` Atribut se používá k identifikaci vlastnost jako primární klíč (Primárníklíč) Pokud je vlastnost název něco než classnameID nebo ID.

Je--nula nebo 1 vztah mezi `Instructor` a `OfficeAssignment` entity. Přiřazení office existuje pouze ve vztahu k lektorem, které je přiřazen. `OfficeAssignment` Primárníklíč je také jeho cizí klíč (Cizíklíč) `Instructor` entity. Základní EF nelze rozpoznat automaticky `InstructorID` jako Primárníklíč z `OfficeAssignment` protože:

* `InstructorID` není podle ID nebo classnameID zásady vytváření názvů.

Proto `Key` atribut se používá k identifikaci `InstructorID` jako primárnímu Klíči:

```csharp
[Key]
public int InstructorID { get; set; }
```

Ve výchozím nastavení EF základní klíče jsou považovány za jiný databáze generované protože sloupec je sloupec pro identifikační vztah.

### <a name="the-instructor-navigation-property"></a>Vlastnost navigace lektorem

`OfficeAssignment` Navigační vlastnost pro `Instructor` entita může mít hodnotu Null protože:

* Referenční typy (například třídy jsou s možnou hodnotou Null).
* Lektorem nemusí mít přiřazení office.


`OfficeAssignment` Entita má hodnotou Null `Instructor` navigační vlastnost protože:

* `InstructorID` je použití hodnot Null.
* Přiřazení office nemůže existovat bez lektorem.

Když `Instructor` entita má související `OfficeAssignment` entity, každá entita odkazuje na jinou v její navigační vlastnosti.

`[Required]` Atribut může být použit na `Instructor` navigační vlastnost:

```csharp
[Required]
public Instructor Instructor { get; set; }
```

Předchozí kód určuje, že musí být související lektorem. Předchozí kód není nutný, protože `InstructorID` cizí klíč (což je také primárnímu Klíči) je použití hodnot Null.

## <a name="modify-the-course-entity"></a>Upravit kurzu Entity

![Během entity](complex-data-model/_static/course-entity.png)

Aktualizace *Models/Course.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

`Course` Entity má vlastnosti cizího klíče (Cizíklíč) `DepartmentID`. `DepartmentID` odkazuje na související `Department` entity. `Course` Entita, která má `Department` navigační vlastnost.

Základní EF nevyžaduje vlastnosti cizího klíče u datového modelu, když model má navigační vlastnost pro související entity.

FKs EF základní automaticky vytvoří v databázi, bez ohledu na jejich jste potřeby. Vytvoří základní EF [stínové vlastnosti](https://docs.microsoft.com/ef/core/modeling/shadow-properties) pro automaticky vytvořené FKs. S cizího klíče v datovém modelu, můžete provést aktualizace teď jednodušší a efektivnější. Představte si třeba model kde vlastnost cizího klíče `DepartmentID` je *není* zahrnuty. Pokud je během entity načtených upravit:

* `Department` Entity má hodnotu null, pokud není výslovně načtení.
* K aktualizaci entity kurzu `Department` nejprve musí být získána entity.

Když vlastnost cizího klíče `DepartmentID` je zahrnutá v datovém modelu, není nutné načíst `Department` entity před aktualizace.

### <a name="the-databasegenerated-attribute"></a>Atribut DatabaseGenerated

`[DatabaseGenerated(DatabaseGeneratedOption.None)]` Atribut určuje, že primárnímu Klíči je součástí aplikace místo generované databáze.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Ve výchozím nastavení EF základní předpokládá, že hodnoty Primárníklíč generované databáze. DB generované Primárníklíč hodnoty je obecně nejlepší metodou. Pro `Course` určuje entity, uživatel PK. Například během číslo jako řadu 1000 oddělení matematické, 2000 řady pro angličtinu oddělení.

`DatabaseGenerated` Atribut můžete také použít ke generování výchozí hodnoty. Například databáze může automaticky generovat pole pro datum k zaznamenání datum vytvoření nebo aktualizaci řádku. Další informace najdete v tématu [generované vlastnosti](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Cizí klíč a navigační vlastnosti

Vlastnosti cizího klíče (Cizíklíč) a navigačních vlastností v `Course` entity podle následující relace:

Kurz je přiřazena k jedné oddělení, takže není `DepartmentID` cizího klíče a `Department` navigační vlastnost.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Kurz může mít libovolný počet studenty zaregistrované v něm proto `Enrollments` navigační vlastnost je kolekce:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Kurz může výukové ve více vyučující, proto `CourseAssignments` navigační vlastnost je kolekce:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment` je vysvětlen [později](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Vytvořit entitu oddělení

![Oddělení entity](complex-data-model/_static/department-entity.png)

Vytvoření *Models/Department.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Atribut sloupce

Dříve `Column` atribut se použít ke změně mapování název sloupce. V kódu `Department` entity, `Column` atribut se používá ke změně mapování datového typu SQL. `Budget` Sloupec je definovaný typ money systému SQL Server v databázi:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapování sloupce není obvykle nutné. Základní EF obecně zvolí příslušného typu dat systému SQL Server, na základě typu CLR pro vlastnost. Modul CLR `decimal` zadejte map k systému SQL Server `decimal` typu. `Budget` je currency, a datový typ money je vhodnější pro měny.

### <a name="foreign-key-and-navigation-properties"></a>Cizí klíč a navigační vlastnosti

Vlastnosti cizího klíče a navigační odráží následující relace:

* Oddělení může nebo nemusí mít správcem.
* Správce je vždy lektorem. Proto `InstructorID` vlastnost je zahrnuta jako cizího klíče na `Instructor` entity.

Navigační vlastnost jmenuje `Administrator` , ale obsahuje `Instructor` entity:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Otazník (?) v předchozím kódu určuje, jestli že vlastnost umožňuje hodnotu Null.

Oddělení může mít mnoho kurzy, takže není navigační vlastnost kurzy:

```csharp
public ICollection<Course> Courses { get; set; }
```

Poznámka: Pomocí konvence, umožňuje EF základní kaskádové odstranění pro použití hodnot Null FKs a relace m: n. Kaskádové odstranění může způsobit cyklické cascade odstranit pravidla. Cyklické cascade odstranit pravidla způsobí výjimku při přidání migrace.

Například pokud `Department.InstructorID` vlastnost nebyla definována jako s možnou hodnotou NULL:

* Základní EF nakonfiguruje kaskádové odstranění pravidlo můžete odstranit lektorem, když je odstraněn z oddělení.
* Odstraňování lektorem při odstranění tohoto oddělení nejde o záměrné chování.

V případě potřeby obchodní pravidla `InstructorID` vlastnost mít hodnotu Null, použijte následující příkaz fluent API:

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

Předchozí kód zakáže kaskádové odstranění v relaci lektorem oddělení.

## <a name="update-the-enrollment-entity"></a>Aktualizace entity registrace

Záznam zápisu je jeden kurzu provedenou jeden student.

![Registrace entity](complex-data-model/_static/enrollment-entity.png)

Aktualizace *Models/Enrollment.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Cizí klíč a navigační vlastnosti

Vlastnosti cizího klíče a navigačních vlastností odráží následující relace:

Záznam zápisu je pro jeden kurzu, takže není `CourseID` vlastnosti cizího klíče a `Course` navigační vlastnost:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Záznam zápisu je pro jeden student, takže není `StudentID` vlastnosti cizího klíče a `Student` navigační vlastnost:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relace m: N

Je vztah m: n mezi `Student` a `Course` entity. `Enrollment` Entity funguje jako tabulku spojení m: n *s odebranou datovou částí* v databázi. "S odebranou datovou částí" znamená, že `Enrollment` tabulka obsahuje další údaje kromě FKs pro spojené tabulky (v tomto případě primárnímu Klíči a `Grade`).

Následující obrázek znázorňuje, jak tyto relace vypadat v diagramu entity. (Tento diagram byl vygenerován pomocí EF výkonné nástroje pro EF 6.x. Vytváření diagramu není součástí tohoto kurzu.)

![Během student mnoho k mnoha relace](complex-data-model/_static/student-course.png)

Každý řádek vztahů je 1 na jeden element end a znak hvězdičky (*) v dalších, která určuje vztah jeden mnoho.

Pokud `Enrollment` tabulky nezahrnuli úrovni informace, jenom třeba, aby obsahovat dvě FKs (`CourseID` a `StudentID`). Tabulku spojení m: n bez datové části se někdy nazývá čistou vazební tabulku (PJT).

`Instructor` a `Course` entit mít vztah m: n pomocí čistou vazební tabulku.

Poznámka: EF 6.x podporuje implicitní spojení tabulky pro relace m: n, ale základní EF nepodporuje. Další informace najdete v tématu [m: n vztahy v EF základní 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>CourseAssignment entity

![CourseAssignment entity](complex-data-model/_static/courseassignment-entity.png)

Vytvoření *Models/CourseAssignment.cs* následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Lektorem kurzy

![M:M lektorem kurzy](complex-data-model/_static/courseassignment.png)

Relace m: n lektorem kurzy:

* Vyžaduje připojení k tabulku, která musí být reprezentovaná sadu entit.
* Je tabulka čistý spojení (tabulky bez datovou část).

Je běžné název entity spojení `EntityName1EntityName2`. Je třeba připojení k tabulce lektorem kurzy s využitím tohoto vzoru `CourseInstructor`. Doporučujeme však používat název, který popisuje relace.

Datové modely spustí jednoduchou a zvýší. Žádné datové spojení (PJTs) se často momentální zahrnout datové části. Od entity popisný název, název nemusí změnit, když se změní v tabulce spojení. Připojení entity v ideálním případě by mít svůj vlastní přirozené název (pravděpodobně jednoslovné) v doméně obchodní. Například může knihy a zákazníky propojené s názvem hodnocení spojovací entity. Pro relaci n: n lektorem kurzy `CourseAssignment` upřednostňuje přes `CourseInstructor`.

### <a name="composite-key"></a>Složený klíč

FKs nejsou s možnou hodnotou Null. Dva FKs v `CourseAssignment` (`InstructorID` a `CourseID`) společně jednoznačně každý řádek `CourseAssignment` tabulky. `CourseAssignment` nevyžaduje vyhrazený PK. `InstructorID` a `CourseID` vlastnosti fungovat jako složený PK. Je jediný způsob, jak určit složené PKs na jádro EF s *rozhraní fluent API*. V další části ukazuje, jak nakonfigurovat složené PK.

Složený klíč zajistí:

* Pro jeden kurzu jsou povoleny více řádků.
* Pro jeden lektorem jsou povoleny více řádků.
* Více řádků pro stejný lektorem a kurzu není povoleno.

`Enrollment` Entity spojení definuje vlastní Primárníklíč, je možné, duplikáty toto řazení. Aby se tyto duplikáty:

* Přidejte jedinečný index na pole cizího klíče nebo
* Konfigurace `Enrollment` s primární složený klíč podobná `CourseAssignment`. Další informace najdete v tématu [indexy](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>Aktualizovat kontext databáze

Přidejte následující zvýrazněný kód, který *Data/SchoolContext.cs*:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Předchozí kód přidá nové entity a nakonfiguruje `CourseAssignment` složené PK. entity

## <a name="fluent-api-alternative-to-attributes"></a>Fluent API alternativní atributů

`OnModelCreating` Metoda v předchozím kód používá *rozhraní fluent API* ke konfiguraci základní EF chování. Rozhraní API se nazývá "fluent", protože se často používají v rozvádět řadu volání metod společně do jednoho příkazu. [Následující kód](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) je příkladem rozhraní fluent API:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

V tomto kurzu se používá rozhraní fluent API jenom pro mapování DB, který nelze provést s atributy. Rozhraní fluent API můžete však zadat většinu formátování, ověření a pravidla mapování, které lze provést s atributy.

Některé atributy, jako `MinimumLength` nelze použít s rozhraní fluent API. `MinimumLength` nepodporuje změnu schématu, vztahuje se pouze minimální délka ověřovacího pravidla.

Někteří vývojáři dávají přednost používání rozhraní fluent API výhradně tak, aby se zachovat jejich tříd entit "čistou." Atributy a rozhraní fluent API můžete kombinovat. Existují některé konfigurace, které lze provádět jen pomocí rozhraní fluent API (výběr složené Primárníklíč). Existují některé konfigurace, které lze provést pouze s atributy (`MinimumLength`). Doporučený postup pro používání fluent API nebo atributy:

* Vyberte jednu z těchto dvou přístupů.
* Použijte konzistentně co nejvíce zvolený způsob.

Některé atributy používané v tomto kurzu se používají pro:

* Pouze ověřování (například `MinimumLength`).
* Pouze EF základní konfiguraci (například `HasKey`).
* Ověření a EF základní konfiguraci (například `[StringLength(50)]`).

Další informace o atributech oproti rozhraní fluent API najdete v tématu [metody konfigurace](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Diagram znázorňující entitami

Následující obrázek znázorňuje diagram, který vytvoření EF výkonné nástroje pro dokončené školní modelu.

![Entity diagram](complex-data-model/_static/diagram.png)

Na předchozím obrázku uvádí:

* Několik řádků vztah jeden mnoho (1 \*).
* Řádek vztah jeden pro žádná nebo jedna (1-0.1) mezi `Instructor` a `OfficeAssignment` entity.
* Řádek vztahů nula nebo 1 n (0.1 k *) mezi `Instructor` a `Department` entity.

## <a name="seed-the-db-with-test-data"></a>Počáteční hodnoty databáze s testovací Data

Aktualizujte kód v *Data/DbInitializer.cs*:

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Předchozí kód poskytuje data počáteční hodnoty pro nové entity. Většina tento kód vytvoří nové entity objekty a načte ukázková data. Ukázková data se používají pro testování. Předchozí kód vytvoří následující relace m: n:

* `Enrollments`
* `CourseAssignment`

Poznámka: [EF základní 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) bude podporovat [synchronizace replik indexů data](https://github.com/aspnet/EntityFrameworkCore/issues/629).

## <a name="add-a-migration"></a>Přidat migrace

Sestavte projekt. Otevřete okno příkazového řádku ve složce projektu a zadejte následující příkaz:

```console
dotnet ef migrations add ComplexDataModel
```

Předchozí příkaz zobrazí varování týkající se možná ztráta dat.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Pokud `database update` příkaz spustí, vytváří k následující chybě:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

Pokud migrace spouštějí s existujícími daty, může být omezení cizího klíče, které nejsou splněné existujícímu daty. V tomto kurzu se vytvoří nové databáze, takže neexistují žádné narušení omezení cizího klíče. V tématu [opravě omezení cizích klíčů s starší data](#fk) pokyny o tom, jak opravit porušení cizího klíče v aktuální databázi.

## <a name="change-the-connection-string-and-update-the-db"></a>Změňte připojovací řetězec a aktualizaci databáze

Kód v aktualizaci `DbInitializer` přidá počáteční hodnoty dat pro nové entity. Chcete-li vynutit EF jádra k vytvoření nové prázdné databáze:

* Změňte název připojovacího řetězce DB v *appSettings.JSON určený* k ContosoUniversity3. Nový název musí být název, který nebyl použit v počítači.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* Můžete taky odstraňte pomocí DB:

    * **SQL Server Object Explorer** (SSOX).
    * `database drop` Rozhraní příkazového řádku příkaz:

   ```console
   dotnet ef database drop
   ```

Spustit `database update` v příkazovém okně:

```console
dotnet ef database update
```

Předchozí příkaz spustí všechny migrace.

Spusťte aplikaci. Spuštění aplikace běží `DbInitializer.Initialize` metoda. `DbInitializer.Initialize` Naplní nové databáze.

Otevřete databázi v SSOX:

* Rozbalte **tabulky** uzlu. Zobrazí se vytvořené tabulky.
* Pokud SSOX byl dříve otevřen, klikněte **aktualizovat** tlačítko.

![Tabulky v SSOX](complex-data-model/_static/ssox-tables.png)

Zkontrolujte **CourseAssignment** tabulky:

* Klikněte pravým tlačítkem myši **CourseAssignment** tabulky a vyberte **Data zobrazení**.
* Ověřte **CourseAssignment** tabulka obsahuje data.

![CourseAssignment data v SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a>Oprava omezení cizích klíčů daty starší verze

Tato část je volitelné.

Pokud migrace spouštějí s existujícími daty, může být omezení cizího klíče, které nejsou splněné existujícímu daty. S použitím provozních dat je učinit migrovat existující data. Tato část poskytuje příklad opravit porušení omezení cizího klíče. Nevytvářejte tyto změny kódu bez předchozího provedení zálohy. Nevytvářejte tyto změny kódu, pokud byla dokončena v předchozí části a aktualizovat databázi.

*{Timestamp}_ComplexDataModel.cs* soubor obsahuje následující kód:

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

Předchozí kód přidá hodnotou Null `DepartmentID` cizího klíče na `Course` tabulky. Databáze z předchozí kurz obsahuje řádky v `Course`, takže tato tabulka nelze aktualizovat pomocí migrace.

Chcete-li `ComplexDataModel` migrace práci s existujícími daty:

* Změnit kód umožnit nového sloupce (`DepartmentID`) výchozí hodnotu.
* Vytvořte falešných oddělení s názvem "Temp" tak, aby fungoval jako výchozí oddělení.

### <a name="fix-the-foreign-key-constraints"></a>Opravte omezení cizího klíče

Aktualizace `ComplexDataModel` třídy `Up` metoda:

* Otevřete *{timestamp}_ComplexDataModel.cs* souboru.
* Komentář řádek kódu, který přidá `DepartmentID` sloupec, který se `Course` tabulky.

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Přidejte následující zvýrazněný kód. Nový kód přejde po `.CreateTable( name: "Department"` bloku: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

S předchozí změny, existující `Course` řádky budou související s oddělení "Temp" po `ComplexDataModel` `Up` metoda spustí.

Produkční aplikace bude:

* Přidat kód nebo skripty, které chcete přidat `Department` řádky a související `Course` řádky do nového `Department` řádků.
* Nepoužívat oddělení "Temp" nebo výchozí hodnotu pro `Course.DepartmentID`.

Další kurz se zaměřuje na související data.

>[!div class="step-by-step"]
[Předchozí](xref:data/ef-rp/migrations)
[další](xref:data/ef-rp/read-related-data)
