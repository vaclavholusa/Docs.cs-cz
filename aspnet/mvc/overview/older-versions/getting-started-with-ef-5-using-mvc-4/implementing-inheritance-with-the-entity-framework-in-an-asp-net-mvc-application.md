---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementace dědičnosti s rozhraní Entity Framework v aplikaci ASP.NET MVC (8 10) | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: ee088f841bdb68f4806b0b62be7d379b9eab9f8c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementace dědičnosti s rozhraní Entity Framework v aplikaci ASP.NET MVC (8 10)
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio 2012. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Kurz řady můžete začít od začátku nebo [stáhnout počáteční projekt pro tato kapitola](building-the-ef5-mvc4-chapter-downloads.md) a začněte zde.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte, [stáhnout dokončené kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a pokuste se znovu provést váš problém. Řešení problému můžete získat obecně tak, že porovnáte svůj kód Dokončený kód. Pro některé běžné chyby a jak je vyřešit, najdete v části [chyby a řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V tomto kurzu předchozí zpracovává výjimky souběžnosti. Tento kurz vám ukáže, jak implementovat dědičnosti v datovém modelu.

V objektově orientované programování, můžete dědičnost eliminovat redundantní kódu. V tomto kurzu změníte `Instructor` a `Student` tak, aby se odvozena od třídy `Person` základní třída, která obsahuje vlastnosti, jako `LastName` , které jsou společné pro vyučující a studenti. Nebude přidat nebo změnit jakékoli webové stránky, ale budete měnit kód, a tyto změny se automaticky zobrazí v databázi.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabulka za hierarchie versus za typ tabulky dědičnosti

V objektově orientované programování, můžete dědičnost usnadnění práce s související třídy. Například `Instructor` a `Student` třídy v `School` datový model sdílet několik vlastností, které vede redundantní kódu:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Předpokládejme, že chcete odstranit redundantní kód pro vlastnosti, které jsou sdíleny `Instructor` a `Student` entity. Můžete vytvořit `Person` základní třídy, který obsahuje pouze ty sdílené vlastnosti, ujistěte se, `Instructor` a `Student` entity dědí ze základní třídy, jak je znázorněno na následujícím obrázku:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Existuje několik způsobů, které tato struktura dědičnosti může být reprezentován v databázi. Můžete mít `Person` tabulku, která obsahuje informace o studenty a vyučující v jediné tabulce. Některé sloupce může použít pouze pro vyučující (`HireDate`), některé jenom pro studenty (`EnrollmentDate`), některé na obojí (`LastName`, `FirstName`). Obvykle byste měli *diskriminátoru* sloupce k určení, který typ každý řádek představuje. Například možná sloupce diskriminátoru pro vyučující a "Student" pro studenty, "Lektorem".

![Table-per-hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Tento vzor generování strukturu dědičnosti entity z jedné tabulky databáze se nazývá *tabulky na hierarchii* dědičnost (TPH).

Alternativou je ke změně databáze vzhled strukturu dědičnosti. Například můžete mít jenom pole název `Person` tabulky a mít samostatné `Instructor` a `Student` tabulky s poli datum.

![Table-per-type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Tento vzor vytvoření databázové tabulky, pro každou třídu entity se nazývá *tabulky podle typu* dědičnosti (TPT).

Vzory dědičnosti TPH obecně doručování lepší výkon ve rozhraní Entity Framework než TPT vzory dědičnosti, protože TPT vzory může mít za následek komplexní spojení dotazy. Tento kurz ukazuje, jak implementovat dědičnost TPH. Můžete to udělat tak, že provedete následující kroky:

- Vytvoření `Person` třídy a změňte `Instructor` a `Student` odvozovat ze tříd `Person`.
- Mapování modelu do databáze kód přidejte do třídy kontextu databáze.
- Změna `InstructorID` a `StudentID` odkazy v rámci projekt `PersonID`.

## <a name="creating-the-person-class"></a>Vytvoření třídy osoba

 Poznámka: Nebude možné po vytvoření třídy níže, dokud neaktualizujete řadiče, které používá tyto třídy kompilace projektu. 

V *modely* složku vytvořit *Person.cs* a nahraďte kód šablony s následujícím kódem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

V *Instructor.cs*, odvozena `Instructor` třídy z `Person` třídy a odeberte pole klíče a název. Kód bude vypadat jako v následujícím příkladu:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Ujistěte se, podobně jako změny *Student.cs*. `Student` Třída bude vypadat jako v následujícím příkladu:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Přidávání do modelu typ Entity osoba

V *SchoolContext.cs*, přidejte `DbSet` vlastnost `Person` typ entity:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

To je vše, chcete-li nakonfigurovat tabulky za hierarchie dědičnosti vyžadující rozhraní Entity Framework. Jak zjistíte, když databáze nebude znovu vytvořena, bude mít `Person` tabulky místě `Student` a `Instructor` tabulky.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Změna InstructorID a StudentID na PersonID

V *SchoolContext.cs*, v příkazu mapování lektorem kurzu změnit `MapRightKey("InstructorID")` k `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Tato změna není povinné; změní jenom název InstructorID sloupce v tabulce m: n spojení. Pokud jste nechali název jako InstructorID, aplikace bude i nadále fungovat správně. Tady je dokončené *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Dále je třeba změnit `InstructorID` k `PersonID` a `StudentID` k `PersonID` v rámci projektu ***s výjimkou*** v souborech migrace časovým razítkem v *migrace* složka. K tomu budete najít a otevřít pouze soubory, které se musí změnit a pak provést globální změnu na otevřených souborů. Jediný soubor ve *migrace* je složka, měli byste změnit *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Začněte tím, že zavřít všechny otevřené soubory v sadě Visual Studio.
2. Klikněte na tlačítko **najít a nahradit – najít všechny soubory** v **upravit** nabídce a potom vyhledejte všechny soubory v projektu, které obsahují `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Otevřete každý soubor v **Najít výsledky** okno ***s výjimkou*** &lt;časové razítko&gt;*\_.cs* migrace souborů v *Migrace* složky poklepáním na jeden řádek pro každý soubor.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Otevřete **nahradit v souborech** dialogové okno a změňte **Hledat v** k **všechny otevřené dokumenty**.
5. Použití **nahradit v souborech** dialogové okno změnit všechny `InstructorID` do `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Najít všechny soubory v projektu, které obsahují `StudentID`.
7. Otevřete každý soubor v **Najít výsledky** okno ***s výjimkou*** &lt;časové razítko&gt;*\_\*.cs* migrace souborů v *migrace* složky poklepáním na jeden řádek pro každý soubor.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Otevřete **nahradit v souborech** dialogové okno a změňte **Hledat v** k **všechny otevřené dokumenty**.
9. Použití **nahradit v souborech** dialogové okno změnit všechny `StudentID` k `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Sestavte projekt.

(Všimněte si, že tento příklad ukazuje *nevýhodou* z `classnameID` vzor pro pojmenovávání primární klíče. Pokud jste měli ID primární klíče bez prefixu název třídy *žádné* přejmenování by nezbytné teď.)

## <a name="create-and-update-a-migrations-file"></a>Vytvoření a aktualizaci souboru migrace

V balíček správce konzoly (pomocí PMC), zadejte následující příkaz:

`Add-Migration Inheritance`

Spustit `Update-Database` v pomocí PMC. Příkaz se v tomto okamžiku nezdaří, protože máme existující data, která migrace není známo, jak bude zpracováván. Dojde k následující chybě:

*Příkaz ALTER TABLE konflikt s omezení FOREIGN KEY "Cizíklíč\_dbo. Oddělení\_dbo. Osoba\_PersonID ". Ke konfliktu došlo v databázi "ContosoUniversity" tabulka "dbo. Osoby", sloupec 'PersonID'.*

Otevřete *migrace\&lt; časové razítko&gt;\_Inheritance.cs* a nahraďte `Up` metoda následujícím kódem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Spustit `update-database` příkaz znovu.

> [!NOTE]
> Je možné získat další chyby při migraci dat a provedení změny schématu. Pokud dojde k chybám migrace nelze vyřešit, můžete pokračovat v tomto kurzu změnou připojovací řetězec *Web.config* soubor nebo odstraňování databáze. Nejjednodušší způsob je přejmenovat databázi *Web.config* souboru. Můžete například změnit název databáze na CU\_testovat, jak je znázorněno v následujícím příkladu:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> S novou databázi, nejsou k dispozici žádná data chcete migrovat a `update-database` příkaz je mnohem pravděpodobnější dokončeno bez chyb. Pokyny k odstranění databáze najdete v tématu [postup odstranění databáze ze sady Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Pokud budete pokračovat v kurzu pořídíte tuto metodu, přeskočte krok nasazení na konci tohoto kurzu, vzhledem k tomu, že bude web by dojde ke stejné chybě, když je automaticky spuštěna migrace. Pokud chcete potíží s migrací, nejlepší prostředek je některý z Entity Framework fóra nebo StackOverflow.com.


## <a name="testing"></a>Testování

Spusťte web a zkuste stránky. Všechno funguje stejným způsobem jako předtím.

V **Průzkumníku serveru** rozbalte **SchoolContext** a potom **tabulky**, a můžete vidět, že **Student** a **lektorem**  byly nahrazeny tabulky **osoba** tabulky. Rozbalte **osoba** tabulky a v tématu obsahuje všechny sloupce, které používají ve **Student** a **lektorem** tabulky.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Klikněte pravým tlačítkem na tabulky osoba a pak klikněte na **zobrazit Data tabulky** zobrazíte sloupce diskriminátoru.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Následující diagram znázorňuje strukturu nové databáze školní:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Souhrn

Teď byl implementován tabulky za hierarchie dědičnosti pro `Person`, `Student`, a `Instructor` třídy. Další informace o tomto a dalších dědičnosti struktury najdete v tématu [strategie mapování dědičnosti](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) na blogu Morteza Manavi. V dalším kurzu se zobrazí několik způsobů, jak implementovat úložiště a jednotky pracovních vzorů.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k dat ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [další](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
