---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Začínáme s Entity Framework 4.0 Database First a technologie ASP.NET 4 webových formulářů | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje postup vytvoření aplikace webových formulářů ASP.NET pomocí Entity Framework 4.0 a Visual Studio 2010...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9f100ccaf5e9cfdaf0633f9bfebbad273212a0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752506"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Začínáme s Entity Framework 4.0 Database First a technologie ASP.NET 4 webových formulářů
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Framework 4.0 a Visual Studio 2010. Ukázková aplikace je web pro fiktivní společnosti Contoso University. Zahrnuje funkce, jako student přijetí, kurz vytvoření a přiřazení instruktorem.
> 
> Tento kurz ukazuje příklady v jazyce C#. [Ukázky ke stažení](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) obsahuje kód v jazyce C# a Visual Basic.
> 
> ## <a name="database-first"></a>První databáze
> 
> Existují tři způsoby, jak můžete pracovat s daty v Entity Framework: *Database First*, *první Model*, a *Code First*. Tento kurz je určen pro první databázi. Informace o rozdílech mezi těmito pracovními postupy a pokyny o tom, jak vybrat tu nejlepší pro váš scénář, naleznete v tématu [Entity Framework vývojářské pracovní postupy](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>webové formuláře
> 
> V této sérii kurzů používá model webových formulářů ASP.NET a předpokládá, že víte, jak pracovat s webovými formuláři ASP.NET v sadě Visual Studio. Pokud ne, najdete v článku [Začínáme s webovými formuláři ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Pokud budete chtít pracovat s rozhraním ASP.NET MVC framework, přečtěte si téma [Začínáme s Entity Framework pomocí technologie ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Verze softwaru
> 
> | **Uvedené v tomto kurzu** | **Funguje taky s** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web. Tento kurz nebyl byly testovány pomocí novější verze sady Visual Studio. Existují mnoho rozdíly v nabídkách, dialogová okna a šablony. |
> | .NET 4 | Rozhraní .NET 4.5 je zpětně kompatibilní s .NET 4, ale tento kurz nebyl prošel testováním s využitím .NET 4.5. |
> | Entity Framework 4 | Tento kurz nebyl byly testovány pomocí novější verze rozhraní Entity Framework. Od verze Entity Framework 5, ve výchozím nastavení používá EF `DbContext API` , která byla zavedena v systému EF 4.1. Ovládací prvek EntityDataSource byla navržena pro použití `ObjectContext` rozhraní API. Informace o tom, jak používat EntityDataSource ovládacím prvkem `DbContext` rozhraní API, najdete v článku [tento příspěvek na blogu](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Dotazy
> 
> Pokud máte nějaké otázky, které přímo nesouvisejí, najdete v tomto kurzu, můžete je publikovat [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework a LINQ to Entities fórum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), nebo [ StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Přehled

Aplikace, kterou je budete vytvářet v těchto kurzech je jednoduchý university Web.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Uživatelé mohou zobrazit a aktualizovat Všichni studenti, kurz a informace instruktorem. Níže se zobrazí několik obrazovek, které vytvoříte.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Vytvoření webové aplikace

Pokud chcete začít kurz, otevřete sadu Visual Studio a vytvořte nový projekt webové aplikace ASP.NET pomocí **webová aplikace ASP.NET** šablony:

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Tato šablona vytvoří projekt webové aplikace, která už obsahuje šablony stylů a hlavní stránky:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Otevřít *Site.Master* soubor a změňte "My ASP.NET Application" k "University společnosti Contoso".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Najít *nabídky* ovládací prvek s názvem `NavigationMenu` a nahraďte ho následujícím kódem, který přidá položky nabídky pro stránky, které vytvoříte.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Otevřít *Default.aspx* stránce a změnit `Content` ovládací prvek s názvem `BodyContent` tomuto:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Teď máte jednoduchý Domovská stránka s odkazy na různé stránky, které vytvoříte:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Vytvoření databáze

Těchto kurzů použijeme návrháře modelu dat Entity Framework automaticky vytvořit datový model založený na existující databázi (často označované jako *databáze* přístup). Alternativou, pro které neplatí v této sérii kurzů je ručně vytvořte datový model a pak je mít návrháře Generovat skripty, které vytvoření databáze ( *první model* přístup).

Pro metodu databáze použitý v tomto kurzu dalším krokem je přidání databáze do lokality. Nejjednodušší způsob je nejdřív stáhnout projekt, který se odkazuje v tomto kurzu. Klepněte pravým tlačítkem myši *aplikace\_Data* složky, vyberte **přidat existující položku**a vyberte *School.mdf* soubor databáze ze staženého projektu.

Alternativou je postupujte podle pokynů na adrese [vytvoření ukázkové databáze školy](https://msdn.microsoft.com/library/bb399731.aspx). Zda stáhnout databázi nebo ji vytvořit, Kopírovat *School.mdf* souboru z následující složky do vaší aplikace *aplikace\_Data* složky:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Toto umístění *.mdf* souboru předpokládá, že používáte systém SQL Server 2008 Express.)

Pokud vytvoříte databázi ze skriptu, proveďte následující kroky a vytvoříte databázového diagramu:

1. V **Průzkumníka serveru**, rozbalte **datová připojení**, rozbalte *School.mdf*, klikněte pravým tlačítkem na **databázových diagramů**a vyberte **Přidat nový Diagram**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Vyberte všechny tabulky a pak klikněte na tlačítko **přidat**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    Vytvoří SQL Server databázového diagramu, který zobrazuje tabulek, sloupců v tabulkách a relace mezi tabulkami. Tabulky přibližně a uspořádávat je však chcete, můžete přesunout.
3. Diagram jako "SchoolDiagram" uložte a zavřete ho.

Pokud si stáhnete *School.mdf* soubor, který se odkazuje v tomto kurzu databázového diagramu můžete zobrazit dvojitým kliknutím **SchoolDiagram** pod **databázových diagramů** v **Průzkumníka serveru**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Diagram by měl vypadat asi takto (tabulky můžou být v různých umístěních, jak je zobrazeno tady):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Vytvoření datového modelu Entity Framework

Datový model Entity Framework teď můžete vytvořit z této databáze. Můžete vytvořit datový model v kořenové složce aplikace, ale pro účely tohoto kurzu budete umístěte ho do složky s názvem *DAL* (pro Data Access Layer).

V **Průzkumníka řešení**, přidejte složku projektu s názvem *DAL* (ujistěte se, že je v rámci projektu není v rámci řešení).

Klikněte pravým tlačítkem myši *DAL* složku a pak vyberte **přidat** a **nová položka**. V části **nainstalované šablony**vyberte **Data**, vyberte **datový Model Entity ADO.NET** šablony, pojmenujte ho *SchoolModel.edmx*, a pak klikněte na tlačítko **přidat**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Otevře se Průvodce datovým modelem Entity. V prvním kroku průvodce **Generovat z databáze** ve výchozím nastavení je vybraná možnost. Klikněte na tlačítko **Další**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

V **vyberte datové připojení** kroku, ponechte výchozí hodnoty a klikněte na tlačítko **Další**. Ve výchozím nastavení je vybraná databáze školy a nastavení připojení se uloží do *Web.config* soubor jako **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

V **zvolte vaše databázové objekty** krok průvodce, vyberte všechny tabulky kromě `sysdiagrams` (který byl vytvořen pro diagram, který jste vygenerovali dříve) a potom klikněte na tlačítko **Dokončit**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Po dokončení vytváření modelu, sada Visual Studio zobrazí grafické reprezentace objektů Entity Framework (entity), které odpovídají databázových tabulek. (Stejně jako u databázového diagramu umístění jednotlivých prvků může lišit od co vidíte na tomto obrázku. Můžete přetáhnout prvky přibližně tak, aby odpovídaly na obrázku, pokud chcete, aby.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Zkoumání datového modelu Entity Framework

Uvidíte, že diagram entita vypadá podobně jako databázového diagramu s několika rozdíly. Jedním rozdílem je součet symboly na konci každé přidružení, které označují typ přidružení (relací mezi tabulkami označují přidružení entity v datovém modelu):

- Přidružení k nule nebo jednom je reprezentován "1" a "0..1".

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    V tomto případě `Person` entit může nebo nemusí být přidružené `OfficeAssignment` entity. `OfficeAssignment` Entity musí být přiřazena `Person` entity. Jinými slovy instruktorem může nebo nemusí být přiřazeno k office, a jakékoli office je možné přiřadit pouze jeden instruktorem.
- Přidružení jednoho k několika je reprezentována "1" a "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    V tomto případě `Person` entit může nebo nemusí mít přidružené `StudentGrade` entity. A `StudentGrade` entita musí být přidružený nejméně k jednomu `Person` entity. `StudentGrade` entity ve skutečnosti představují zaregistrovaná kurzy v této databázi. Pokud student je zaregistrované v kurzu a neexistuje žádná třída ještě, `Grade` vlastnost má hodnotu null. Jinými slovy student nemusí být zaregistrovaná v nějaké kurzy, ale mohou být zaregistrovaná v jeden kurz nebo je možné zaregistrovat ve více. Každý na podnikové úrovni v zaregistrovaném kurzu platí pro pouze jeden studentů.
- Je reprezentována přidružení many-to-many "\*"a"\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    V tomto případě `Person` entit může nebo nemusí mít přidružené `Course` entity a naopak platí to i naopak: `Course` entit může nebo nemusí mít přidružené `Person` entity. Jinými slovy instruktorem mohou naučit více kurzů a kurzu může vedené instruktory více. (V této databázi tento vztah se vztahuje pouze na Instruktoři; to není propojena studenty ke kurzům. Studenti jsou propojené ke kurzům tabulce StudentGrades.)

Další rozdíl mezi databázového diagramu a modelu dat je další **navigační vlastnosti** část pro každou entitu. Navigační vlastnost entity odkazuje na související entity. Například `Courses` vlastnost `Person` entita obsahuje kolekci všech `Course` entity, které se vztahují k, které `Person` entity.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Další rozdíl mezi databáze a datový model je chybí, ale `CourseInstructor` přidružení tabulky, který se odkazuje v databázi `Person` a `Course` tabulky v relaci m: m. Navigační vlastnosti umožňují získat související `Course` entit na základě `Person` entity a související `Person` entit na základě `Course` entity, takže není nutné k reprezentaci přidružení tabulky v datovém modelu.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Pro účely tohoto kurzu Předpokládejme, že `FirstName` sloupec `Person` tabulka ve skutečnosti obsahuje jméno osoby i druhé křestní jméno. Chcete změnit název pole tak, aby odrážely to ale nemusí správce databáze (DBA) má změnit databázi. Můžete změnit název `FirstName` vlastnost v modelu, při opuštění ekvivalentní své databáze beze změny.

V návrháři, klikněte pravým tlačítkem na **FirstName** v `Person` entity a pak vyberte **přejmenovat**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Zadejte nový název "FirstMidName". Tím se změní tak, jak odkazovat na sloupec v kódu beze změny databáze.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Prohlížeč modelu nabízí další způsob, jak zobrazit struktura databáze, strukturu datového modelu a mapování mezi nimi. Zobrazíte ho, klikněte pravým tlačítkem na prázdnou oblast v Návrháři entity a potom klikněte na **prohlížeč modelu**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**Prohlížeč modelu** podokno zobrazuje stromovou strukturu. ( **Prohlížeč modelu** podokně může být ukotven s **Průzkumníka řešení** podokně.) **Nazvaného SchoolModel** uzel představuje strukturu modelu dat a **SchoolModel.Store** představuje uzel struktury databáze.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Rozbalte **SchoolModel.Store** zobrazíte tabulky rozbalte **tabulky / zobrazení** vidět tabulky, a potom rozbalte **kurzu** zobrazíte sloupce v tabulce.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Rozbalte **nazvaného SchoolModel**, rozbalte **typy entit**a potom rozbalte **kurzu** uzlu entity a vlastnosti v rámci těchto entit.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

V obou návrháři nebo **prohlížeč modelu** podokně se zobrazí, jak se Entity Framework vztahuje objekty ze dvou modelů. Klikněte pravým tlačítkem myši `Person` entity a vyberte **mapování tabulky,**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Tím se otevře **podrobnosti mapování** okna. Všimněte si, že toto okno můžete vidět, že sloupec databáze `FirstName` je namapována na `FirstMidName`, což je, co jste přejmenovali na v datovém modelu.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework používá k ukládání informací o databáze, datového modelu a mapování mezi nimi XML. *SchoolModel.edmx* soubor je ve skutečnosti soubor XML, který obsahuje tyto informace. Návrhář vykreslí informaci v grafické podobě, ale soubor ve formátu XML můžete také zobrazit kliknutím pravým tlačítkem myši *edmx* ve **Průzkumníku řešení**, kliknutím na příkaz **otevřít v**a výběrem **Editor (textový) XML**. (Návrháře modelů data a editoru XML jsou jen dva různé způsoby otevírání a práce s stejného souboru, takže není možné, aby návrháři otevřete a otevřete soubor v editoru XML ve stejnou dobu.)

Právě jste vytvořili na webu, databáze a datového modelu. Z dalšího návodu budete začít pracovat s daty pomocí datového modelu a technologie ASP.NET `EntityDataSource` ovládacího prvku.

> [!div class="step-by-step"]
> [Next](the-entity-framework-and-aspnet-getting-started-part-2.md)
