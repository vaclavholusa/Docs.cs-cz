---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: "Vytváření složitějších datový Model pro aplikaci ASP.NET MVC | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9a89aa8e7dd3b2f6ac18e0b1a9c2a9d64d27189c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application"></a>Vytváření složitějších datový Model pro aplikaci ASP.NET MVC
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stáhnout PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio 2013. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


V předchozích kurzech pracovali s jednoduché datového modelu, který se skládá z tři entity. V tomto kurzu přidáte další entity a vztahy a datový model budete přizpůsobit zadáním formátování, ověřování a pravidla mapování databáze. Zobrazí dva způsoby, jak přizpůsobit datový model: přidáním atributů k třídy entitu a přidávání kódu do třídy kontextu databáze.

Jakmile budete hotovi, bude tříd entit tvoří dokončené datový model, který je znázorněno na následujícím obrázku:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Přizpůsobení datového modelu s použitím atributů

V této části se zobrazí postup přizpůsobení datového modelu s použitím atributy, které určují, formátování, ověření a pravidla mapování databáze. Potom v některé z těchto částí vytvoříte kompletní `School` datový model přidáním atributy třídám jste již vytvořené a vytváření nové třídy pro zbývající typy entit v modelu.

### <a name="the-datatype-attribute"></a>Datový typ atributu

Pro studenty registrace kalendářních dat všech webových stránek aktuálně zobrazuje čas, spolu s datem, i když všechny, které se zajímáte o pro toto pole je datum. Pomocí datové poznámky atributy, můžete si ho code změny, která bude opravte formát zobrazení v každé zobrazení, která zobrazuje data. Chcete-li zobrazit příklad, jak to provést, že přidáte do atribut `EnrollmentDate` vlastnost v `Student` třídy.

V *Models\Student.cs*, přidejte `using` příkaz pro `System.ComponentModel.DataAnnotations` obor názvů a přidejte `DataType` a `DisplayFormat` atributů k `EnrollmentDate` vlastnost, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

[Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atribut slouží k určení datový typ, který je specifičtější než vnitřní typ databáze. V tomto případě chceme jenom udržování přehledu o datum, není datum a čas. [Datový typ výčtu](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) poskytuje pro mnoho typů dat, jako například *datum, čas, telefonní číslo, měny, EmailAddress* a další. `DataType` Atributu můžete také povolit aplikace automaticky poskytnout konkrétní typ funkce. Například `mailto:` může vytvořit odkaz pro [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), a datum selektor lze zadat pro [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) v prohlížečích podporujících [HTML5](http://html5.org/). [Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributy vysílá standardu HTML 5 [data -](http://ejohn.org/blog/html-5-data-attributes/) (vyslovováno *data dash*) atributy, které můžete porozumět standardu HTML 5 prohlížeče. [Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributy neposkytují žádné ověření.

`DataType.Date`neurčuje formát data, které se zobrazí. Ve výchozím nastavení, zobrazí se pole dat podle výchozích formátů podle serveru [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

`DisplayFormat` Atribut slouží k explicitnímu zadání formát data:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode` Nastavení určuje, že zadané formátování také bude použito při hodnota je zobrazena v textovém poli pro úpravy. (Který není vhodné pro některá pole – například pro hodnoty měny, nemusí chcete symbolu měny do textového pole pro úpravy.)

Můžete použít [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut podle sám sebe, ale obecně je vhodné používat [datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) také atribut. `DataType` Přenese tak atribut *sémantiku* dat, jako byl proti na tom, jak vykreslit ho na obrazovce a nabízí následující výhody, které není dostupná s `DisplayFormat`:

- V prohlížeči můžete povolit funkce HTML5 (například k zobrazení ovládacího prvku kalendář, symbolu měny vhodné národního prostředí, e-mailu odkazů některé klientské zadejte ověření atd.).
- Ve výchozím nastavení, prohlížeč bude vykreslovat data správný formát na základě vaší [národního prostředí](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributu můžete povolit MVC na výběr šablony pravé pole k poskytnutí dat ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) používá šablonu řetězec). Další informace najdete v tématu Brad Wilson [ASP.NET MVC 2 šablony](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (I když napsané pro MVC 2, tento článek stále platí pro aktuální verzi ASP.NET MVC.)

Pokud použijete `DataType` atribut s poli data, budete muset určit `DisplayFormat` atribut také, aby se zajistilo, že pole správně vykreslení v prohlížeči Chrome. Další informace najdete v tématu [tohoto podprocesu StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Další informace o tom, jak zpracovat další formáty data v MVC, přejděte na [MVC 5 Úvod: prozkoumání upravit metody a upravit zobrazení](../introduction/examining-the-edit-methods-and-edit-view.md) a vyhledávání na stránce pro &quot;internacionalizace&quot;.

Znovu spustit Student indexovou stránku a Všimněte si, že časy se už nezobrazují mezi daty registrace. Stejné bude platit pro všechna zobrazení, která používá `Student` modelu.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

Můžete také zadat pravidla ověření dat a chybové zprávy ověření pomocí atributů. [StringLength atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Nastaví maximální délku v databázi a poskytuje na straně klienta a na straně serveru ověřování pro architekturu ASP.NET MVC. Minimální délka řetězce. můžete také zadat v tomto atributu, ale minimální hodnota nemá žádný vliv na schéma databáze.

Předpokládejme, že chcete zajistit, že uživatelé nezadávejte víc než 50 znaků pro název. Chcete-li přidat toto omezení, přidejte [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atributů k `LastName` a `FirstMidName` vlastnosti, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atribut nebude uživatel zabránit v přechodu do prázdných znaků pro název. Můžete použít [regulární výraz](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atribut použít omezení na vstup. Například následující kód vyžaduje první znak, který má být velkými písmeny a zbývající znaků, které mají být abecední:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) atribut poskytuje podobné funkce jako [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atribut, ale neposkytuje na straně klienta ověření.

Spusťte aplikaci a klikněte na tlačítko **studenty** kartě. Dojde k následující chybě:

*Model zálohování kontext 'SchoolContext' se od vytvoření databáze změnil. Zvažte použití migrace Code First k aktualizaci databáze ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Model databáze byl změněn způsobem, který vyžaduje změnu schématu databáze a Entity Framework zjistil, že. Migrace budete používat k aktualizaci schématu bez ztráty dat, který jste přidali do databáze pomocí uživatelského rozhraní. Pokud jste změnili data, která byla vytvořená `Seed` metoda, která bude změněna zpět do původního stavu z důvodu [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) metoda, která používáte v `Seed` metoda. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) je ekvivalentní operace "upsert", z databáze terminologie.)

V balíček správce konzoly (pomocí PMC), zadejte následující příkazy:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration` Příkaz vytvoří soubor s názvem  *&lt;časové razítko&gt;\_MaxLengthOnNames.cs*. Tento soubor obsahuje kód `Up` metoda, která aktualizuje databázi tak, aby odpovídala aktuální datový model. `update-database` Tento kód byl spuštěn příkaz.

Časové razítko před název souboru migrace je pomocí rozhraní Entity Framework sloužící k uspořádání byla migrace. Můžete vytvořit více migrací dřív, než spustíte `update-database` příkaz a potom všechny byla migrace se použijí v pořadí, ve kterém byly vytvořeny.

Spustit **vytvořit** stránky a potom zadejte buď název delší než 50 znaků. Když kliknete na tlačítko **vytvořit**, ověřování na straně klienta zobrazí chybovou zprávu.

![val chyba na straně klienta](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>Atribut sloupce

Atributy můžete taky řídit, jak jsou mapovány třídy a vlastnosti do databáze. Předpokládejme, že při použití názvu `FirstMidName` pro první název pole, protože pole může obsahovat také křestní jméno. Ale chcete sloupci databáze s názvem `FirstName`, protože jsou uživatelé, kteří budou být zápis dotazů ad-hoc v databázi zvykli tohoto názvu. Chcete-li toto mapování, můžete použít `Column` atribut.

`Column` Atribut určuje, že při vytvoření databáze, sloupec `Student` tabulku, která se mapuje `FirstMidName` vlastnost bude mít název `FirstName`. Jinými slovy, pokud váš kód odkazuje na `Student.FirstMidName`, data budou pocházet z nebo aktualizovány v `FirstName` sloupec `Student` tabulky. Pokud nezadáte názvy sloupců, jsou uvedené stejný název jako název vlastnosti.

V *Student.cs* soubor, přidejte `using` příkaz pro [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) a přidejte atribut název sloupce, který se `FirstMidName` vlastnost, jak je znázorněno následující zvýrazněný kód:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Přidání [atribut sloupce](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) změní model zálohování SchoolContext, takže nebude odpovídat databázi. Zadejte následující příkazy v pomocí PMC vytvořit další migraci:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

V **Průzkumníka serveru**, otevřete *Student* návrháře tabulky dvojitým kliknutím *Student* tabulky.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

Následující obrázek znázorňuje původní název sloupce jako před použít první dvě migrace. Kromě názvu sloupce změna z `FirstMidName` k `FirstName`, dvou sloupců název byly změněny z `MAX` délce až 50 znaků.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Můžete také provést databáze mapování změny pomocí [rozhraní Fluent API](https://msdn.microsoft.com/data/jj591617), jak uvidíte později v tomto kurzu.

> [!NOTE]
> Pokud se pokusíte zkompilovat před dokončení vytváření všechny třídy entity v následujících částech, může docházet k chybám kompilátoru.


## <a name="complete-changes-to-the-student-entity"></a>Dokončení změny Student Entity

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

V *Models\Student.cs*, nahraďte kód, který jste přidali dříve následujícím kódem. Změny se zvýrazněnou.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>Požadovaný atribut

[Požadovaný atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) díky povinná pole název vlastnosti. `Required attribute` Není u typů hodnot, jako je například datum a čas, int, double, třeba a float. Typy hodnot nelze přiřadit hodnotu null, tak ze své podstaty jsou považovány za povinná pole. Je vhodné odebrat [požadovaný atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) a nahraďte parametr minimální délku `StringLength` atribut:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

### <a name="the-display-attribute"></a>Atribut zobrazení

`Display` Atribut určuje, že titulek pro do textových polí by měl být "Jméno", "Příjmení", "Úplný název" a "Registrace datum" namísto název vlastnosti v každé instanci (což je žádné místo dělení slova).

### <a name="the-fullname-calculated-property"></a>Položka FullName vypočítat vlastnost

`FullName`je počítané vlastnosti, která vrátí hodnotu, která se vytvoří zřetězením dva další vlastnosti. Proto má jen `get` přistupujícího objektu a ne `FullName` vygeneruje sloupec v databázi.

## <a name="create-the-instructor-entity"></a>Vytvořit entitu lektorem

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Vytvoření *Models\Instructor.cs*, nahraďte kód šablony s následujícím kódem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Všimněte si, že několik vlastností, které jsou stejné ve `Student` a `Instructor` entity. V [implementace dědičnosti](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) kurz později z této série budete Refaktorovat tento kód eliminovat redundance.

Více atributů můžete umístit na jeden řádek, můžete také napsat třídu lektorem následujícím způsobem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Kurzy a OfficeAssignment navigační vlastnosti

`Courses` a `OfficeAssignment` vlastnosti jsou navigační vlastnosti. Jak bylo popsáno dříve, jsou obvykle definovány jako [virtuální](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) tak, aby můžete využít rozhraní Entity Framework funkci [opožděného načítání](https://msdn.microsoft.com/magazine/hh205756.aspx). Kromě toho, pokud navigační vlastnost může obsahovat více entit, její typ musí implementovat [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) rozhraní. Například [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) vyfiltrování ale není [rozhraní IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) protože `IEnumerable<T>` neimplementuje [přidat ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Lektorem můžete naučit libovolný počet kurzy, takže `Courses` je definována jako kolekce `Course` entity.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Naše obchodní pravidla o stavu lektorem může mít pouze maximálně jeden office, takže `OfficeAssignment` je definován jako jeden `OfficeAssignment` entity (která může být `null` Pokud není přiřazena žádná office).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Vytvořit entitu OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Vytvoření *Models\OfficeAssignment.cs* následujícím kódem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Sestavení projektu, který uloží změny a ověřuje, že nebyly provedeny žádné kopírování a vkládání chyb, které můžete kompilátor catch.

### <a name="the-key-attribute"></a>Klíčový atribut

Je--nula nebo 1 vztah mezi `Instructor` a `OfficeAssignment` entity. Přiřazení office existuje pouze ve vztahu k lektorem je přiřazena k, a proto jeho primární klíč je také jeho cizí klíč `Instructor` entity. Rozhraní Entity Framework nelze rozpoznat automaticky, ale `InstructorID` jako primární klíče této entity, protože jeho název `ID` nebo *classname* `ID` zásady vytváření názvů. Proto `Key` atribut slouží k identifikaci jako klíč:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Můžete také `Key` atribut Pokud entity mít svůj vlastní primární klíč, ale chcete vlastnost název něco jiného než `classnameID` nebo `ID`. Ve výchozím nastavení EF klíče jsou považovány za jiný databáze generované protože sloupec je sloupec pro identifikační vztah.

### <a name="the-foreignkey-attribute"></a>Atribut cizí klíč

Když je vztah jeden pro žádná nebo jedna nebo relaci mezi dvěma entitami (takové jako mezi `OfficeAssignment` a `Instructor`), EF nemůže pracovat a které konci relace se objekt end, které je závislá. Relace 1: 1 mají vlastnost navigační odkaz v každé třídě k jiné třídě. [ForeignKey atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) závislé třídy k navázání vztahu můžete použít. V případě vynechání [ForeignKey atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), při pokusu o vytvoření migrace se zobrazí následující chyba:

*Nelze určit hlavní konec přidružení mezi typy 'ContosoUniversity.Models.OfficeAssignment' a 'ContosoUniversity.Models.Instructor'. Hlavní konec tohoto přidružení musí být explicitně nakonfigurovaný buď pomocí rozhraní fluent API relace nebo pomocí datových poznámek.*

Později v tomto kurzu uvidíte postup konfigurace tento vztah se rozhraní fluent API.

### <a name="the-instructor-navigation-property"></a>Vlastnost navigace lektorem

`Instructor` Entita, která má s možnou hodnotou Null `OfficeAssignment` navigační vlastnost (protože lektorem nemusí mít přiřazení office) a `OfficeAssignment` entita má hodnotou Null `Instructor` navigační vlastnost (protože nelze přiřazení office Existují bez lektorem – `InstructorID` neumožňuje hodnotu Null). Když `Instructor` entita má související `OfficeAssignment` entity, každá entita bude mít odkaz na další jeden v její navigační vlastnosti.

By mohlo `[Required]` atribut na navigační vlastnost lektorem k určení, že musí být související lektorem, ale nemáte k tomu, protože InstructorID cizí klíč (což je také klíč, který chcete tuto tabulku) je použití hodnot Null.

## <a name="modify-the-course-entity"></a>Upravit kurzu Entity

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

V *Models\Course.cs*, nahraďte kód, který jste přidali dříve následujícím kódem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Během entita má vlastností cizího klíče `DepartmentID` který odkazuje na související `Department` entity a má `Department` navigační vlastnost. Rozhraní Entity Framework nevyžaduje, můžete přidat vlastností cizího klíče do datového modelu, když máte navigační vlastnost pro související entity. EF automaticky vytvoří cizí klíče v databázi, kde jsou potřeba. Ale s cizí klíč v datovém modelu můžete provést aktualizace teď jednodušší a efektivnější. Například při načtení entity kurzu chcete upravit, `Department` entity má hodnotu null. Pokud nemáte načíst ho, tak při aktualizaci entity kurzu, budete muset nejdřív načíst `Department` entity. Když vlastností cizího klíče `DepartmentID` je zahrnutá v datovém modelu, není nutné načíst `Department` entity před aktualizací.

### <a name="the-databasegenerated-attribute"></a>Atribut DatabaseGenerated

[DatabaseGenerated atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) s [žádné](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parametr na `CourseID` vlastnost určuje, že hodnot primárního klíče jsou zadané uživatelem, nikoli generované databáze.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Ve výchozím nastavení rozhraní Entity Framework předpokládá, že hodnot primárního klíče generované databáze. To je, co chcete ve většině scénářů. Ale pro `Course` entity, budete používat několik zadán uživatel během například 1000 řady pro jeden oddělení, 2000 řady pro jiné oddělení a tak dále.

### <a name="foreign-key-and-navigation-properties"></a>Cizí klíč a navigační vlastnosti

Vlastnosti cizího klíče a navigačních vlastností v `Course` entity podle následující relace:

- Kurz je přiřazena k jedné oddělení, takže není `DepartmentID` cizí klíč a `Department` navigační vlastnost z důvodů uvedených výše. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Kurz může mít libovolný počet studenty zaregistrované v něm proto `Enrollments` navigační vlastnost je kolekce: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Kurz může výukové ve více vyučující, proto `Instructors` navigační vlastnost je kolekce: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Vytvořit entitu oddělení

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Vytvoření *Models\Department.cs* následujícím kódem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Atribut sloupce

Dříve jste použili [atribut sloupce](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) změnit mapování název sloupce. V kódu pro `Department` entity, `Column` ke změnit SQL mapování datového typu, aby sloupec bude nutné definovat pomocí systému SQL Server používá atribut [peníze](https://msdn.microsoft.com/library/ms179882.aspx) typ v databázi:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Mapování sloupce není povinné, protože rozhraní Entity Framework obvykle vybere odpovídající typ dat systému SQL Server na základě typu CLR, které definujete pro vlastnost. Modul CLR `decimal` zadejte map k systému SQL Server `decimal` typu. Ale v takovém případě víte, že sloupec bude stisknuta částky měny a [peníze](https://msdn.microsoft.com/library/ms179882.aspx) datový typ je vhodnější, pro který. Další informace o datových typech CLR a jak budou odpovídat na datové typy systému SQL Server najdete v tématu [SqlClient pro Entity FrameworkTypes](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Cizí klíč a navigační vlastnosti

Vlastnosti cizího klíče a navigační odráží následující relace:

- Oddělení může nebo nemusí mít správce a správce je vždy lektorem. Proto `InstructorID` vlastnost je zahrnuta jako cizí klíč k `Instructor` po přidání entity a otazník `int` zadejte označení označit vlastnost jako s možnou hodnotou Null. Navigační vlastnost jmenuje `Administrator` , ale obsahuje `Instructor` entity: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Oddělení může mít mnoho kurzy, takže není `Courses` navigační vlastnost: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

 > [!NOTE]
 > Podle konvence rozhraní Entity Framework umožňuje kaskádové odstranění pro použití hodnot Null cizí klíče a pro relace m: n. Výsledkem může být Cyklické kaskádové odstranění pravidla, která způsobí výjimku při pokusu o přidání migrace. Například, pokud neuvedli `Department.InstructorID` vlastnost jako s možnou hodnotou Null, by získat zpráva o výjimce: "referenční vztah způsobí cyklické odkaz, který není povolen." V případě potřeby obchodní pravidla `InstructorID` vlastnost, která má mít hodnotu Null, je třeba použít následující příkaz rozhraní API fluent zakázat kaskádové odstranění v relaci: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modify-the-enrollment-entity"></a>Upravit registraci Entity

![Enrollment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

 V *Models\Enrollment.cs*, nahraďte kód, který jste přidali dříve následujícím kódem

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Cizí klíč a navigační vlastnosti

Vlastnosti cizího klíče a navigačních vlastností odráží následující relace:

- Záznam zápisu je jeden kurzu, takže není `CourseID` vlastností cizího klíče a `Course` navigační vlastnost: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Záznam zápisu je pro jeden student, takže není `StudentID` vlastností cizího klíče a `Student` navigační vlastnost: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Relace m: N

Je vztah m: n mezi `Student` a `Course` entity a `Enrollment` entity funguje jako tabulku spojení m: n *s odebranou datovou částí* v databázi. To znamená, že `Enrollment` tabulka obsahuje další údaje kromě cizí klíče pro spojené tabulky (v tomto případě primární klíč a `Grade` vlastnosti).

Následující obrázek znázorňuje, jak tyto relace vypadat v diagramu entity. (Tento diagram byla generována pomocí [výkonné nástroje Entity Framework](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)vytváření diagramu není součástí tohoto kurzu, je právě používán sem jako obrázek.)

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Každý řádek vztahů je 1 na jeden element end a znak hvězdičky (\*) jiných, která určuje vztah jeden mnoho.

Pokud `Enrollment` tabulky nezahrnuli úrovni informace, jenom třeba, aby obsahovat dvě cizí klíče `CourseID` a `StudentID`. V takovém případě by odpovídat na tabulku spojení m: n *bez datové části* (nebo *čistý spojení tabulky*) v databázi, a máte nebude vůbec vytvářet třídu modelu pro ni. `Instructor` a `Course` entity mají tento druh relace m: n, a jak můžete vidět, neexistuje žádná třída entity mezi nimi:

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Připojení k tabulku je nutné v databázi, ale, jak je znázorněno v následujícím diagramu databáze:

![Instructor-Course_many-to-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Rozhraní Entity Framework vytvoří automaticky `CourseInstructor` tabulky a můžete číst a aktualizovat ji nepřímo čtení a aktualizaci `Instructor.Courses` a `Course.Instructors` navigační vlastnosti.

## <a name="entity-diagram-showing-relationships"></a>Diagram znázorňující entitami

Následující obrázek znázorňuje diagram, který vytvořit výkonné nástroje Entity Framework pro dokončené školní model.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

Kromě relace m: n řádků (\* k \*) a řádky, vztah jeden mnoho (1 \*), Zde uvidíte řádek vztah jeden pro žádná nebo jedna (1-0..1) mezi `Instructor` a `OfficeAssignment` entity a řádku relace nula nebo 1 n (0..1 k \*) mezi entitami lektorem a oddělení.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Přidání kódu do kontextu databáze přizpůsobit datový Model

Dále přidáte nové entity k `SchoolContext` třídy a přizpůsobit některé mapování pomocí [rozhraní fluent API](https://msdn.microsoft.com/data/jj591617) volání. Rozhraní API je "fluent", protože se často používají v rozvádět řadu volání metod společně do jednoho příkazu, jako v následujícím příkladu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

V tomto kurzu použijete rozhraní fluent API jenom pro mapování databáze, které nemůžete dělat s atributy. Rozhraní fluent API však můžete také použít k určení většinu formátování, ověření a pravidla mapování, které můžete provést pomocí atributů. Některé atributy, jako `MinimumLength` nelze použít s rozhraní fluent API. Jak je uvedeno nahoře, `MinimumLength` nemění schématu, vztahuje se pouze klientské a serverové straně ověřovacího pravidla

Někteří vývojáři dávají přednost používání rozhraní fluent API výhradně tak, aby se zachovat jejich tříd entit "čistou." Pokud chcete, a neexistují několik úprav, které lze provést pouze pomocí rozhraní fluent API je možné kombinovat atributy a rozhraní fluent API, ale obecně doporučený postup je vyberte jednu z těchto dvou přístupů a používání který konzistentně co nejvíc.

Pokud chcete přidat nové entity dat modelu a proveďte mapování databáze, která nebyla provedete pomocí atributů, nahraďte kód v *DAL\SchoolContext.cs* následujícím kódem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Nové prohlášení v [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metoda nakonfiguruje v tabulce m: n připojení:

- Pro relaci n: n mezi `Instructor` a `Course` entity, kód určuje názvy tabulek a sloupců pro tabulku spojení. Kód nejprve můžete nakonfigurovat relace m: n pro vás bez tento kód, ale pokud nemáte volat ji, zobrazí se výchozí názvy, jako `InstructorInstructorID` pro `InstructorID` sloupce.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Následující kód představuje příklad, jak vám může mít používána rozhraní fluent API místo atributy Pokud chcete určit vztah mezi `Instructor` a `OfficeAssignment` entity:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Informace o co "rozhraní fluent API" příkazy dělají na pozadí, najdete v článku [rozhraní Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) příspěvku na blogu.

## <a name="seed-the-database-with-test-data"></a>Naplnit databázi daty testu

Nahraďte kód v *Migrations\Configuration.cs* souboru následujícím kódem, chcete-li poskytovat počáteční hodnoty dat pro nové entity, které jste vytvořili.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Jak už jste viděli v první kurz, většina tento kód jednoduše aktualizuje nebo vytvoří nové entity objekty a načte ukázková data do vlastnosti podle potřeby pro testování. Všimněte si však, jak `Course` entity, který je v relaci m: n s `Instructor` entity, se zpracovává:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Při vytváření `Course` objektu inicializaci `Instructors` navigační vlastnost jako prázdnou kolekci pomocí kódu `Instructors = new List<Instructor>()`. Díky tomu je možné přidat `Instructor` entit, které se vztahují k tomuto `Course` pomocí `Instructors.Add` metoda. Pokud jste nevytvořili prázdný seznam, nepůjdou přidat tyto relace, protože `Instructors` vlastnost by mít hodnotu null a nebude mít `Add` metoda. Můžete také přidat seznamu inicializace konstruktoru.

## <a name="add-a-migration-and-update-the-database"></a>Přidejte migrace a aktualizaci databáze

Z pomocí PMC, zadejte `add-migration` příkazu (nedělají nic `update-database` příkaz ještě):

`add-Migration ComplexDataModel`

Pokud jste se pokusili spustit `update-database` příkaz v tomto okamžiku (není to je ještě), se zobrazí následující chyba:

*Příkaz ALTER TABLE konflikt s omezení FOREIGN KEY "Cizíklíč\_dbo. Během\_dbo. Oddělení\_DepartmentID ". Ke konfliktu došlo v databázi "ContosoUniversity" tabulka "dbo. Oddělení", sloupec 'DepartmentID'.*

V případech, kdy můžete spustit migrace s existujícími daty, je třeba vložit se zakázaným inzerováním data do databáze, aby pokryl omezení cizích klíčů, a který bude co musíte udělat teď. Generovaný kód v ComplexDataModel `Up` metoda přidá hodnotou Null `DepartmentID` cizí klíče `Course` tabulky. Protože jsou již řádků v `Course` tabulky při spuštění kódu `AddColumn` operace selže, protože systém SQL Server není známo, jaké hodnoty Pokud chcete umístit do sloupce, který nemůže mít hodnotu null. Proto musí změnit kód poskytnout nového sloupce výchozí hodnotu a vytvořit se zakázaným inzerováním oddělení s názvem "Temp" tak, aby fungoval jako výchozí oddělení. V důsledku toho existující `Course` řádky budou všechny připojeny ke oddělení "Temp" po `Up` metoda spustí. Propojovat je na správné oddělení v `Seed` metoda.

Upravit &lt; *časové razítko&gt;\_ComplexDataModel.cs* souboru, Odkomentujte řádek kódu, který přidává DepartmentID sloupce do tabulky kurzu a přidejte následující zvýrazněný kód (komentáři řádek je také označený):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Při `Seed` metoda spustí vloží řádky v `Department` tabulky a se týkají existující `Course` řádků, které mají tyto nové `Department` řádků. Pokud jste nepřidali žádné kurzy v uživatelském rozhraní, pak už potřebovali byste oddělení "Temp" nebo výchozí hodnotu na `Course.DepartmentID` sloupce. Povolit možnost, že někdo pravděpodobně přidána kurzy pomocí aplikace, by také chcete aktualizovat `Seed` metoda kód zajistit, aby všechny `Course` řádků (nikoli pouze ty vložená starší postupnými `Seed` metoda) mají platný `DepartmentID` hodnoty před odebráním výchozí hodnoty ze sloupce a odstranit oddělení "Temp".

Po dokončení úprav &lt; *časové razítko&gt;\_ComplexDataModel.cs* souboru, zadejte `update-database` v pomocí PMC k provedení migrace.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> Je možné získat další chyby při migraci dat a provedení změny schématu. Pokud dojde k chybám migrace, které nelze vyřešit, můžete změnit název databáze v připojovacím řetězci nebo odstraňte tuto databázi. Nejjednodušší způsob je přejmenovat databázi v *Web.config* souboru. Následující příklad ukazuje název změnit tak, aby CU\_testu:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
> 
> S novou databázi, nejsou k dispozici žádná data chcete migrovat a `update-database` příkaz je mnohem pravděpodobnější dokončeno bez chyb. Pokyny k odstranění databáze najdete v tématu [postup odstranění databáze ze sady Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
> 
> Pokud to nepomůže, je dalším krokem, který můžete vyzkoušet, znovu inicializovat databázi tak, že zadáte následující příkaz pomocí PMC:
> 
> `update-database -TargetMigration:0`


Otevřít databázi v **Průzkumníka serveru** stejně jako dříve a rozbalte **tabulky** uzel pro zobrazení, že všechny tabulky byly vytvořeny. (Pokud máte **Průzkumníka serveru** otevřete z dřívější čas, klikněte na **aktualizovat** tlačítko.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Jste nevytvořili třídu modelu pro `CourseInstructor` tabulky. Jak je popsáno výše, to je tabulka spojení pro vztah m: n mezi `Instructor` a `Course` entity.

Klikněte pravým tlačítkem myši `CourseInstructor` tabulky a vyberte **zobrazit Data tabulky** ověřte, že má data v ní na základě těchto `Instructor` entity, které jste přidali do `Course.Instructors` navigační vlastnost.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="summary"></a>Souhrn

Nyní máte složitější datový model a příslušné databáze. V následujícím kurzu se dozvíte informace o různých způsobech přístup související data.

Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit. Můžete také vyžádat nová témata na [zobrazit mi jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Odkazy na další zdroje Entity Framework najdete v [přístup k datům ASP.NET - doporučené prostředky](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Předchozí](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[další](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
