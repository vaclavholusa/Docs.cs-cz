---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Implementace dědičnosti s rozhraní Entity Framework 6 v aplikaci ASP.NET MVC 5 (11 12) | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e6ee3f9c055a15b13c27f94675006b9a7e804f1b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Implementace dědičnosti s rozhraní Entity Framework 6 v aplikaci ASP.NET MVC 5 (11 12)
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stáhnout PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio 2013. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


V tomto kurzu předchozí zpracovává výjimky souběžnosti. Tento kurz vám ukáže, jak implementovat dědičnosti v datovém modelu.

V objektově orientované programování, můžete použít [dědičnosti](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) usnadňuje [kódu opakovaného použití](http://en.wikipedia.org/wiki/Code_reuse). V tomto kurzu změníte `Instructor` a `Student` tak, aby se odvozena od třídy `Person` základní třída, která obsahuje vlastnosti, jako `LastName` , které jsou společné pro vyučující a studenti. Nebude přidat nebo změnit jakékoli webové stránky, ale budete měnit kód, a tyto změny se automaticky zobrazí v databázi.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Možnosti pro mapování dědičnosti tabulkách databáze

`Instructor` a `Student` třídy v `School` datového modelu mají několik vlastností, které jsou stejné:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Předpokládejme, že chcete odstranit redundantní kód pro vlastnosti, které jsou sdíleny `Instructor` a `Student` entity. Nebo chcete napsat služba, která dokáže formátovat názvy bez caring, zda název pochází lektorem nebo student. Můžete vytvořit `Person` základní třídy, který obsahuje pouze ty sdílené vlastnosti, ujistěte se, `Instructor` a `Student` entity dědí ze základní třídy, jak je znázorněno na následujícím obrázku:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Existuje několik způsobů, které tato struktura dědičnosti může být reprezentován v databázi. Můžete mít `Person` tabulku, která obsahuje informace o studenty a vyučující v jediné tabulce. Některé sloupce může použít pouze pro vyučující (`HireDate`), některé jenom pro studenty (`EnrollmentDate`), některé na obojí (`LastName`, `FirstName`). Obvykle byste měli *diskriminátoru* sloupce k určení, který typ každý řádek představuje. Například možná sloupce diskriminátoru pro vyučující a "Student" pro studenty, "Lektorem".

![Tabulka za hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Tento vzor generování strukturu dědičnosti entity z jedné tabulky databáze se nazývá *tabulky na hierarchii* dědičnost (TPH).

Alternativou je ke změně databáze vzhled strukturu dědičnosti. Například můžete mít jenom pole název `Person` tabulky a mít samostatné `Instructor` a `Student` tabulky s poli datum.

![Tabulka za type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Tento vzor vytvoření databázové tabulky, pro každou třídu entity se nazývá *tabulky podle typu* dědičnosti (TPT).

Další možností je ještě mapování všechny typy neabstraktní na jednotlivé tabulky. Všechny vlastnosti třídy, včetně zděděné vlastnosti, mapování na sloupce odpovídající tabulky. Tento vzor nazývá dědičnost tabulky na konkrétní třídy (TPC). Pokud jste implementovali TPC dědičnosti pro `Person`, `Student`, a `Instructor` třídy, jako je uvedené výše, `Student` a `Instructor` tabulky vypadat neliší po implementace dědičnosti než dříve.

TPC a vzory dědičnosti TPH obecně poskytovat lepší výkon v rozhraní Entity Framework než TPT vzory dědičnosti, protože TPT vzory může mít za následek komplexní spojení dotazy.

Tento kurz ukazuje, jak implementovat dědičnost TPH. TPH je vzor dědičnosti výchozí v Entity Framework, tak, aby všechny stačí vytvořit `Person` třídy, změňte `Instructor` a `Student` odvozovat ze tříd `Person`, přidejte novou třídu do `DbContext`a vytvořte migrace. (Informace o tom, jak implementovat dalšími dědičnosti vzory najdete v tématu [mapování dědění za typ tabulky (TPT)](https://msdn.microsoft.com/en-us/data/jj591617#2.5) a [mapování dědičnosti tabulky na konkrétní třídy (TPC)](https://msdn.microsoft.com/en-us/data/jj591617#2.6) v webu MSDN Dokumentace Entity Framework.)

## <a name="create-the-person-class"></a>Vytvořte třídu osoba

V *modely* složku vytvořit *Person.cs* a nahraďte kód šablony s následujícím kódem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Ujistěte se, studenty a lektorem třídy dědí osoba

V *Instructor.cs*, odvozena `Instructor` třídy z `Person` třídy a odeberte pole klíče a název. Kód bude vypadat jako v následujícím příkladu:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Ujistěte se, podobně jako změny *Student.cs*. `Student` Třída bude vypadat jako v následujícím příkladu:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Přidat typ uživatel Entity do modelu

V *SchoolContext.cs*, přidejte `DbSet` vlastnost `Person` typ entity:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

To je vše, chcete-li nakonfigurovat tabulky za hierarchie dědičnosti vyžadující rozhraní Entity Framework. Jak zjistíte, když se aktualizuje databázi, bude mít `Person` tabulky místě `Student` a `Instructor` tabulky.

## <a name="create-and-update-a-migrations-file"></a>Vytvoření a aktualizaci souboru migrace

V balíček správce konzoly (pomocí PMC), zadejte následující příkaz:

`Add-Migration Inheritance`

Spustit `Update-Database` v pomocí PMC. Příkaz se v tomto okamžiku nezdaří, protože máme existující data, která migrace není známo, jak bude zpracováván. Zobrazí chybovou zprávu stejný, jako je následující:

> *Nelze vyřadit objekt ' dbo. Lektorem, protože odkazuje omezení FOREIGN KEY.*


Otevřete *migrace\&lt; časové razítko&gt;\_Inheritance.cs* a nahraďte `Up` metoda následujícím kódem:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Tento kód má na starosti následující úlohy aktualizace databáze:

- Odebere omezení cizího klíče a indexy, které ukazují na Student tabulku.
- Tabulku lektorem jako uživatel přejmenuje a provede změny, které jsou potřeba pro ni k ukládání dat Student:

    - Přidá EnrollmentDate s možnou hodnotou Null pro studenty.
    - Přidá sloupce diskriminátoru označuje, zda řádek pro student nebo lektorem.
    - Vzhledem k tomu, že řádky student nebude mít přijetím kalendářních dat, díky HireDate s možnou hodnotou Null.
    - Přidá dočasné pole, které se použije k aktualizaci cizí klíče, které odkazují na studenty. Při kopírování studenty do tabulky osob získají budete nové hodnot primárního klíče.
- Kopíruje data z tabulky studenty do tabulky osob. To způsobí, že studentů přiřazovány nové hodnot primárního klíče.
- Opravy hodnoty cizího klíče, které odkazují na studenty.
- Znovu vytvoří omezení cizího klíče a indexy, nyní je odkazující na tabulky osoba.

(Při použití GUID místo celé číslo jako typ primární klíče, student hodnot primárního klíče nebude muset změnit a některé z těchto kroků by byly vynechány.)

Spustit `update-database` příkaz znovu.

(V produkční systému by změníte odpovídající metodu nižší v případě, že jste někdy museli použít se vrátíte k předchozí verzi databáze. V tomto kurzu nebudete používat metody dolů.)

> [!NOTE]
> Je možné získat další chyby při migraci dat a provedení změny schématu. Pokud dojde k chybám migrace nelze vyřešit, můžete pokračovat v tomto kurzu změnou připojovací řetězec *Web.config* souboru nebo pomocí odstranění databáze. Nejjednodušší způsob je přejmenovat databázi *Web.config* souboru. Můžete například změňte název databáze na ContosoUniversity2, jak je znázorněno v následujícím příkladu:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> S novou databázi, nejsou k dispozici žádná data chcete migrovat a `update-database` příkaz je mnohem pravděpodobnější dokončeno bez chyb. Pokyny k odstranění databáze najdete v tématu [postup odstranění databáze ze sady Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Pokud budete pokračovat v kurzu pořídíte tuto metodu, přeskočte krok nasazení na konci tohoto kurzu nebo nasadit do nové lokality a databáze. Pokud nasazujete aktualizaci nasadit do stejné lokality, které jste si byla nasazení do již, EF dostane ke stejné chybě při spuštění migrace automaticky. Pokud chcete potíží s migrací, nejlepší prostředek je některý z Entity Framework fóra nebo StackOverflow.com.


## <a name="testing"></a>Testování

Spusťte web a zkuste stránky. Všechno funguje stejným způsobem jako předtím.

V **Průzkumníku serveru** rozbalte **Data Connections\SchoolContext** a potom **tabulky**, a uvidíte, že **Student** a **Lektorem** byly nahrazeny tabulky **osoba** tabulky. Rozbalte **osoba** tabulky a v tématu obsahuje všechny sloupce, které používají ve **Student** a **lektorem** tabulky.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Klikněte pravým tlačítkem na tabulky osoba a pak klikněte na **zobrazit Data tabulky** zobrazíte sloupce diskriminátoru.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Následující diagram znázorňuje strukturu nové databáze školní:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Nasazení do Azure

V této části vyžaduje, abyste dokončili nepovinný **nasazení aplikace do Azure** kapitoly [část 3, řazení, filtrování a stránkování](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) tento kurz řady. Pokud jste měli chyby migrace, které můžete vyřešit odstraněním databázi ve vašem místním projektu, přeskočte tento krok; nebo vytvořte novou lokalitu a databázi a nasadit do nového prostředí.

1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **publikovat** v místní nabídce.  
  
    ![Publikování v kontextové nabídce projektu](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. Klikněte na tlačítko **publikování**.  
  
    ![Publikování](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
 Webové aplikace se otevře ve výchozím prohlížeči.
3. Testování aplikace k ověření pracuje.

    Při prvním spuštění stránky, který přistupuje k databázi, rozhraní Entity Framework spustí všechny byla migrace `Up` metody, které jsou nezbytné k přivedení aktuální pomocí aktuálního modelu dat databáze.

## <a name="summary"></a>Souhrn

Jste implementovali tabulky za hierarchie dědičnosti pro `Person`, `Student`, a `Instructor` třídy. Další informace o tomto a dalších dědičnosti struktury najdete v tématu [TPT dědičnosti vzor](https://msdn.microsoft.com/en-us/data/jj618293) a [vzor dědičnosti TPH](https://msdn.microsoft.com/en-us/data/jj618292) na webu MSDN. V dalším kurzu se zobrazí, jak bude zpracováván celou řadu relativně pokročilých scénářích rozhraní Entity Framework.

Odkazy na další zdroje Entity Framework najdete v [přístup k datům ASP.NET - doporučené prostředky](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Předchozí](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[další](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
