---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Vytvoření složitějšího datového modelu pro aplikace ASP.NET MVC (4 z 10) | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje postup vytvoření aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a sady Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a7fbcf8dc41086764e1e8ba9055929bde132192a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375736"
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>Vytvoření složitějšího datového modelu pro aplikace ASP.NET MVC (4 z 10)
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a Visual Studio 2012. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Série kurzů můžete začít od začátku nebo [stáhnout počáteční projekt pro tuto kapitolu](building-the-ef5-mvc4-chapter-downloads.md) a začněte tady.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte sami, [stáhnout dokončený kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a zkuste problém reprodukovat. Porovnáním kód Dokončený kód v obecně najdete řešení problému. Některé běžné chyby a jejich řešení najdete v tématu [chyby a náhradní řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V předchozích kurzech jste pracovali s jednoduchý datový model, který se skládá z tři entity. V tomto kurzu přidáte další entity a relace a tak, že zadáte formátování, ověřování a pravidel mapování database budete Přizpůsobte si datový model. Zobrazí se vám dva způsoby, jak Přizpůsobte si datový model: přidáním atributů do tříd entit a přidáním kódu do třídy kontextu databáze.

Jakmile budete hotovi, tříd entit budou použity k vytvoření dokončeného datový model, který je znázorněno na následujícím obrázku:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Přizpůsobte si datový Model s použitím atributů

V této části uvidíte, jak přizpůsobit datového modelu s použitím atributů, které určují databáze mapování pravidel ověřování a formátování. Pak v některé z následujících částí vytvoříte kompletní `School` modelu data přidáním atributy do třídy již vytvořené a vytváření nových tříd pro zbývající typy entit v modelu.

### <a name="the-datatype-attribute"></a>Datový typ atributu

Student zápisu dat všechny webové stránky aktuálně zobrazit čas spolu s datem, i když je všechno, co vás zajímá pro toto pole datum. S použitím anotace atributů dat, můžete si ho kódu změnu, která budou v každé zobrazení, která zobrazuje data opravit formát zobrazení. Chcete-li zobrazit příklad tohoto postupu, že přidáte atribut, který má `EnrollmentDate` vlastnost `Student` třídy.

V *Models\Student.cs*, přidejte `using` příkaz pro `System.ComponentModel.DataAnnotations` obor názvů a přidejte `DataType` a `DisplayFormat` atributů `EnrollmentDate` vlastnost, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

[Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atribut se používá k určení datový typ, který je specifičtější než vnitřní typ databáze. V tomto případě chceme jenom udržovat přehled o datum, nejsou data a času. [Datový typ výčtu](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) poskytuje pro mnoho typů dat, například *datum, čas, telefonní číslo, Měna, EmailAddress* a provádění dalších akcí. `DataType` Atribut můžete také povolit aplikace a zajistit tak automaticky specifické pro typ funkce. Například `mailto:` propojení lze vytvořit pro [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), a selektor date lze zadat pro [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) v prohlížečích podporujících [HTML5](http://html5.org/). [Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributy vysílá HTML 5 [data -](http://ejohn.org/blog/html-5-data-attributes/) (vyslovováno *data dash*) atributy, které umožní pochopit prohlížeče HTML 5. [Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributy se neposkytuje žádné ověřování.

`DataType.Date` neurčuje formátu, který se zobrazí datum. Ve výchozím nastavení, zobrazí se pole data podle výchozí formát založený na serveru [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

`DisplayFormat` Atribut se používá s ohledem na formát data:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode` Nastavení určuje, že zadané formátování také bude použito při hodnota se zobrazí v textovém poli pro úpravu. (Pokud nechcete pro některá pole – například pro hodnoty měny, nemusí chcete symbol měny v textovém poli pro úpravu.)

Můžete použít [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut samotný, ale to je obecně vhodné použít [datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) také atribut. `DataType` Atribut přenáší *sémantiku* dat jako rozdíl od vykreslování na obrazovce a nabízí následující výhody, které vám s `DisplayFormat`:

- Funkce HTML5 (pro příklad, který znázorňuje ovládacího prvku kalendář, symbol měny odpovídající národní prostředí, odkazy na e-mailu, atd.) můžete povolit v prohlížeči.
- Ve výchozím prohlížeči bude vykreslovat data ve správném formátu, na základě vašich [národní prostředí](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atribut můžete povolit MVC zvolit šablonu pravé pole k poskytnutí dat ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) pokud používá sama používá šablony řetězce). Další informace najdete v tématu Brada Wilsona [šablony ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (I když napsané pro MVC 2, tento článek stále platí pro aktuální verzi technologie ASP.NET MVC.)

Pokud používáte `DataType` atribut pomocí pole pro datum, budete muset zadat `DisplayFormat` atribut také aby se správně vykresluje pole v prohlížečích Chrome. Další informace najdete v tématu [toto vlákno na StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Znovu spusťte Student indexovou stránku a Všimněte si, že časy se už nezobrazují pro data registrací. Stejné budou platit pro všechna zobrazení, která se používá `Student` modelu.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

Můžete také zadat data ověřovací pravidla a zpráv pomocí atributů. Předpokládejme, že chcete mít jistotu, že uživatelé nezadávejte více než 50 znaků pro název. Chcete-li přidat toto omezení, přidejte [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atributů `LastName` a `FirstMidName` vlastnosti, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atribut nebude zabránit uživateli v zadávání prázdné znaky pro název. Můžete použít [regulární výraz](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atributu použít omezení na vstup. Například následující kód vyžaduje prvního znaku na velká písmena a zbývající znaky, které mají být abecední znak:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) atribut poskytuje podobné funkce jako [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atribut, ale neposkytuje na straně klienta ověření.

Spusťte aplikaci a klikněte na tlačítko **studenty** kartu. Zobrazí následující chyba:

*Model zálohování kontextu 'SchoolContext' byl změněn, protože byla vytvořena databáze. Zvažte použití migrace Code First k aktualizaci databáze ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Model databáze byl změněn způsobem, který vyžaduje změnu schématu databáze a Entity Framework, které zjistil. Migrace budete používat k aktualizaci schématu bez ztráty dat, který jste přidali k databázi pomocí uživatelského rozhraní. Pokud jste změnili dat, který byl vytvořen `Seed` metoda, která bude změněna zpět do původního stavu z důvodu [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) metodu, která používáte v `Seed` metody. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) je ekvivalentní k operaci "upsert", z databáze terminologie.)

V balíčku správce konzoly (konzolu PMC), zadejte následující příkazy:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration MaxLengthOnNames` Příkaz vytvoří soubor s názvem  *&lt;časové razítko&gt;\_MaxLengthOnNames.cs*. Tento soubor obsahuje kód, který aktualizuje databázi tak, aby odpovídaly aktuálním datovým modelem. Časové razítko pro název souboru migrace používá Entity Framework pro řazení migrace. Po vytvoření více migrace, je-li vyřadit databázi nebo nasazení projektu s použitím migrace, všechny migrace se použijí v pořadí, ve kterém byly vytvořeny.

Spustit **vytvořit** stránku a zadejte buď název delší než 50 znaků. Jakmile být delší než 50 znaků, ověřování na straně klienta okamžitě zobrazí chybovou zprávu.

![Chyba val straně klienta](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>Atribut sloupce

Atributy můžete také řídit, jak třídy a vlastnosti jsou namapovány na databázi. Předpokládejme, že byste použili název `FirstMidName` pro název prvního pole, protože pole může obsahovat také křestní jméno. Chcete sloupec databáze s názvem, ale `FirstName`, protože uživatelé budou psát ad-hoc dotazy na databázi jsou zvyklí na tento název. Chcete-li toto mapování, můžete použít `Column` atribut.

`Column` Atribut určuje, že když se vytvoří databáze, sloupci `Student` tabulku, která se mapuje `FirstMidName` bude mít název vlastnosti `FirstName`. Jinými slovy, pokud váš kód odkazuje na `Student.FirstMidName`, data budou přicházet z nebo v aktualizovat `FirstName` sloupec `Student` tabulky. Pokud nechcete zadat názvy sloupců, jsou uvedeny stejný název jako název vlastnosti.

Přidat sadu pomocí příkazu pro [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) a atributu název sloupce `FirstMidName` vlastnost, jak je znázorněno v následující zvýrazněný kód:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Přidání [atribut sloupce](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) změní modelu zálohování SchoolContext, takže nebude odpovídat databázi. V konzole PMC vytvořit další migraci zadejte následující příkazy:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

V **Průzkumníka serveru** (**Průzkumník databáze** Pokud používáte Express for Web), dvakrát klikněte *Student* tabulky.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

Následující obrázek ukazuje původní název sloupce, protože byla použita první dvě migrace. Kromě názvu sloupce změny z `FirstMidName` k `FirstName`, dva sloupce název byly změněny z `MAX` délku až 50 znaků.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Můžete také provádět databáze pomocí změny mapování [rozhraní Fluent API](https://msdn.microsoft.com/data/jj591617), jak uvidíte později v tomto kurzu.

> [!NOTE]
> Při pokusu o kompilaci před dokončením vytvoření všech těchto tříd entit může docházet k chybám kompilátoru.


## <a name="create-the-instructor-entity"></a>Vytvoření Entity instruktorem

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Vytvoření *Models\Instructor.cs*, nahraďte kód šablony následujícím kódem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Všimněte si, že některé vlastnosti jsou ve stejné `Student` a `Instructor` entity. V [implementace dědičnosti](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) kurz později v této sérii, budete refaktorujete tuto redundanci odstranit pomocí dědičnosti.

### <a name="the-required-and-display-attributes"></a>Požadované a atributy zobrazení

Atributy `LastName` vlastnost zadejte je povinné pole, popisek pro textové pole by měl být "Last Name" (místo názvu vlastnosti, který by "LastName" bez mezer) a hodnota nemůže být delší než 50 znaků.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

[StringLength atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Nastaví maximální počet znaků v databázi a poskytuje na straně klienta a na straně serveru ověřování pro architekturu ASP.NET MVC. Minimální délka řetězce. můžete také zadat v tomto atributu, ale minimální hodnota nemá žádný vliv na schéma databáze. [Požadovaný atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) není potřeba u typů hodnot, jako je například datum a čas, int, double a float. Typy hodnot nelze přiřadit hodnotu null, takže jsou ze své podstaty vyžaduje. Můžete odebrat [požadovaný atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) a nahraďte ji metodou minimální délku parametru `StringLength` atribut:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

Více atributů můžete umístit na jednom řádku, proto by mohl taky zapisovat třídy kurzů vedených následujícím způsobem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>FullName počítá vlastností

`FullName` je počítaná vlastnost, která vrací hodnotu, která se vytváří zřetězením dvou dalších vlastností. Proto má pouze `get` přistupující objekt a ne `FullName` vygeneruje sloupec v databázi.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Kurzy a OfficeAssignment navigační vlastnosti

`Courses` a `OfficeAssignment` jsou navigační vlastnosti. Jak bylo vysvětleno dříve, se obvykle definují jako [virtuální](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) tak, aby můžete využít Entity Frameworku funkci s názvem [opožděné načtení](https://msdn.microsoft.com/magazine/hh205756.aspx). Kromě toho pokud navigační vlastnost může obsahovat více entit, jeho typ musí implementovat [rozhraní ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) rozhraní. (Například [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) kvalifikuje ale ne [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) protože `IEnumerable<T>` neimplementuje [přidat ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Instruktorem můžete naučit libovolný počet kurzů, takže `Courses` je definována jako kolekce `Course` entity. Naše obchodní pravidla stavu instruktorem může mít pouze nejvýše jeden office, takže `OfficeAssignment` je definován jako jeden `OfficeAssignment` entity (která může být `null` Pokud není přiřazena žádná office).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Vytvoření OfficeAssignment Entity

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Vytvoření *Models\OfficeAssignment.cs* následujícím kódem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Sestavení projektu, který uloží změny a ověřuje, že nebyla provedena jakékoli kopírování a vkládání chyb, které kompilátor může zachytit.

### <a name="the-key-attribute"></a>Klíčový atribut

Existuje jedna: nula nebo 1 vztah mezi `Instructor` a `OfficeAssignment` entity. Přiřazení kanceláře existuje pouze ve vztahu k instruktorem, je přiřazen, a proto jeho primární klíč se také jeho cizí klíč `Instructor` entity. Entity Framework nemůže rozpoznat automaticky, ale `InstructorID` jako primární klíče této entity, protože jeho název `ID` nebo *classname* `ID` zásady vytváření názvů. Proto `Key` atribut se používá k jeho identifikaci jako klíč:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Můžete také použít `Key` atribut Pokud entita nemá primární klíč, ale budete chtít název vlastnosti něco jiného než `classnameID` nebo `ID`. Ve výchozím nastavení EF považuje za klíč bez databáze vygenerovala protože sloupec je pro identifikující relaci.

### <a name="the-foreignkey-attribute"></a>Atribut cizí klíč

Když dojde k nule nebo jednom relaci nebo relaci mezi dvěma entitami (takové mezi `OfficeAssignment` a `Instructor`), které end je závislý a které konci vztahu se objekt zabezpečení nemůže pracovat EF. Relace 1: 1 mají vlastnost navigační odkaz v každé třídě na třídu. [Klíč ForeignKey atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) lze použít u závislé třídy a tím vytvoří vztah. Vynecháte-li [klíč ForeignKey atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), při pokusu o vytvoření migrace se zobrazí následující chyba:

Nepovedlo se určit hlavní konec asociace mezi typy "ContosoUniversity.Models.OfficeAssignment" a "ContosoUniversity.Models.Instructor". Hlavní konec toto přidružení explicitně nastavené pomocí rozhraní API fluent relace nebo datových poznámek.

Později v tomto kurzu vám ukážeme, jak nakonfigurovat tuto relaci s rozhraním API fluent.

### <a name="the-instructor-navigation-property"></a>Navigační vlastnost instruktorem

`Instructor` Entita má s povolenou hodnotou Null `OfficeAssignment` navigační vlastnosti (protože instruktorem nemusí mít přiřazení office) a `OfficeAssignment` entita má zakázanou `Instructor` navigační vlastnosti (protože nelze přiřazení office existovat bez instruktorem – `InstructorID` je null). Když `Instructor` entita má se souvisejícím `OfficeAssignment` entity, každá entita bude mít odkaz na druhou v jeho navigační vlastnost.

Můžete umístit `[Required]` atribut u vlastnosti navigace kurzů vedených k určení, že musí být související instruktorem, ale nemáte to provést, protože InstructorID cizí klíč (což je také klíč do této tabulky) je null.

## <a name="modify-the-course-entity"></a>Upravit Entity kurzu

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

V *Models\Course.cs*, nahraďte kód, který jste přidali dříve následujícím kódem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Kurz entita má vlastnost cizího klíče `DepartmentID` který odkazuje na související `Department` entity a má `Department` navigační vlastnost. Entity Framework nevyžaduje, můžete přidat vlastnost cizího klíče do datového modelu, když máte navigační vlastnost pro související entity. EF automaticky vytvoří cizí klíče v databázi bez ohledu na to se v případě potřeby zapíná. Ale s cizího klíče v datovém modelu může být aktualizace jednodušší a efektivnější. Například při načtení entity kurzu chcete upravit, `Department` entity má hodnotu null Pokud načtete není to, proto když aktualizujete entity kurzu budete mít nejdřív načíst `Department` entity. Když vlastnost cizího klíče `DepartmentID` je zahrnuta v datovém modelu, není nutné načíst `Department` entity před aktualizací.

### <a name="the-databasegenerated-attribute"></a>Atribut DatabaseGenerated

[DatabaseGenerated atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) s [žádný](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parametru u `CourseID` vlastnost určuje, že hodnoty primárního klíče jsou uživatelem zadané místo generován databází.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Ve výchozím nastavení Entity Framework předpokládá, že hodnoty primárního klíče je generován databází. Který se má ve většině scénářů. Ale pro `Course` entity, budete používat číslo uživatel zadal kurzu například řadu 1000 pro jedno oddělení, řadu 2000 pro jiného oddělení a tak dále.

### <a name="foreign-key-and-navigation-properties"></a>Cizí klíč a navigačních vlastností

Vlastnosti cizího klíče a vlastnosti navigace v `Course` entity zahrnují následující vztahy:

- Kurz je přiřazena jednoho oddělení, takže `DepartmentID` cizího klíče a `Department` navigační vlastnost z důvodů uvedených výše. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Kurz můžete mít libovolný počet studentů zaregistrované, takže `Enrollments` navigační vlastnost je kolekce: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Kurz může být vedená instruktorů více, proto `Instructors` navigační vlastnost je kolekce: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>Vytváří se entita oddělení

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Vytvoření *Models\Department.cs* následujícím kódem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Atribut sloupce

Dříve jste použili [atribut sloupce](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) Chcete-li změnit název mapování sloupců. V kódu `Department` entity, `Column` atribut se používá k změnit SQL mapování datového typu tak, aby sloupec bude definován pomocí serveru SQL Server [peníze](https://msdn.microsoft.com/library/ms179882.aspx) typ databáze:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Mapování sloupců se obecně nevyžaduje, protože rozhraní Entity Framework obvykle vybere odpovídající datový typ SQL serveru na základě typu CLR, které definujete pro vlastnost. Modul CLR `decimal` zadejte mapuje se na serveru SQL Server `decimal` typu. Ale v takovém případě budete vědět, že sloupec se drží peněžních hodnot a [peníze](https://msdn.microsoft.com/library/ms179882.aspx) datový typ je vhodnější k tomu.

### <a name="foreign-key-and-navigation-properties"></a>Cizí klíč a navigačních vlastností

Vlastnosti cizího klíče a navigace zahrnují následující vztahy:

- Oddělení může nebo nemusí mít správce a správce je vždy instruktorem. Proto `InstructorID` vlastnost je součástí jako cizí klíč `Instructor` přidá entity a otazník za `int` označení označit vlastnosti jako datový typ s možnou hodnotou Null typu. Navigační vlastnost jmenuje `Administrator` obsahuje, ale `Instructor` entity: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Oddělení může mít mnoho kurzů, takže `Courses` navigační vlastnost: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Podle konvence rozhraní Entity Framework umožňuje kaskádové odstranění pro Null cizí klíče a vztahy many-to-many. To může způsobit Cyklické kaskádové odstranění pravidla, která způsobí výjimku při spuštění kódu inicializátor. Například pokud definujete nebyla `Department.InstructorID` vlastnost jako s možnou hodnotou Null, získali byste k následující výjimce při spuštění inicializátoru: "referenční vztahu povede cyklického odkazu, který není povolen." V případě potřeby obchodní pravidla `InstructorID` vlastnost jako Null, je třeba použít následující rozhraní API fluent zakázat kaskádové odstranění v relaci: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>Úprava entit studenta

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

V *Models\Student.cs*, nahraďte kód, který jste přidali dříve následujícím kódem. Změny jsou zvýrazněné.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>Registrace Entity

 V *Models\Enrollment.cs*, nahraďte kód, který jste přidali dříve následujícím kódem

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>Cizí klíč a navigačních vlastností

Vlastnosti cizího klíče a navigačních vlastností zahrnují následující vztahy:

- Záznam registrace je jeden kurzům, tedy `CourseID` vlastnost cizího klíče a `Course` navigační vlastnost: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- Záznam registrace je pro jeden student, tedy `StudentID` vlastnost cizího klíče a `Student` navigační vlastnost: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>Relace m: N

Existuje vztah n: n mezi `Student` a `Course` entity a `Enrollment` entity funguje jako tabulka many-to-many spojení *s datovou částí* v databázi. To znamená, že `Enrollment` tabulka obsahuje další údaje kromě cizí klíče pro spojené tabulky (v tomto případě primární klíč a `Grade` vlastnost).

Následující obrázek znázorňuje, jak tyto vztahy vypadat v diagramu entity. (Tento diagram se vygeneroval pomocí [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); vytvoření diagramu, které nejsou součástí tohoto kurzu, je právě používán jako ilustraci zde.)

![Student Course_many k many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Každý řádek vztah má 1 na jednom konci a hvězdičku (\*) na druhém, určující vztah jeden mnoho.

Pokud `Enrollment` tabulky nezahrnuli na podnikové úrovni informace, třeba jenom tak, aby obsahovala dva cizího klíče `CourseID` a `StudentID`. V takovém případě by odpovídat na tabulku spojení many-to-many *bez datové části* (nebo *čistě vazební tabulka*) v databázi, a nebude nutné vytvořit třídu modelu, vůbec. `Instructor` a `Course` entity mají tento druh vztahu many-to-many, a jak je vidět, mezi nimi neexistuje žádná třída entity:

![Kurzů vedených Course_many k many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Vazební tabulka v databázi, ale vyžaduje, jak je znázorněno v následujícím diagramu databáze:

![Kurzů vedených Course_many k many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Entity Framework automaticky vytvoří `CourseInstructor` tabulku a číst a aktualizovat ho nepřímo pomocí čtení a aktualizace `Instructor.Courses` a `Course.Instructors` navigační vlastnosti.

## <a name="entity-diagram-showing-relationships"></a>Diagram znázorňující entitami

Následující obrázek znázorňuje diagram, který Entity Framework Power Tools vytvořit pro dokončené model školy.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Kromě many-to-many vztahu čáry (\* k \*) a vztah jednoho k několika řádky (1 \*), Zde uvidíte řádek jedna nula nebo 1 v relaci m (1-0..1) mezi `Instructor` a `OfficeAssignment` entity a relace nula nebo 1 n řádek (0.. 1 na \*) mezi entitami instruktorem a oddělení.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Přizpůsobte si datový Model přidáním kódu do kontextu databáze

Dále přidáte nové entity, které chcete `SchoolContext` třídy a přizpůsobit některé z mapování pomocí [rozhraní fluent API](https://msdn.microsoft.com/data/jj591617) volání. (Rozhraní API je "fluent", protože je často používána zavěšování řadu volání metody společně na jediném příkazu.)

V tomto kurzu použijete rozhraní fluent API pouze pro mapování databáze, které nelze použít s atributy. Rozhraní fluent API však můžete také použít k určení většinu formátování, ověřování a pravidla mapování, která vám pomůžou s použitím atributů. Některé atributy, jako `MinimumLength` nelze použít s rozhraním API fluent. Jak už bylo zmíněno dříve, `MinimumLength` nedojde ke změně schématu, vztahuje se pouze ověřovacího pravidla na straně klienta a serveru

Někteří vývojáři dávají přednost používání rozhraní fluent API výhradně tak, aby se zachovat jejich tříd entit "vyčištění." Pokud chcete, a existuje několik přizpůsobení, které lze provést pouze s použitím rozhraní fluent API je možné kombinovat atributy a dynamického rozhraní API, ale obecně doporučeným postupem je zvolte jednu z těchto dvou přístupů a použití, který konzistentně dosahovat.

Pokud chcete přidat nové entity data model a provést mapování databáze, které jste neprovedli pomocí atributů, nahraďte kód v *DAL\SchoolContext.cs* následujícím kódem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Nový příkaz v [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metoda nakonfiguruje many-to-many vazební tabulka:

- Pro many-to-many vztah mezi `Instructor` a `Course` entity, kód určuje názvy tabulek a sloupců pro tabulku spojení. Kód nejprve můžete nakonfigurovat vztah mnoho mnoho pro vás bez tohoto kódu, ale pokud není říkat, zobrazí se výchozí názvy, jako `InstructorInstructorID` pro `InstructorID` sloupce.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Následující kód znázorňuje, jak může použitých rozhraní fluent API namísto atributů k určení vztahu mezi `Instructor` a `OfficeAssignment` entity:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Informace o příkazech "rozhraní API fluent" činnosti na pozadí, najdete v článku [rozhraní Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) blogový příspěvek.

## <a name="seed-the-database-with-test-data"></a>Přidání dat do databáze s testovací Data

Nahraďte kód v *Migrations\Configuration.cs* souboru následujícím kódem, aby bylo možné poskytnout data počáteční hodnotu pro nové entity, které jste vytvořili.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Jak už jste viděli v první kurz, většina tento kód jednoduše aktualizuje nebo vytváří nové objekty entity a načte ukázková data do vlastnosti podle potřeby pro testování. Všimněte si však jak `Course` entity, která má vztah mnoho mnoho s `Instructor` entity, probíhá:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Když vytvoříte `Course` objektu, je inicializovat `Instructors` navigační vlastnost jako prázdnou kolekci pomocí kódu `Instructors = new List<Instructor>()`. Díky tomu je možné přidat `Instructor` entity, které se vztahují k tomuto `Course` pomocí `Instructors.Add` metody. Pokud jste nevytvořili je seznam prázdný, jste nemohli přidat tyto relace, protože `Instructors` vlastnost by mít hodnotu null a nebude mít `Add` metody. Inicializační seznam můžete také přidat do konstruktoru.

## <a name="add-a-migration-and-update-the-database"></a>Přidejte migraci a aktualizaci databáze

V konzole PMC, zadejte `add-migration` příkaz:

`PM> add-Migration Chap4`

Pokud se pokusíte aktualizovat databázi v tomto okamžiku, získáte následující chybu:

*Příkaz ALTER TABLE způsobil konflikt s omezení FOREIGN KEY "FK\_dbo. Kurz\_dbo. Oddělení\_DepartmentID ". Ke konfliktu došlo v databázi "ContosoUniversity" table "dbo. Oddělení", sloupec"DepartmentID".*

Upravit &lt; *časové razítko&gt;\_Chap4.cs* soubor a proveďte následující změny kódu (kurzu přidáte příkaz jazyka SQL a upravit `AddColumn` příkazu):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(Ujistěte se, že okomentovat nebo odstranit existující `AddColumn` řádek při přidání nového nebo budete dojde k chybě při zadání `update-database` příkazu.)

V případech, kdy můžete spustit migrace s existujícími daty, je třeba vložit data zástupných procedur do databáze splňovat omezení cizího klíče a, který je, co děláte nyní. Generovaný kód přidá neumožňující `DepartmentID` cizí klíč `Course` tabulky. Pokud jsou již řádků v `Course` tabulky při spuštění kódu `AddColumn` operace selže, protože systém SQL Server nebude vědět, jakou hodnotu put ve sloupci, který nemůže mít hodnotu null. Proto jste změnili kód a zadejte nový sloupec výchozí hodnotu a vytvoření zástupné procedury oddělení s názvem "Temp" tak, aby fungoval jako výchozí oddělení. Ve výsledku Pokud existují `Course` řádky, když tento kód se spustí, že se všechny souviset s oddělení "Temp".

Při `Seed` metoda pracuje, vloží řádků v `Department` tabulky a jejich vztah existující `Course` řádků, které mají tyto nové `Department` řádků. Pokud jste nepřidali žádné kurzy v uživatelském rozhraní, by potom už nepotřebujete oddělení "Temp" nebo výchozí hodnotu na `Course.DepartmentID` sloupce. Povolit možnost, že někdo může přidali kurzy s použitím aplikace, by také chcete aktualizovat `Seed` kódu metody zajistit, aby všechny `Course` řádky (nejen těm, které jsou vloženy pomocí předchozích spuštění `Seed` metoda) mají platný `DepartmentID` hodnoty před odebráním výchozí hodnotu ze sloupce a odstranit oddělení "Temp".

Po dokončení úprav &lt; *časové razítko&gt;\_Chap4.cs* soubor, zadejte `update-database` příkazu v konzole PMC k provedení migrace.

> [!NOTE]
> Je možné získat další chyby při migraci dat a provádění změn schématu. Pokud se zobrazí chyby při migraci nevyřešíte sami, můžete buď změnit připojovací řetězec *Web.config* souboru nebo odstraňte databázi. Nejjednodušším způsobem je přejmenovat databázi v *Web.config* souboru. Například změnit název databáze na kapacitní jednotka\_otestovat, jak je znázorněno v následujícím:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  S novou databázi, nejsou žádná data chcete migrovat a `update-database` příkaz je mnohem pravděpodobnější k dokončení bez chyb. Pokyny k odstranění databáze najdete v tématu [jak vyřadit databázi ze sady Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).


Otevřít databázi v **Průzkumníka serveru** stejně jako dříve a rozbalte **tabulky** uzel zobrazíte, že všechny tabulky byly vytvořeny. (Pokud stále máte **Průzkumníka serveru** otevřít z dřívější čas, klikněte na tlačítko **aktualizovat** tlačítko.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Jste nevytvořili třídy modelu pro `CourseInstructor` tabulky. Jak jsme vysvětlili výše, toto je tabulka spojení pro many-to-many vztah mezi `Instructor` a `Course` entity.

Klikněte pravým tlačítkem myši `CourseInstructor` tabulky a vyberte **zobrazit Data tabulky** ověřte, že má data v něm kvůli `Instructor` entity, které jste přidali do `Course.Instructors` navigační vlastnost.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>Souhrn

Teď máte složitějšího datového modelu a odpovídající databáze. V následujícím kurzu se dozvíte informace o různých způsobech pro přístup k související data.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k Data technologie ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [další](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
