---
title: "Jádro ASP.NET MVC s EF Core - Model dat – 5 10"
author: tdykstra
description: "V tomto kurzu přidáte další entity a vztahy a přizpůsobit datový model zadáním formátování, ověřování a pravidla mapování databáze."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: d844e2a69e4bbfdf3942f2666ead0047bdf83b7a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a>Vytvoření modelu komplexní data - základní EF s kurz k ASP.NET MVC jádra (5 10)

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).

V předchozích kurzech pracovali s jednoduché datového modelu, který se skládá z tři entity. V tomto kurzu přidáte další entity a vztahy a datový model budete přizpůsobit zadáním formátování, ověřování a pravidla mapování databáze.

Jakmile budete hotovi, bude tříd entit tvoří dokončené datový model, který je znázorněno na následujícím obrázku:

![Entity diagram](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Přizpůsobení datového modelu s použitím atributů

V této části se zobrazí postup přizpůsobení datového modelu s použitím atributy, které určují, formátování, ověření a pravidla mapování databáze. Potom v některé z těchto částí, které vytvoříte kompletní datový model školní přidáním atributy třídám jste již vytvořili a vytvoření nové třídy pro zbývající typy entit v modelu.

### <a name="the-datatype-attribute"></a>Datový typ atributu

Pro studenty registrace kalendářních dat všech webových stránek aktuálně zobrazuje čas, spolu s datem, i když všechny, které se zajímáte o pro toto pole je datum. Pomocí datové poznámky atributy, můžete si ho code změny, která bude opravte formát zobrazení v každé zobrazení, která zobrazuje data. Chcete-li zobrazit příklad, jak to provést, že přidáte do atribut `EnrollmentDate` vlastnost v `Student` třídy.

V *Models/Student.cs*, přidejte `using` příkaz pro `System.ComponentModel.DataAnnotations` obor názvů a přidejte `DataType` a `DisplayFormat` atributů k `EnrollmentDate` vlastnost, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

`DataType` Atribut slouží k určení datový typ, který je specifičtější než vnitřní typ databáze. V tomto případě chceme jenom udržování přehledu o datum, není datum a čas. `DataType` Výčtu poskytuje pro mnoho typů dat, jako je například datum, čas, telefonní číslo, měny, EmailAddress a další. `DataType` Atributu můžete také povolit aplikace automaticky poskytnout konkrétní typ funkce. Například `mailto:` může vytvořit odkaz pro `DataType.EmailAddress`, a datum selektor lze zadat pro `DataType.Date` v prohlížečích podporujících HTML5. `DataType` Atribut vysílá standardu HTML 5 `data-` (výrazný data dash) atributy, které můžete porozumět standardu HTML 5 prohlížeče. `DataType` Atributy neposkytují žádné ověření.

`DataType.Date`neuvádí formát data, které se zobrazí. Ve výchozím nastavení je datové pole zobrazí podle výchozích formátů podle serveru CultureInfo.

`DisplayFormat` Atribut slouží k explicitnímu zadání formát data:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` Nastavení určuje, že formátování, které také bude použito při hodnota je zobrazena v textovém poli pro úpravy. (Není vhodné, aby pro některá pole – například hodnoty měny, nemusí chcete symbolu měny do textového pole pro úpravy.)

Můžete použít `DisplayFormat` atribut podle sám sebe, ale obecně je vhodné používat `DataType` také atribut. `DataType` Atribut přenese tak sémantika data a jak vykreslit ho na obrazovce a nabízí následující výhody, které není dostupná s `DisplayFormat`:

* V prohlížeči můžete povolit funkce HTML5 (například k zobrazení ovládacího prvku kalendář, symbolu měny vhodné národního prostředí, e-mailu odkazů některé klientské zadejte ověření atd.).

* Ve výchozím nastavení bude v prohlížeči vykreslovat data ve správném formátu podle národního prostředí.

Další informace najdete v tématu [ \<vstupní > označení dokumentaci pomocná](../../mvc/views/working-with-forms.md#the-input-tag-helper).

Spusťte aplikaci, přejděte na stránku studenty Index a Všimněte si, že časy se už nezobrazují mezi daty registrace. Stejné bude platit pro všechna zobrazení, která používá Student model.

![Studenti, kteří index stránky zobrazující data bez časů](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>Atribut StringLength

Můžete také zadat pravidla ověření dat a chybové zprávy ověření pomocí atributů. `StringLength` Atribut Nastaví maximální délku v databázi a poskytuje na straně klienta a na straně serveru ověřování pro architekturu ASP.NET MVC. Minimální délka řetězce. můžete také zadat v tomto atributu, ale minimální hodnota nemá žádný vliv na schéma databáze.

Předpokládejme, že chcete zajistit, že uživatelé nezadávejte víc než 50 znaků pro název. Chcete-li přidat toto omezení, přidejte `StringLength` atributů k `LastName` a `FirstMidName` vlastnosti, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

`StringLength` Atribut nebude uživatel zabránit v přechodu do prázdných znaků pro název. Můžete použít `RegularExpression` atribut použít omezení na vstup. Například následující kód vyžaduje první znak, který má být velkými písmeny a zbývající znaků, které mají být abecední:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

`MaxLength` Atribut poskytuje podobné funkce `StringLength` atribut, ale neposkytuje na straně klienta ověření.

Model databáze se změnilo tak, že vyžaduje změnu schématu databáze. Migrace budete používat k aktualizaci schématu bez ztráty dat, která jste mohli přidat do databáze pomocí aplikace uživatelského rozhraní.

Uložte změny a sestavte projekt. Potom otevřete příkazové okno ve složce projektu a zadejte následující příkazy:

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

`migrations add` Příkaz upozorní, že může dojít ke ztrátě dat, protože tato změna umožňuje maximální délku kratší pro dva sloupce.  Migrace vytvoří soubor s názvem  *\<časové razítko > _MaxLengthOnNames.cs*. Tento soubor obsahuje kód `Up` metoda, která aktualizuje databázi tak, aby odpovídala aktuální datový model. `database update` Tento kód byl spuštěn příkaz.

Časové razítko předponu k názvu souboru migrace je pomocí rozhraní Entity Framework sloužící k uspořádání byla migrace. Můžete vytvořit více migrací před spuštěním příkazu update-database, a potom všechny byla migrace se použijí v pořadí, ve kterém byly vytvořeny.

Spuštění aplikace, vyberte **studenty** , klikněte na **vytvořit nový**a zadejte buď název delší než 50 znaků. Když kliknete na tlačítko **vytvořit**, ověřování na straně klienta zobrazí chybovou zprávu.

![Studenti, kteří index stránky zobrazující chyby délka řetězce](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a>Atribut sloupce

Atributy můžete taky řídit, jak jsou mapovány třídy a vlastnosti do databáze. Předpokládejme, že při použití názvu `FirstMidName` pro první název pole, protože pole může obsahovat také křestní jméno. Ale chcete sloupci databáze s názvem `FirstName`, protože jsou uživatelé, kteří budou být zápis dotazů ad-hoc v databázi zvykli tohoto názvu. Chcete-li toto mapování, můžete použít `Column` atribut.

`Column` Atribut určuje, že při vytvoření databáze, sloupec `Student` tabulku, která se mapuje `FirstMidName` vlastnost bude mít název `FirstName`. Jinými slovy, pokud váš kód odkazuje na `Student.FirstMidName`, data budou pocházet z nebo aktualizovány v `FirstName` sloupec `Student` tabulky. Pokud nezadáte názvy sloupců, získá se stejný název jako název vlastnosti.

V *Student.cs* soubor, přidejte `using` příkaz pro `System.ComponentModel.DataAnnotations.Schema` a přidejte atribut název sloupce, který se `FirstMidName` vlastnost, jak je znázorněno v následující zvýrazněný kód:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Přidání `Column` základní model, změní se atribut `SchoolContext`, takže nebude odpovídat databázi.

Uložte změny a sestavte projekt. Potom otevřete příkazové okno ve složce projektu a zadejte následující příkazy k vytvoření jiná migrace:

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

V **Průzkumník objektů systému SQL Server**, otevřete návrháře tabulky Student dvojitým kliknutím **Student** tabulky.

![Studenti, kteří tabulky v SSOX po migrace](complex-data-model/_static/ssox-after-migration.png)

Před použitím první dva migrace se název sloupce z typu nvarchar(MAX). Jsou nyní nvarchar(50) a název sloupce se změnil z FirstMidName na FirstName.

> [!Note]
> Pokud se pokusíte zkompilovat před dokončení vytváření všechny třídy entity v následujících částech, může docházet k chybám kompilátoru.

## <a name="final-changes-to-the-student-entity"></a>Poslední změny Student entity

![Student entity](complex-data-model/_static/student-entity.png)

V *Models/Student.cs*, nahraďte kód, který jste přidali dříve následujícím kódem. Změny se zvýrazněnou.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Požadovaný atribut

`Required` Atribut je povinná pole název vlastnosti. `Required` Atribut není nutný pro použití hodnot Null typy, jako jsou typy hodnot (data a času, int, dvakrát, float, atd.). Typy, které nemůže mít hodnotu null, jsou automaticky považovány za povinná pole.

Je vhodné odebrat `Required` atribut a nahraďte ji metodou minimální délku parametru pro `StringLength` atribut:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Atribut zobrazení

`Display` Atribut určuje, že titulek pro do textových polí by měl být "Jméno", "Příjmení", "Úplný název" a "Registrace datum" namísto název vlastnosti v každé instanci (což je žádné místo dělení slova).

### <a name="the-fullname-calculated-property"></a>Vlastnost FullName vypočítat

`FullName`je počítané vlastnosti, která vrátí hodnotu, která se vytvoří zřetězením dva další vlastnosti. Proto má pouze přistupující a ne `FullName` vygeneruje sloupec v databázi.

## <a name="create-the-instructor-entity"></a>Vytvořit entitu lektorem

![Entitu lektorem](complex-data-model/_static/instructor-entity.png)

Vytvoření *Models/Instructor.cs*, nahraďte kód šablony s následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Všimněte si, že několik vlastností, které jsou stejné ve Student a lektorem entity. V [implementace dědičnosti](inheritance.md) kurz později z této série budete Refaktorovat tento kód eliminovat redundance.

Můžete vložit více atributů na jeden řádek, můžete také napsat `HireDate` atributy následujícím způsobem:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments a OfficeAssignment navigační vlastnosti

`CourseAssignments` a `OfficeAssignment` vlastnosti jsou navigační vlastnosti.

Lektorem můžete naučit libovolný počet kurzy, takže `CourseAssignments` je definována jako kolekce.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Pokud vlastnost navigace mohou být uloženy více entit, její typ musí být seznam, ve kterém položky lze přidat, odstranit a aktualizovat.  Můžete zadat `ICollection<T>` , nebo typu, jako `List<T>` nebo `HashSet<T>`. Pokud zadáte `ICollection<T>`, vytvoří EF `HashSet<T>` kolekce ve výchozím nastavení.

Důvod, proč se `CourseAssignment` entity je popsáno níže v části o relace m: n.

Contoso univerzity obchodní pravidla o stavu lektorem může mít pouze maximálně jeden office, proto `OfficeAssignment` vlastnost obsahuje jednu entitu OfficeAssignment (který může mít hodnotu null, pokud není přiřazena žádná office).

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Vytvořit entitu OfficeAssignment

![OfficeAssignment entity](complex-data-model/_static/officeassignment-entity.png)

Vytvoření *Models/OfficeAssignment.cs* následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Klíč atributu

Je--nula nebo 1 vztah mezi lektorem a entitami, OfficeAssignment. Přiřazení office existuje pouze ve vztahu k lektorem, které je přiřazen, a proto jeho primární klíč je také jeho cizí klíč entitu lektorem. Ale rozhraní Entity Framework nelze rozpoznat automaticky InstructorID jako primární klíč tuto entitu vzhledem k tomu, že její název není podle ID nebo classnameID zásady vytváření názvů. Proto `Key` atribut slouží k identifikaci jako klíč:

```csharp
[Key]
public int InstructorID { get; set; }
```

Můžete také `Key` atribut Pokud entity mít svůj vlastní primární klíč, ale budete chtít název vlastnost něco jiného než classnameID nebo ID.

Ve výchozím nastavení EF klíče jsou považovány za jiný databáze generované protože sloupec je sloupec pro identifikační vztah.

### <a name="the-instructor-navigation-property"></a>Vlastnost navigace lektorem

Lektorem entita, která má s možnou hodnotou Null `OfficeAssignment` navigační vlastnost (protože lektorem nemusí mít přiřazení office), a OfficeAssignment entita, která má hodnotou Null `Instructor` navigační vlastnost (protože nelze přiřazení office Existují bez lektorem – `InstructorID` neumožňuje hodnotu Null). Pokud entitu lektorem související entity OfficeAssignment, bude mít každá entita odkaz na další jeden v její navigační vlastnosti.

By mohlo `[Required]` atribut na navigační vlastnost lektorem k určení, že musí být související lektorem, ale nemáte k tomu, protože `InstructorID` cizí klíč (což je také klíč, který chcete tuto tabulku) je použití hodnot Null.

## <a name="modify-the-course-entity"></a>Upravit kurzu Entity

![Během entity](complex-data-model/_static/course-entity.png)

V *Models/Course.cs*, nahraďte kód, který jste přidali dříve následujícím kódem. Změny se zvýrazněnou.

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

Během entita má vlastností cizího klíče `DepartmentID` který odkazuje na související entity oddělení a má `Department` navigační vlastnost.

Rozhraní Entity Framework nevyžaduje, můžete přidat vlastností cizího klíče do datového modelu, když máte navigační vlastnost pro související entity.  EF automaticky vytvoří cizí klíče v databázi bez ohledu na jejich jste potřeby a vytvoří [stínové vlastnosti](https://docs.microsoft.com/ef/core/modeling/shadow-properties) pro ně. Ale s cizí klíč v datovém modelu můžete provést aktualizace teď jednodušší a efektivnější. Například při fetch kurzu entity upravit oddělení entita je null. Pokud nemáte načíst ho, tak při aktualizaci entity kurzu, budete muset nejdřív načíst entity oddělení. Pokud vlastnost cizího klíče `DepartmentID` je zahrnutá v datovém modelu, nemusíte načtení entity oddělení dřív, než je aktualizovat.

### <a name="the-databasegenerated-attribute"></a>Atribut DatabaseGenerated

`DatabaseGenerated` Atribut s `None` parametr na `CourseID` vlastnost určuje, že hodnot primárního klíče jsou zadané uživatelem, nikoli generované databáze.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Ve výchozím nastavení rozhraní Entity Framework předpokládá, že hodnot primárního klíče generované databáze. To je, co chcete ve většině scénářů. Pro entity kurzu, budete však použít několik zadán uživatel během například řadu 1000 oddělení jednu řadu 2000 pro jiné oddělení a tak dále.

`DatabaseGenerated` Atribut můžete také použít ke generování výchozí hodnoty, jako v případě sloupců databáze slouží k záznamu datum řádek byl vytvořen nebo aktualizován.  Další informace najdete v tématu [generované vlastnosti](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Cizí klíč a navigační vlastnosti

Vlastnosti cizího klíče a navigačních vlastností v entitě kurzu odráží následující relace:

Kurz je přiřazena k jedné oddělení, takže není `DepartmentID` cizí klíč a `Department` navigační vlastnost z důvodů uvedených výše.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Kurz může mít libovolný počet studenty zaregistrované v něm proto `Enrollments` navigační vlastnost je kolekce:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Kurz může výukové ve více vyučující, proto `CourseAssignments` navigační vlastnost je kolekce (typ `CourseAssignment` je vysvětlen [později](#many-to-many-relationships)):

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a>Vytvořit entitu oddělení

![Oddělení entity](complex-data-model/_static/department-entity.png)


Vytvoření *Models/Department.cs* následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Atribut sloupce

Dříve jste použili `Column` atribut změnit mapování název sloupce. V kódu pro entitu oddělení `Column` atribut je používána pro změnit SQL mapování datového typu, aby sloupec bude nutné definovat pomocí typ money systému SQL Server v databázi:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapování sloupce není povinné, protože rozhraní Entity Framework vybere odpovídající typ dat systému SQL Server na základě typu CLR, které definujete pro vlastnost. Modul CLR `decimal` zadejte map k systému SQL Server `decimal` typu. Ale v takovém případě víte, že sloupec bude podržíte částky měny a je vhodnější pro tento datový typ money.

### <a name="foreign-key-and-navigation-properties"></a>Cizí klíč a navigační vlastnosti

Vlastnosti cizího klíče a navigační odráží následující relace:

Oddělení může nebo nemusí mít správce a správce je vždy lektorem. Proto `InstructorID` vlastnost je zahrnuta jako cizí klíč na entitu lektorem a otazník bude přidán za `int` zadejte označení označit vlastnost jako s možnou hodnotou Null. Navigační vlastnost jmenuje `Administrator` , ale obsahuje entitu lektorem:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Oddělení může mít mnoho kurzy, takže není navigační vlastnost kurzy:

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> Podle konvence rozhraní Entity Framework umožňuje kaskádové odstranění pro použití hodnot Null cizí klíče a pro relace m: n. Výsledkem může být Cyklické kaskádové odstranění pravidla, která způsobí výjimku při pokusu o přidání migrace. Například pokud vlastnost Department.InstructorID neuvedli jako s možnou hodnotou Null, EF byste nakonfigurovali odstranit lektorem při odstranění oddělení, která není, co se stane chcete odstranit pravidlo cascade. V případě potřeby obchodní pravidla `InstructorID` vlastnost, která má mít hodnotu Null, je třeba použít následující příkaz rozhraní API fluent zakázat kaskádové odstranění v relaci:
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a>Upravit registraci entity

![Registrace entity](complex-data-model/_static/enrollment-entity.png)

V *Models/Enrollment.cs*, nahraďte kód, který jste přidali dříve následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Cizí klíč a navigační vlastnosti

Vlastnosti cizího klíče a navigačních vlastností odráží následující relace:

Záznam zápisu je jeden kurzu, takže není `CourseID` vlastností cizího klíče a `Course` navigační vlastnost:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Záznam zápisu je pro jeden student, takže není `StudentID` vlastností cizího klíče a `Student` navigační vlastnost:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relace m: N

Je relace m: n mezi studenty a kurzu entity a entity registrace funguje jako tabulku spojení m: n *s odebranou datovou částí* v databázi. "S odebranou datovou částí" znamená, že registrace tabulka obsahuje další údaje kromě cizí klíče pro spojené tabulky (v tomto případě primární klíč a vlastnost úrovni).

Následující obrázek znázorňuje, jak tyto relace vypadat v diagramu entity. (Tento diagram byl vygenerován pomocí nástroje Entity Framework napájení pro EF 6.x vytváření diagramu není součástí tohoto kurzu, je právě používán sem jako obrázek.)

![Během student mnoho k mnoha relace](complex-data-model/_static/student-course.png)

Každý řádek vztahů je 1 na jeden element end a znak hvězdičky (*) v dalších, která určuje vztah jeden mnoho.

Pokud v tabulce registrace nezahrnuli úrovni informace, jenom třeba, aby obsahovat dvě cizí klíče CourseID a StudentID. V takovém případě je m: n spojení tabulku bez datová část (nebo čistou vazební tabulku) v databázi. Entity lektorem a kurzu mají tento druh relace m: n a dalším krokem je vytvoření třídu entity jako tabulku spojení bez datové části.

(EF 6.x podporuje implicitní spojení tabulky pro relace m: n, ale základní EF nepodporuje. Další informace najdete v tématu [diskuse v úložišti GitHub základní EF](https://github.com/aspnet/EntityFramework/issues/1368).) 

## <a name="the-courseassignment-entity"></a>CourseAssignment entity

![CourseAssignment entity](complex-data-model/_static/courseassignment-entity.png)

Vytvoření *Models/CourseAssignment.cs* následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>Připojení k názvy entit

Je nutné připojení k tabulce v databázi pro vztah m: n lektorem kurzy a musí být reprezentovaná sadu entit. Je běžné název entity spojení `EntityName1EntityName2`, který v tomto případě bude `CourseInstructor`. Nicméně doporučujeme, abyste zvolili název, který popisuje relace. Datové modely spustí jednoduchou a růst s žádné datové spojení často získávání datových částí později. Pokud začnete se entity popisný název, nebudete muset později změnit název. Připojení entity v ideálním případě by mít svůj vlastní přirozené název (pravděpodobně jednoslovné) v doméně obchodní. Například může knihy a zákazníky spojený prostřednictvím hodnocení. Pro tento vztah `CourseAssignment` je vhodnější než `CourseInstructor`.

### <a name="composite-key"></a>Složený klíč

Vzhledem k tomu, že cizí klíče nejsou s možnou hodnotou Null a společně jednoznačně identifikovat každý řádek v tabulce, není nutné pro samostatný primární klíč. *InstructorID* a *CourseID* vlastnosti by měla fungovat jako složený primární klíč. Jediný způsob, jak identifikovat složené primární klíče, aby EF je pomocí *rozhraní fluent API* (není možné provést pomocí atributů). Uvidíte, jak nakonfigurovat složené primární klíč v další části.

Složený klíč zajistí, že i když můžete mít více řádků pro jeden kurzu a více řádků pro jeden lektorem, nemůže mít více řádků pro stejný lektorem a kurzu. `Enrollment` Spojení entity definuje vlastní primární klíč, takže je možné, duplikáty toto řazení. Nechcete, aby tyto duplikáty, můžete přidat jedinečný index na pole cizího klíče, nebo nakonfigurovat `Enrollment` s primární složený klíč podobná `CourseAssignment`. Další informace najdete v tématu [indexy](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-database-context"></a>Aktualizace kontext databáze

Přidejte následující zvýrazněný kód, který *Data/SchoolContext.cs* souboru:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Tento kód přidá nové entity a nakonfiguruje složené primární klíče CourseAssignment entity.

## <a name="fluent-api-alternative-to-attributes"></a>Fluent API alternativní atributů

Kód v `OnModelCreating` metodu `DbContext` třídy používá *rozhraní fluent API* pro konfiguraci EF chování. Rozhraní API se nazývá "fluent", protože se často používají v rozvádět do jednoho příkazu, jako v následujícím příkladě z řady volání metod společně [EF základní dokumentace](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

V tomto kurzu používáte rozhraní fluent API jenom pro mapování databáze, které nemůžete dělat s atributy. Rozhraní fluent API však můžete také použít k určení většinu formátování, ověření a pravidla mapování, které můžete provést pomocí atributů. Některé atributy, jako `MinimumLength` nelze použít s rozhraní fluent API. Jak je uvedeno nahoře, `MinimumLength` nemění schématu, vztahuje se pouze klientské a serverové straně ověřovací pravidlo.

Někteří vývojáři dávají přednost používání rozhraní fluent API výhradně tak, aby se zachovat jejich tříd entit "čistou." Pokud chcete, a neexistují několik úprav, které lze provést pouze pomocí rozhraní fluent API je možné kombinovat atributy a rozhraní fluent API, ale obecně doporučený postup je vyberte jednu z těchto dvou přístupů a používání který konzistentně co nejvíc. Pokud používáte obě, Všimněte si vždy, když dojde ke konfliktu, rozhraní Fluent API přepsání atributy.

Další informace o atributech oproti rozhraní fluent API najdete v tématu [metody konfigurace](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Diagram znázorňující entitami

Následující obrázek znázorňuje diagram, který vytvořit výkonné nástroje Entity Framework pro dokončené školní model.

![Entity diagram](complex-data-model/_static/diagram.png)

Kromě řádky vztah jeden mnoho (1 \*), můžete tady vidíte řádku vztah jeden pro žádná nebo jedna (1-0..1) mezi lektorem a OfficeAssignment entity a řádku nula nebo 1 n relace (0..1 k *) mezi Entity lektorem a oddělení.

## <a name="seed-the-database-with-test-data"></a>Naplnit databázi daty testu

Nahraďte kód v *Data/DbInitializer.cs* souboru následujícím kódem, chcete-li poskytovat počáteční hodnoty dat pro nové entity, které jste vytvořili.

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Jak už jste viděli v první kurz, většina tento kód jednoduše vytvoří nové entity objekty a načte ukázková data do vlastnosti podle potřeby pro testování. Všimněte si, jak jsou zpracovávány relace m: n: kód vytvoří vztahy vytvořením entity v `Enrollments` a `CourseAssignment` připojení sady entit.

## <a name="add-a-migration"></a>Přidat migrace

Uložte změny a sestavte projekt. Potom otevřete příkazové okno ve složce projektu a zadejte `migrations add` příkazu (nedělají nic příkazu update-database ještě):

```console
dotnet ef migrations add ComplexDataModel
```

Zobrazí se upozornění o ztráta dat.

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Pokud jste se pokusili spustit `database update` příkaz v tomto okamžiku (není to je ještě), se zobrazí následující chyba:

> Příkaz ALTER TABLE konflikt s omezení FOREIGN KEY "FK_dbo. Course_dbo. Department_DepartmentID". Ke konfliktu došlo v databázi "ContosoUniversity" tabulka "dbo. Oddělení", sloupec 'DepartmentID'.

V případech, kdy můžete spustit migrace s existujícími daty, je třeba vložit se zakázaným inzerováním data do databáze, aby pokryl omezení cizího klíče. Generovaný kód v `Up` metoda přidá do tabulky během použití hodnot Null DepartmentID cizí klíč. Pokud již existují řádky v tabulce kurzu při spuštění kódu `AddColumn` operace selže, protože systém SQL Server není známo, jaké hodnoty Pokud chcete umístit do sloupce, který nemůže mít hodnotu null. Pro účely tohoto kurzu je potřeba spustit migraci na novou databázi, ale v praxi by musíte provádět migrace zpracovat existující data, takže zobrazit příklad toho, jak to udělat následující pokyny.

Chcete-li tato migrace pracovat s existujícími daty, budete muset změnit kód umožnit nového sloupce výchozí hodnotu a vytvořit se zakázaným inzerováním oddělení s názvem "Temp" tak, aby fungoval jako výchozí oddělení. V důsledku toho existující kurzu řádky budou všechny souviset oddělení "Temp" po `Up` metoda spustí.

* Otevřete *{timestamp}_ComplexDataModel.cs* souboru. 

* Zakomentovat řádek kódu, který přidá sloupec DepartmentID kurzu tabulky.

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* Přidejte následující zvýrazněný kód po kód, který vytvoří tabulku oddělení:

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

V případě produkční aplikace by napíšete kód nebo skripty k přidání oddělení řádky a řádky kurzu se týkají nové řádky oddělení. Potom už potřebovali byste oddělení "Temp" nebo výchozí hodnotu pro sloupec Course.DepartmentID.

Uložte změny a sestavte projekt.

## <a name="change-the-connection-string-and-update-the-database"></a>Změňte připojovací řetězec a aktualizaci databáze

Nyní máte nový kód `DbInitializer` třída, která přidává počáteční hodnoty dat pro nové entity pro prázdnou databázi. Chcete-li EF vytvoření nové prázdné databáze, změňte název databáze v připojovacím řetězci v *appSettings.JSON určený* ContosoUniversity3 nebo jiný název, který nepoužili na počítači, který používáte.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

Uložte změnu do *appSettings.JSON určený*.

> [!NOTE]
> Jako alternativu k změnit název databáze můžete odstranit databázi. Použití **Průzkumník objektů systému SQL Server** (SSOX) nebo `database drop` rozhraní příkazového řádku příkaz:
> ```console
> dotnet ef database drop
> ```

Po jste změnili název databáze nebo odstranit databázi, spusťte `database update` příkazu v příkazovém okně spuštění migrace.

```console
dotnet ef database update
```

Spusťte aplikaci způsobit, že `DbInitializer.Initialize` metoda spustit a přidat nové databáze.

Otevřít databázi v SSOX, jako jste to udělali dříve a rozbalte **tabulky** uzel pro zobrazení, že všechny tabulky byly vytvořeny. (Pokud máte SSOX otevřít z dřívější čas, klikněte na tlačítko **aktualizovat** tlačítko.)

![Tabulky v SSOX](complex-data-model/_static/ssox-tables.png)

Spusťte aplikaci pro aktivaci inicializátoru kód, který doplňuje pro databázi.

Klikněte pravým tlačítkem myši **CourseAssignment** tabulky a vyberte **Data zobrazení** ověřit, zda má data v ní.

![CourseAssignment data v SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a>Souhrn

Nyní máte složitější datový model a příslušné databáze. V následujícím kurzu se dozvíte informace o tom, jak získat přístup k datům související.

>[!div class="step-by-step"]
[Předchozí](migrations.md)
[další](read-related-data.md)  
