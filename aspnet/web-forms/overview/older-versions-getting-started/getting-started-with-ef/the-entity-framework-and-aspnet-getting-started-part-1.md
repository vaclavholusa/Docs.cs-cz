---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Začínáme s databáze Entity Framework 4.0 nejprve a ASP.NET 4 webových formulářů | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET pomocí Entity Framework 4.0 a Visual Studio 2010...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: ad504b02d801f9513787f9fde1a4d00d7b0afff0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889485"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Začínáme s databáze Entity Framework 4.0 nejprve a ASP.NET 4 – webové formuláře
====================
Podle [tní Dykstra](https://github.com/tdykstra)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET pomocí sady Visual Studio 2010 a Entity Framework 4.0. Ukázkové aplikace je web pro fiktivní vysoké školy Contoso. Obsahuje funkce, jako je jejich příchodu student, postupu vytvoření a přiřazení lektorem.
> 
> Tento kurz ukazuje příklady v jazyce C#. [Ke stažení ukázkové](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) obsahuje kód v jak C# a Visual Basic.
> 
> ## <a name="database-first"></a>První databáze
> 
> Existují tři způsoby, můžete pracovat s daty v Entity Framework: *Database First*, *Model First*, a *Code First*. Tento kurz je určen pro první databáze. Informace o rozdílech mezi tyto pracovní postupy a pokyny o tom, jak zvolit tu nejvhodnější pro váš scénář najdete v tématu [Entity Framework vývoj pracovních](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>webové formuláře
> 
> Tento kurz řady používá model webových formulářů ASP.NET a předpokládá, že víte, jak pracovat s webovými formuláři ASP.NET v sadě Visual Studio. Pokud ne, najdete v části [Začínáme s webovými formuláři ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Pokud dáváte přednost práci s rozhraní ASP.NET MVC, přečtěte si téma [Začínáme s rozhraní Entity Framework používat rozhraní ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Verze softwaru
> 
> | **Zobrazí v tomto kurzu** | **Taky spolupracuje se službou** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express pro Web. Tento kurz nebyl testován s novější verzí sady Visual Studio. Existuje mnoho rozdílů v nabídce Možnosti, dialogová okna a šablony. |
> | .NET 4 | Rozhraní .NET 4.5 je zpětně kompatibilní s .NET 4, ale tento kurz nebyl testován s .NET 4.5. |
> | Rozhraní Entity Framework 4 | Tento kurz nebyl testován pomocí novější verze Entity Framework. Od verze Entity Framework 5, EF používá ve výchozím nastavení `DbContext API` která byla zavedená s EF 4.1. Ovládací prvek EntityDataSource byla navržená tak, aby použít `ObjectContext` rozhraní API. Informace o tom, jak používat EntityDataSource řízení s `DbContext` rozhraní API, najdete v části [tomto příspěvku na blogu](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Otázky
> 
> Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [ASP.NET Entity Framework fórum](https://forums.asp.net/1227.aspx), [Entity Framework a technologie LINQ to Entities fórum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), nebo [ StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Přehled

Aplikace, kterou jste budete vytvářet v těchto kurzech je jednoduchý univerzity Web.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Uživatelé mohou zobrazit a aktualizovat studenty, během a lektorem informace. Níže jsou uvedeny některé obrazovky, které vytvoříte.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Vytváření webové aplikace

Pokud chcete spustit tento kurz, otevřete Visual Studio a pak vytvořte nový projekt webové aplikace ASP.NET pomocí **webové aplikace ASP.NET** šablony:

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Tato šablona vytvoří projekt webové aplikace, která již obsahuje šablony stylů a hlavní stránky:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Otevřete *Site.Master* soubor a změňte "Moje aplikace technologie ASP.NET" na "Univerzity Contoso".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Najít *nabídky* ovládací prvek s názvem `NavigationMenu` a nahraďte ji metodou následující kód, který přidá položky nabídky pro stránky budete vytváření.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Otevřete *Default.aspx* stránky a změňte `Content` ovládací prvek s názvem `BodyContent` k tomuto:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Nyní máte jednoduchý Domovská stránka s odkazy na stránky, které budete vytváření:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Vytvoření databáze

Tyto kurzy použijete návrháře model dat Entity Framework pro automatické vytvoření datového modelu na základě na existující databázi (často říká *první databáze* přístup). Alternativu, která není součástí tohoto kurzu řady je ručně vytvořit datový model a pak mít návrháře generování skriptů, které vytvářejí databáze ( *první model* přístup).

Pro první databáze metodu použitou v tomto kurzu dalším krokem je přidání databáze do lokality. Nejjednodušším způsobem je nejdřív si stáhněte projekt, který přejde v tomto kurzu. Klikněte pravým tlačítkem *aplikace\_Data* složky, vyberte **přidat existující položku**a vyberte *School.mdf* soubor databáze ze staženého projektu.

Alternativou je podle pokynů v [vytváření ukázkovou databázi školy](https://msdn.microsoft.com/library/bb399731.aspx). Ať už stáhnout databázi nebo ji vytvořit, zkopírujte *School.mdf* soubor ve složce následující do vaší aplikace *aplikace\_Data* složky:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Toto umístění *.mdf* souboru se předpokládá, že používáte systém SQL Server 2008 Express.)

Pokud vytvoříte databázi ze skriptu, proveďte následující kroky k vytvoření databáze diagram:

1. V **Průzkumníka serveru**, rozbalte položku **připojení dat**, rozbalte položku *School.mdf*, klikněte pravým tlačítkem na **databázových schématech**a vyberte **Přidat nový Diagram**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Vyberte všechny tabulky a pak klikněte na tlačítko **přidat**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server vytvoří diagram databáze, který znázorňuje tabulek, sloupců tabulky a relace mezi tabulkami. V tabulkách přibližně a uspořádávat je ale chcete se můžete přesunout.
3. Diagram jako "SchoolDiagram" uložte a zavřete ji.

Pokud stahujete *School.mdf* soubor, který prochází v tomto kurzu dvojitým kliknutím můžete zobrazit diagram databáze **SchoolDiagram** pod **databázových schématech** v **Průzkumníka serveru**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Diagram vypadá přibližně takto (tabulky může být v různých umístěních z tady zobrazené):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Vytvoření datového modelu Entity Framework

Nyní můžete vytvořit datový model Entity Framework z této databáze. Můžete vytvořit datový model v kořenové složce aplikace, ale pro účely tohoto kurzu budete umístěte jej do složky s názvem *DAL* (pro Data Access Layer).

V **Průzkumníku řešení**, přidání složky na projektu s názvem *DAL* (zkontrolujte, zda je v projektu, není v části řešení).

Klikněte pravým tlačítkem myši *DAL* složku a potom vyberte **přidat** a **novou položku**. V části **nainstalovaných šablonách**, vyberte **Data**, vyberte **ADO.NET Entity Data Model** šablony, pojmenujte ji *SchoolModel.edmx*, a pak klikněte na tlačítko **přidat**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Spustí se průvodce Entity Data Model. V prvním kroku průvodce **generování z databáze** ve výchozím nastavení je vybraná možnost. Klikněte na tlačítko **Další**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

V **vybrat datové připojení** krok, nechte výchozí hodnoty a klikněte na **Další**. Databázi školy je vybrán ve výchozím nastavení a nastavení připojení je uložen v *Web.config* souboru jako **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

V **zvolte si databázové objekty** kroku průvodce, Vybrat vše tabulek s výjimkou `sysdiagrams` (který byl vytvořen pro diagram, který jste vygenerovali dříve) a potom klikněte na **Dokončit**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Po jeho dokončení vytvoření modelu, Visual Studio zobrazí grafické reprezentace objektů Entity Framework (entit), které odpovídají databázových tabulek. (Stejně jako u diagram databáze umístění jednotlivých prvků může být odlišný od co se zobrazí v tomto obrázku. Můžete přetáhnout elementy přibližně tak, aby odpovídala na obrázku, pokud chcete.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Zkoumání datového modelu Entity Framework

Uvidíte, že vypadá velmi podobně jako diagram databáze, s několika rozdíly diagramu entity. Jeden rozdíl je v přidání symbolů na konci každé přidružení, které označují typ přidružení (relace mezi tabulkami se nazývají přidružení entity v datovém modelu):

- Přidružení-nula nebo 1 je reprezentována "1" a "0..1".

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    V takovém případě `Person` entita může nebo nemusí být přidružené `OfficeAssignment` entity. `OfficeAssignment` Entit musí být přidružen `Person` entity. Jinými slovy lektorem může nebo nemusí být přiřazená office, a všechny office lze přiřadit pouze jeden lektorem.
- Přidružení k více je reprezentována "1" a "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    V takovém případě `Person` entita může nebo nemusí být přidruženy `StudentGrade` entity. A `StudentGrade` entit musí být přidružený jeden `Person` entity. `StudentGrade` entity ve skutečnosti představují zaregistrovaná kurzy v této databázi; Pokud student je zaregistrované v kurzu a neexistuje žádná třída ještě `Grade` vlastnost má hodnotu null. Jinými slovy student nemusí být zaregistrované v žádné kurzy ještě nebyla, může být zaregistrované v jeden kurzu nebo může být zaregistrované v několika kurzy. Každé třídy v zaregistrovaných kurzu platí pro pouze jeden student.
- Přidružení m: n je reprezentována "\*"a"\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    V takovém případě `Person` entita může nebo nemusí být přidruženy `Course` entity a naopak je také hodnotu true: `Course` entita může nebo nemusí být přidruženy `Person` entity. Jinými slovy lektorem může naučit více kurzy a kurzu může výukové ve více vyučující. (V této databázi tento vztah se vztahuje pouze na vyučující; ho neobsahuje odkazy studenty na kurzy. Studenti, kteří jsou propojeny s kurzy v tabulce StudentGrades.)

Další rozdíl mezi diagram databáze a datový model je další **navigační vlastnosti** část pro každou entitu. Vlastnost navigace entita odkazuje na související entity. Například `Courses` vlastnost `Person` entity obsahuje kolekci všech `Course` entit, které se vztahují k které `Person` entity.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Ještě další rozdíl mezi databáze a datový model je absenci `CourseInstructor` tabulky přidružení, který se používá v databázi propojení `Person` a `Course` tabulky v relaci m: n. Navigační vlastnosti umožňují získat související `Course` entity z `Person` entity a související `Person` entity z `Course` entity, takže není nutné, aby reprezentoval přidružení tabulky v datovém modelu.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Pro účely tohoto kurzu, Předpokládejme, že `FirstName` sloupec `Person` tabulky ve skutečnosti obsahuje křestní jméno osoby i křestní jméno. Chcete změnit název pole tak, aby odrážela to však správce databáze (DBA) nemusí chcete změnit databázi. Můžete změnit název `FirstName` vlastnost v datovém modelu, a nechat svou databázi ekvivalentní beze změny.

V návrháři, klikněte pravým tlačítkem na **FirstName** v `Person` entity a potom vyberte **přejmenovat**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Zadejte nový název "FirstMidName". Tato operace změní způsob, jakým odkazovat na sloupec v kódu beze změny databáze.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

V prohlížeči modelu poskytuje jiný způsob, jak zobrazit databázové struktury, strukturu datového modelu a mapování mezi nimi. Nevidíte, klikněte pravým tlačítkem myši na prázdnou oblast v entity designeru a pak klikněte na **prohlížeče modelu**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**Prohlížeče modelu** podokně se zobrazí v zobrazení stromu. ( **Prohlížeče modelu** podokně může ukotven s **Průzkumníku řešení** podokně.) **Nazvaného SchoolModel** představuje uzel strukturu datového modelu a **SchoolModel.Store** představuje uzel struktury databáze.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Rozbalte položku **SchoolModel.Store** v tabulkách najdete rozbalte **tabulky nebo zobrazení** vidět tabulky, a potom rozbalte **kurzu** zobrazíte sloupce v tabulce.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Rozbalte položku **nazvaného SchoolModel**, rozbalte položku **typy entit**a potom rozbalte **kurzu** uzlu entit a vlastnosti v rámci entity.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

Buď Designer nebo **prohlížeče modelu** podokně se zobrazí, jak rozhraní Entity Framework souvisí objekty ze dvou modelů. Klikněte pravým tlačítkem myši `Person` entity a vyberte **mapování tabulky**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Tím se otevře **podrobnosti mapování** okno. Všimněte si, že toto okno umožňuje vidět sloupce databáze `FirstName` je namapována na `FirstMidName`, což je, co jste přejmenovali jeho v datovém modelu.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Rozhraní Entity Framework se používá k ukládání informací o databázi, v datovém modelu a mapování mezi nimi soubor XML. *SchoolModel.edmx* soubor je ve skutečnosti soubor XML, který obsahuje tyto informace. Návrháře vykreslí informace ve formátu grafického rozhraní, ale můžete také zobrazit soubor ve formátu XML kliknutím pravým tlačítkem myši *.edmx* souboru v **Průzkumníku řešení**, kliknete na příkaz **otevřít v**a výběr **editoru XML (Text)**. (Návrháře dat modelu a XML editor jsou právě dva různé způsoby otevírání a práci s ke stejnému souboru, takže nemůže mít designer otevřete a otevřete soubor v editoru XML ve stejnou dobu.)

Nyní jste vytvořili web, databázi a datový model. V další návodu budete začnete pracovat s daty pomocí v datovém modelu a ASP.NET `EntityDataSource` ovládacího prvku.

> [!div class="step-by-step"]
> [Next](the-entity-framework-and-aspnet-getting-started-part-2.md)
