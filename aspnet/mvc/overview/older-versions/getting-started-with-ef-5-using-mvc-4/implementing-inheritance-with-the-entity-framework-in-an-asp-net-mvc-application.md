---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementace dědičnosti s rozhraním Entity Framework v aplikaci ASP.NET MVC (8 10) | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje postup vytvoření aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a sady Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b39fc609007d437dd0845f9087dd5a32272cebe9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821273"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a><span data-ttu-id="fdf0a-103">Implementace dědičnosti s rozhraním Entity Framework v aplikaci ASP.NET MVC (8 10)</span><span class="sxs-lookup"><span data-stu-id="fdf0a-103">Implementing Inheritance with the Entity Framework in an ASP.NET MVC Application (8 of 10)</span></span>
====================
<span data-ttu-id="fdf0a-104">podle [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="fdf0a-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="fdf0a-105">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="fdf0a-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="fdf0a-106">Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="fdf0a-107">Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="fdf0a-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="fdf0a-108">Série kurzů můžete začít od začátku nebo [stáhnout počáteční projekt pro tuto kapitolu](building-the-ef5-mvc4-chapter-downloads.md) a začněte tady.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="fdf0a-109">Pokud narazíte na problém nevyřešíte sami, [stáhnout dokončený kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a zkuste problém reprodukovat.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="fdf0a-110">Porovnáním kód Dokončený kód v obecně najdete řešení problému.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="fdf0a-111">Některé běžné chyby a jejich řešení najdete v tématu [chyby a náhradní řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="fdf0a-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="fdf0a-112">V předchozím kurzu zpracování výjimky souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-112">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="fdf0a-113">Tento kurzu se dozvíte, jak implementovat dědičnosti v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-113">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="fdf0a-114">V objektově orientované programování, můžete dědičnost vyloučit redundantní kód.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-114">In object-oriented programming, you can use inheritance to eliminate redundant code.</span></span> <span data-ttu-id="fdf0a-115">V tomto kurzu změníte `Instructor` a `Student` tak, že jsou odvozeny z třídy `Person` základní třída, která obsahuje vlastnosti, například `LastName` , které jsou společné pro vyučující a studenty.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-115">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="fdf0a-116">Nebude přidat nebo změnit libovolné webové stránky, ale změníte kód, a tyto změny se automaticky projeví v databázi.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-116">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a><span data-ttu-id="fdf0a-117">Tabulky na hierarchii a dědičnosti na typ tabulky</span><span class="sxs-lookup"><span data-stu-id="fdf0a-117">Table-per-Hierarchy versus Table-per-Type Inheritance</span></span>

<span data-ttu-id="fdf0a-118">V objektově orientované programování, můžete dědičnost usnadňují práci s souvisejícími třídami.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-118">In object-oriented programming, you can use inheritance to make it easier to work with related classes.</span></span> <span data-ttu-id="fdf0a-119">Například `Instructor` a `Student` tříd v `School` datový model sdílet několik vlastností, výsledek bude redundantní kód:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-119">For example, the `Instructor` and `Student` classes in the `School` data model share several properties, which results in redundant code:</span></span>

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

<span data-ttu-id="fdf0a-121">Předpokládejme, že chcete vyloučit redundantní kód pro vlastnosti, které jsou sdíleny `Instructor` a `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="fdf0a-122">Můžete vytvořit `Person` základní třída, která obsahuje pouze ty sdílené vlastnosti a pak proveďte `Instructor` a `Student` entit dědí ze základní třídy, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-122">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

<span data-ttu-id="fdf0a-124">Existuje několik způsobů, které tato struktura dědičnosti můžou být reprezentované v databázi.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-124">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="fdf0a-125">Mohli byste mít `Person` tabulku, která obsahuje informace o studenty a vyučující v jediné tabulce.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-125">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="fdf0a-126">Některé sloupce, které může používat jenom u Instruktoři (`HireDate`), některé jenom pro studenty (`EnrollmentDate`), některé obě (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="fdf0a-126">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="fdf0a-127">Obvykle byste měli *diskriminátoru* sloupec, aby označoval, jaký typ každý řádek představuje.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-127">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="fdf0a-128">Pro sloupec diskriminátoru může mít například "Kurzů vedených" pro vyučující a "Studentů" pro studenty.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-128">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Tabulka za hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

<span data-ttu-id="fdf0a-130">Tento model generování struktury dědičnosti entity z tabulky izolovanou databázi nazvali *na hierarchii tabulky* dědičnost (TPH).</span><span class="sxs-lookup"><span data-stu-id="fdf0a-130">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="fdf0a-131">Další možností je databázi tak, aby vypadat více jako struktury dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-131">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="fdf0a-132">Například může mít pouze název pole `Person` tabulky a mít samostatné `Instructor` a `Student` tabulky s poli datum.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-132">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Tabulka za type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

<span data-ttu-id="fdf0a-134">Tento model vytváření tabulky databáze pro každou třídu entity se nazývá *tabulky podle typu* dědičnosti (TPT).</span><span class="sxs-lookup"><span data-stu-id="fdf0a-134">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="fdf0a-135">Vzory dědičnosti TPH obecně doručit lepší výkon v Entity Framework než vzory TPT dědičnosti, protože TPT vzory může vést k dotazům komplexní spojení.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-135">TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span> <span data-ttu-id="fdf0a-136">Tento kurz ukazuje, jak implementovat TPH dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="fdf0a-137">Můžete to udělat tak, že provedete následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-137">You'll do that by performing the following steps:</span></span>

- <span data-ttu-id="fdf0a-138">Vytvoření `Person` třídy a změňte `Instructor` a `Student` třídy, které jsou odvozeny z `Person`.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-138">Create a `Person` class and change the `Instructor` and `Student` classes to derive from `Person`.</span></span>
- <span data-ttu-id="fdf0a-139">Přidejte databázi model mapování kód do třídy kontextu databáze.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-139">Add model-to-database mapping code to the database context class.</span></span>
- <span data-ttu-id="fdf0a-140">Změna `InstructorID` a `StudentID` odkazy v celém projektu `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-140">Change `InstructorID` and `StudentID` references throughout the project to `PersonID`.</span></span>

## <a name="creating-the-person-class"></a><span data-ttu-id="fdf0a-141">Vytvoření třídy osoby</span><span class="sxs-lookup"><span data-stu-id="fdf0a-141">Creating the Person Class</span></span>

 <span data-ttu-id="fdf0a-142">Poznámka: Nebude možné ke kompilaci projektu po vytvoření třídy níže, dokud neaktualizujete řadičích, které používá tyto třídy.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-142">Note: You won't be able to compile the project after creating the classes below until you update the controllers that uses these classes.</span></span> 

<span data-ttu-id="fdf0a-143">V *modely* složku, vytvořte *Person.cs* a nahraďte kód šablony následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-143">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="fdf0a-144">V *Instructor.cs*, odvozovat `Instructor` třídy z `Person` třídy a odstraňte klíč a název pole.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-144">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="fdf0a-145">Kód bude vypadat jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-145">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="fdf0a-146">Provádět podobné změny *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-146">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="fdf0a-147">`Student` Třídy bude vypadat jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-147">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a><span data-ttu-id="fdf0a-148">Přidání typu Person Entity do modelu</span><span class="sxs-lookup"><span data-stu-id="fdf0a-148">Adding the Person Entity Type to the Model</span></span>

<span data-ttu-id="fdf0a-149">V *SchoolContext.cs*, přidejte `DbSet` vlastnost `Person` typ entity:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-149">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="fdf0a-150">To je vše, Entity Framework potřebuje, aby konfigurace tabulky na hierarchii dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="fdf0a-151">Jak uvidíte, kdy databáze nebude znovu vytvořena, bude mít `Person` tabulky místo `Student` a `Instructor` tabulky.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-151">As you'll see, when the database is re-created, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="changing-instructorid-and-studentid-to-personid"></a><span data-ttu-id="fdf0a-152">Změna InstructorID a StudentID PersonID</span><span class="sxs-lookup"><span data-stu-id="fdf0a-152">Changing InstructorID and StudentID to PersonID</span></span>

<span data-ttu-id="fdf0a-153">V *SchoolContext.cs*, v příkazu mapování kurzů vedených kurzu změňte `MapRightKey("InstructorID")` k `MapRightKey("PersonID")`:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-153">In *SchoolContext.cs*, in the Instructor-Course mapping statement, change `MapRightKey("InstructorID")` to `MapRightKey("PersonID")`:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

<span data-ttu-id="fdf0a-154">Tato změna není vyžadována; pouze změní název InstructorID sloupce v tabulce many-to-many spojení.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-154">This change isn't required; it just changes the name of the InstructorID column in the many-to-many join table.</span></span> <span data-ttu-id="fdf0a-155">Pokud jste nechali název jako InstructorID, aplikace bude i nadále fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-155">If you left the name as InstructorID, the application would still work correctly.</span></span> <span data-ttu-id="fdf0a-156">Tady je dokončené *SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-156">Here is the completed *SchoolContext.cs*:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

<span data-ttu-id="fdf0a-157">Dál je potřeba změnit `InstructorID` k `PersonID` a `StudentID` k `PersonID` v celém projektu ***s výjimkou*** v souborech migrace časovým razítkem v *migrace* složka.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-157">Next you need to change `InstructorID` to `PersonID` and `StudentID` to `PersonID` throughout the project ***except*** in the time-stamped migrations files in the *Migrations* folder.</span></span> <span data-ttu-id="fdf0a-158">K tomu budete najít a otevřít pouze soubory, které je potřeba změnit a potom proveďte globálních změn na otevřené soubory.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-158">To do that you'll find and open only the files that need to be changed, then perform a global change on the opened files.</span></span> <span data-ttu-id="fdf0a-159">Jediným souborem v *migrace* je složka, měli byste změnit *Migrations\Configuration.cs.*</span><span class="sxs-lookup"><span data-stu-id="fdf0a-159">The only file in the *Migrations* folder you should change is *Migrations\Configuration.cs.*</span></span>

1. > [!IMPORTANT]
   > <span data-ttu-id="fdf0a-160">Začněte tím, že zavření všech otevřených souborů v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-160">Begin by closing all the open files in Visual Studio.</span></span>
2. <span data-ttu-id="fdf0a-161">Klikněte na tlačítko **najít a nahradit – vyhledat všechny soubory** v **upravit** nabídky a vyhledejte všechny soubory v projektu, které obsahují `InstructorID`.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-161">Click **Find and Replace -- Find all Files** in the **Edit** menu, and then search for all files in the project that contain `InstructorID`.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. <span data-ttu-id="fdf0a-162">Otevřete každý soubor v **výsledky hledání** okno ***s výjimkou*** &lt;časové razítko&gt;*\_.cs* migrace souborů v *Migrace* složky dvojitým kliknutím na jeden řádek pro každý soubor.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-162">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. <span data-ttu-id="fdf0a-163">Otevřít **nahradit v souborech** dialogové okno a změnit **Hledat v** k **všechny otevřené dokumenty**.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-163">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
5. <span data-ttu-id="fdf0a-164">Použití **nahradit v souborech** dialogové okno změnit všechny `InstructorID` do `PersonID.`</span><span class="sxs-lookup"><span data-stu-id="fdf0a-164">Use the **Replace in Files** dialog to change all `InstructorID` to `PersonID.`</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. <span data-ttu-id="fdf0a-165">Vyhledat všechny soubory v projektu, který obsahují `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-165">Find all the files in the project that contain `StudentID`.</span></span>
7. <span data-ttu-id="fdf0a-166">Otevřete každý soubor v **výsledky hledání** okno ***s výjimkou*** &lt;časové razítko&gt;*\_\*.cs* migrace souborů v *migrace* složky dvojitým kliknutím na jeden řádek pro každý soubor.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-166">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_\*.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. <span data-ttu-id="fdf0a-167">Otevřít **nahradit v souborech** dialogové okno a změnit **Hledat v** k **všechny otevřené dokumenty**.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-167">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
9. <span data-ttu-id="fdf0a-168">Použití **nahradit v souborech** dialogové okno změnit všechny `StudentID` k `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-168">Use the **Replace in Files** dialog to change all `StudentID` to `PersonID`.</span></span>   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. <span data-ttu-id="fdf0a-169">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-169">Build the project.</span></span>

<span data-ttu-id="fdf0a-170">(Všimněte si, že tento příklad ukazuje *nevýhodou* z `classnameID` vzoru pro pojmenovávání primárních klíčů.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-170">(Note that this demonstrates a *disadvantage* of the `classnameID` pattern for naming primary keys.</span></span> <span data-ttu-id="fdf0a-171">Pokud jste měli ID primárního klíče bez předpony názvu třídy, *žádné* přejmenování třeba teď.)</span><span class="sxs-lookup"><span data-stu-id="fdf0a-171">If you had named primary keys ID without prefixing the class name, *no* renaming would be necessary now.)</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="fdf0a-172">Vytvoření a aktualizaci souboru migrace</span><span class="sxs-lookup"><span data-stu-id="fdf0a-172">Create and Update a Migrations File</span></span>

<span data-ttu-id="fdf0a-173">V balíčku správce konzoly (konzolu PMC), zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-173">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="fdf0a-174">Spustit `Update-Database` příkazu v konzole PMC.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-174">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="fdf0a-175">Příkaz se nezdaří v tomto okamžiku, když máme existující data, která migrace nebude vědět, jak zpracovat.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-175">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="fdf0a-176">Zobrazí následující chyba:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-176">You get the following error:</span></span>

<span data-ttu-id="fdf0a-177">*Příkaz ALTER TABLE způsobil konflikt s omezení FOREIGN KEY "FK\_dbo. Oddělení\_dbo. Osoba\_PersonID ". Ke konfliktu došlo v databázi "ContosoUniversity" table "dbo. Osoba", sloupec"PersonID".*</span><span class="sxs-lookup"><span data-stu-id="fdf0a-177">*The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK\_dbo.Department\_dbo.Person\_PersonID". The conflict occurred in database "ContosoUniversity", table "dbo.Person", column 'PersonID'.*</span></span>

<span data-ttu-id="fdf0a-178">Otevřít *migrace\&lt; časové razítko&gt;\_Inheritance.cs* a nahraďte `Up` metodu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-178">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

<span data-ttu-id="fdf0a-179">Spustit `update-database` příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-179">Run the `update-database` command again.</span></span>

> [!NOTE]
> <span data-ttu-id="fdf0a-180">Je možné získat další chyby při migraci dat a provádění změn schématu.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="fdf0a-181">Pokud se zobrazí chyby při migraci nevyřešíte sami, můžete pokračovat v kurzu tak, že změníte připojovací řetězec *Web.config* souboru nebo odstranění databáze.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or deleting the database.</span></span> <span data-ttu-id="fdf0a-182">Nejjednodušším způsobem je přejmenovat databázi *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="fdf0a-183">Například změnit název databáze na kapacitní jednotka\_otestovat, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-183">For example, change the database name to CU\_test as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> <span data-ttu-id="fdf0a-184">S novou databázi, nejsou žádná data chcete migrovat a `update-database` příkaz je mnohem pravděpodobnější k dokončení bez chyb.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="fdf0a-185">Pokyny k odstranění databáze najdete v tématu [jak vyřadit databázi ze sady Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="fdf0a-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="fdf0a-186">Pokud při volbě této možnosti budete pokračovat v kurzu, přeskočte krok nasazení na konci tohoto kurzu, protože nasazené lokality byste získali ke stejné chybě při spuštění migrace automaticky.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial, since the deployed site would get the same error when it runs migrations automatically.</span></span> <span data-ttu-id="fdf0a-187">Pokud chcete odstranění chyby migrace, je nejlepší prostředek Entity Framework fóra nebo StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-187">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="fdf0a-188">Testování</span><span class="sxs-lookup"><span data-stu-id="fdf0a-188">Testing</span></span>

<span data-ttu-id="fdf0a-189">Spuštění tohoto webu a vyzkoušejte různé stránky.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-189">Run the site and try various pages.</span></span> <span data-ttu-id="fdf0a-190">Všechno, co funguje stejně jako předtím.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-190">Everything works the same as it did before.</span></span>

<span data-ttu-id="fdf0a-191">V **Průzkumníka serveru** rozbalte **SchoolContext** a potom **tabulky**, a uvidíte, že **Student** a **instruktorem**  byly nahrazeny tabulky **osoba** tabulky.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-191">In **Server Explorer,** expand **SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="fdf0a-192">Rozbalte **osoba** tabulky a najdete v článku, že obsahuje všechny sloupce, které mají být používány v **Student** a **kurzů vedených** tabulky.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-192">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="fdf0a-194">Klikněte pravým tlačítkem na tabulku osoba a potom klikněte na **zobrazit Data tabulky** zobrazit sloupec diskriminátoru.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="fdf0a-195">Následující diagram znázorňuje strukturu nové databáze školy:</span><span class="sxs-lookup"><span data-stu-id="fdf0a-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a><span data-ttu-id="fdf0a-197">Souhrn</span><span class="sxs-lookup"><span data-stu-id="fdf0a-197">Summary</span></span>

<span data-ttu-id="fdf0a-198">Tabulky na hierarchii dědičnosti má nyní implementována pro `Person`, `Student`, a `Instructor` třídy.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-198">Table-per-hierarchy inheritance has now been implemented for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="fdf0a-199">Další informace o tomto a dalších struktury dědičnosti najdete v tématu [dědičnosti mapování strategie](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) na Morteza Manavi blogu.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-199">For more information about this and other inheritance structures, see [Inheritance Mapping Strategies](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) on Morteza Manavi's blog.</span></span> <span data-ttu-id="fdf0a-200">V dalším kurzu uvidíte některé způsoby, jak implementovat úložiště a jednotky pracovních vzorů.</span><span class="sxs-lookup"><span data-stu-id="fdf0a-200">In the next tutorial you'll see some ways to implement the repository and unit of work patterns.</span></span>

<span data-ttu-id="fdf0a-201">Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k Data technologie ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="fdf0a-201">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fdf0a-202">[Předchozí](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [další](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="fdf0a-202">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span></span>
