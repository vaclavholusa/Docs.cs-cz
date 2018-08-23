---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementace dědičnosti s rozhraním Entity Framework v aplikaci ASP.NET MVC (8 10) | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje postup vytvoření aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a sady Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 10ee0be62a1d601e323afc423e9022bed56f4f33
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756638"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementace dědičnosti s rozhraním Entity Framework v aplikaci ASP.NET MVC (8 10)
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a Visual Studio 2012. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Série kurzů můžete začít od začátku nebo [stáhnout počáteční projekt pro tuto kapitolu](building-the-ef5-mvc4-chapter-downloads.md) a začněte tady.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte sami, [stáhnout dokončený kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a zkuste problém reprodukovat. Porovnáním kód Dokončený kód v obecně najdete řešení problému. Některé běžné chyby a jejich řešení najdete v tématu [chyby a náhradní řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V předchozím kurzu zpracování výjimky souběžnosti. Tento kurzu se dozvíte, jak implementovat dědičnosti v datovém modelu.

V objektově orientované programování, můžete dědičnost vyloučit redundantní kód. V tomto kurzu změníte `Instructor` a `Student` tak, že jsou odvozeny z třídy `Person` základní třída, která obsahuje vlastnosti, například `LastName` , které jsou společné pro vyučující a studenty. Nebude přidat nebo změnit libovolné webové stránky, ale změníte kód, a tyto změny se automaticky projeví v databázi.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabulky na hierarchii a dědičnosti na typ tabulky

V objektově orientované programování, můžete dědičnost usnadňují práci s souvisejícími třídami. Například `Instructor` a `Student` tříd v `School` datový model sdílet několik vlastností, výsledek bude redundantní kód:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Předpokládejme, že chcete vyloučit redundantní kód pro vlastnosti, které jsou sdíleny `Instructor` a `Student` entity. Můžete vytvořit `Person` základní třída, která obsahuje pouze ty sdílené vlastnosti a pak proveďte `Instructor` a `Student` entit dědí ze základní třídy, jak je znázorněno na následujícím obrázku:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Existuje několik způsobů, které tato struktura dědičnosti můžou být reprezentované v databázi. Mohli byste mít `Person` tabulku, která obsahuje informace o studenty a vyučující v jediné tabulce. Některé sloupce, které může používat jenom u Instruktoři (`HireDate`), některé jenom pro studenty (`EnrollmentDate`), některé obě (`LastName`, `FirstName`). Obvykle byste měli *diskriminátoru* sloupec, aby označoval, jaký typ každý řádek představuje. Pro sloupec diskriminátoru může mít například "Kurzů vedených" pro vyučující a "Studentů" pro studenty.

![Tabulka za hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Tento model generování struktury dědičnosti entity z tabulky izolovanou databázi nazvali *na hierarchii tabulky* dědičnost (TPH).

Další možností je databázi tak, aby vypadat více jako struktury dědičnosti. Například může mít pouze název pole `Person` tabulky a mít samostatné `Instructor` a `Student` tabulky s poli datum.

![Tabulka za type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Tento model vytváření tabulky databáze pro každou třídu entity se nazývá *tabulky podle typu* dědičnosti (TPT).

Vzory dědičnosti TPH obecně doručit lepší výkon v Entity Framework než vzory TPT dědičnosti, protože TPT vzory může vést k dotazům komplexní spojení. Tento kurz ukazuje, jak implementovat TPH dědičnosti. Můžete to udělat tak, že provedete následující kroky:

- Vytvoření `Person` třídy a změňte `Instructor` a `Student` třídy, které jsou odvozeny z `Person`.
- Přidejte databázi model mapování kód do třídy kontextu databáze.
- Změna `InstructorID` a `StudentID` odkazy v celém projektu `PersonID`.

## <a name="creating-the-person-class"></a>Vytvoření třídy osoby

 Poznámka: Nebude možné ke kompilaci projektu po vytvoření třídy níže, dokud neaktualizujete řadičích, které používá tyto třídy. 

V *modely* složku, vytvořte *Person.cs* a nahraďte kód šablony následujícím kódem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

V *Instructor.cs*, odvozovat `Instructor` třídy z `Person` třídy a odstraňte klíč a název pole. Kód bude vypadat jako v následujícím příkladu:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Provádět podobné změny *Student.cs*. `Student` Třídy bude vypadat jako v následujícím příkladu:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Přidání typu Person Entity do modelu

V *SchoolContext.cs*, přidejte `DbSet` vlastnost `Person` typ entity:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

To je vše, Entity Framework potřebuje, aby konfigurace tabulky na hierarchii dědičnosti. Jak uvidíte, kdy databáze nebude znovu vytvořena, bude mít `Person` tabulky místo `Student` a `Instructor` tabulky.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Změna InstructorID a StudentID PersonID

V *SchoolContext.cs*, v příkazu mapování kurzů vedených kurzu změňte `MapRightKey("InstructorID")` k `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Tato změna není vyžadována; pouze změní název InstructorID sloupce v tabulce many-to-many spojení. Pokud jste nechali název jako InstructorID, aplikace bude i nadále fungovat správně. Tady je dokončené *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Dál je potřeba změnit `InstructorID` k `PersonID` a `StudentID` k `PersonID` v celém projektu ***s výjimkou*** v souborech migrace časovým razítkem v *migrace* složka. K tomu budete najít a otevřít pouze soubory, které je potřeba změnit a potom proveďte globálních změn na otevřené soubory. Jediným souborem v *migrace* je složka, měli byste změnit *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Začněte tím, že zavření všech otevřených souborů v sadě Visual Studio.
2. Klikněte na tlačítko **najít a nahradit – vyhledat všechny soubory** v **upravit** nabídky a vyhledejte všechny soubory v projektu, které obsahují `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Otevřete každý soubor v **výsledky hledání** okno ***s výjimkou*** &lt;časové razítko&gt;*\_.cs* migrace souborů v *Migrace* složky dvojitým kliknutím na jeden řádek pro každý soubor.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Otevřít **nahradit v souborech** dialogové okno a změnit **Hledat v** k **všechny otevřené dokumenty**.
5. Použití **nahradit v souborech** dialogové okno změnit všechny `InstructorID` do `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Vyhledat všechny soubory v projektu, který obsahují `StudentID`.
7. Otevřete každý soubor v **výsledky hledání** okno ***s výjimkou*** &lt;časové razítko&gt;*\_\*.cs* migrace souborů v *migrace* složky dvojitým kliknutím na jeden řádek pro každý soubor.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Otevřít **nahradit v souborech** dialogové okno a změnit **Hledat v** k **všechny otevřené dokumenty**.
9. Použití **nahradit v souborech** dialogové okno změnit všechny `StudentID` k `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Sestavte projekt.

(Všimněte si, že tento příklad ukazuje *nevýhodou* z `classnameID` vzoru pro pojmenovávání primárních klíčů. Pokud jste měli ID primárního klíče bez předpony názvu třídy, *žádné* přejmenování třeba teď.)

## <a name="create-and-update-a-migrations-file"></a>Vytvoření a aktualizaci souboru migrace

V balíčku správce konzoly (konzolu PMC), zadejte následující příkaz:

`Add-Migration Inheritance`

Spustit `Update-Database` příkazu v konzole PMC. Příkaz se nezdaří v tomto okamžiku, když máme existující data, která migrace nebude vědět, jak zpracovat. Zobrazí následující chyba:

*Příkaz ALTER TABLE způsobil konflikt s omezení FOREIGN KEY "FK\_dbo. Oddělení\_dbo. Osoba\_PersonID ". Ke konfliktu došlo v databázi "ContosoUniversity" table "dbo. Osoba", sloupec"PersonID".*

Otevřít *migrace\&lt; časové razítko&gt;\_Inheritance.cs* a nahraďte `Up` metodu s následujícím kódem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Spustit `update-database` příkaz znovu.

> [!NOTE]
> Je možné získat další chyby při migraci dat a provádění změn schématu. Pokud se zobrazí chyby při migraci nevyřešíte sami, můžete pokračovat v kurzu tak, že změníte připojovací řetězec *Web.config* souboru nebo odstranění databáze. Nejjednodušším způsobem je přejmenovat databázi *Web.config* souboru. Například změnit název databáze na kapacitní jednotka\_otestovat, jak je znázorněno v následujícím příkladu:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> S novou databázi, nejsou žádná data chcete migrovat a `update-database` příkaz je mnohem pravděpodobnější k dokončení bez chyb. Pokyny k odstranění databáze najdete v tématu [jak vyřadit databázi ze sady Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Pokud při volbě této možnosti budete pokračovat v kurzu, přeskočte krok nasazení na konci tohoto kurzu, protože nasazené lokality byste získali ke stejné chybě při spuštění migrace automaticky. Pokud chcete odstranění chyby migrace, je nejlepší prostředek Entity Framework fóra nebo StackOverflow.com.


## <a name="testing"></a>Testování

Spuštění tohoto webu a vyzkoušejte různé stránky. Všechno, co funguje stejně jako předtím.

V **Průzkumníka serveru** rozbalte **SchoolContext** a potom **tabulky**, a uvidíte, že **Student** a **instruktorem**  byly nahrazeny tabulky **osoba** tabulky. Rozbalte **osoba** tabulky a najdete v článku, že obsahuje všechny sloupce, které mají být používány v **Student** a **kurzů vedených** tabulky.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Klikněte pravým tlačítkem na tabulku osoba a potom klikněte na **zobrazit Data tabulky** zobrazit sloupec diskriminátoru.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Následující diagram znázorňuje strukturu nové databáze školy:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Souhrn

Tabulky na hierarchii dědičnosti má nyní implementována pro `Person`, `Student`, a `Instructor` třídy. Další informace o tomto a dalších struktury dědičnosti najdete v tématu [dědičnosti mapování strategie](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) na Morteza Manavi blogu. V dalším kurzu uvidíte některé způsoby, jak implementovat úložiště a jednotky pracovních vzorů.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k Data technologie ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [další](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
