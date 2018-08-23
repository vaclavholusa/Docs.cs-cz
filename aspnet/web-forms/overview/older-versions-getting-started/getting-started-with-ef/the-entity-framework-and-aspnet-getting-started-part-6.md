---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Začínáme s Entity Framework 4.0 Database First a technologie ASP.NET 4 webových formulářů – 6. část | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Frameworku. Ukázková aplikace je...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 57ba4a47dcc33a1fcc449bc32e9ce186e76cfb5b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757093"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Začínáme s Entity Framework 4.0 Database First a 4 webových formulářů ASP.NET – 6. část
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Framework 4.0 a Visual Studio 2010. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementace tabulky za hierarchie dědičnosti

V předchozím kurzu jste nepracovali s souvisejících dat přidáním a odstraněním relace a tak, že přidáte novou entitu, která má vztah k existující entity. Tento kurzu se dozvíte, jak implementovat dědičnosti v datovém modelu.

V objektově orientované programování, můžete dědičnost usnadňují práci s souvisejícími třídami. Můžete například vytvořit `Instructor` a `Student` třídy, které jsou odvozeny z `Person` základní třídy. Stejné typy struktury dědičnosti mezi entitami můžete vytvořit v Entity Framework.

V této části kurzu se nebude vytvářet žádné nové webové stránky. Místo toho přidejte odvozené entity do datového modelu a upravovat stávající stránky použít nové entity.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabulky na hierarchii a dědičnosti na typ tabulky

Databáze může uchovávat informace o souvisejících objektů, v tabulce jeden nebo více tabulek. Například v `School` databáze, `Person` tabulka obsahuje informace o studenty a vyučující v jediné tabulce. Některé sloupce, které platí pouze pro vyučující (`HireDate`), některé jenom pro studenty (`EnrollmentDate`) a některé na obě (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Můžete nakonfigurovat rozhraní Entity Framework k vytvoření `Instructor` a `Student` entity, které dědí `Person` entity. Tento model generování struktury dědičnosti entity z tabulky izolovanou databázi nazvali *na hierarchii tabulky* dědičnost (TPH).

Pro kurzů, které `School` databáze používá jiný model. Online kurzy a získáte kurzy jsou uložené v samostatných tabulkách, z nichž každá má cizí klíč odkazující na `Course` tabulky. Informace, které jsou společné pro oba typy kurzu jsou uloženy pouze v `Course` tabulky.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Datový model Entity Framework můžete nakonfigurovat tak, aby `OnlineCourse` a `OnsiteCourse` entity dědit z `Course` entity. Tento model generování struktury dědičnosti entit ze samostatných tabulek pro jednotlivé typy s každou samostatnou tabulku odkazující zpět na tabulku, která ukládá data, které jsou společné pro všechny typy, se nazývá *tabulky podle typu* dědičnosti (TPT).

Vzory dědičnosti TPH obecně doručit lepší výkon v Entity Framework než vzory TPT dědičnosti, protože TPT vzory může vést k dotazům komplexní spojení. Tento návod ukazuje, jak implementovat TPH dědičnosti. Můžete to udělat tak, že provedete následující kroky:

- Vytvoření `Instructor` a `Student` typy entit, které jsou odvozeny z `Person`.
- Přesunutí vlastnosti, které se týkají odvozené entity z `Person` entitu, kterou chcete odvozené entity.
- Nastavte omezení vlastností v odvozených typech.
- Ujistěte se, `Person` entity abstraktní entity.
- Mapa každý odvozené entitu, kterou chcete `Person` tabulku s podmínku, která určuje, jak určit, jestli `Person` řádek představuje, které odvozeného typu.

## <a name="adding-instructor-and-student-entities"></a>Přidání instruktorem a entity studenta

Otevřít <em>SchoolModel.edmx</em> souboru, klikněte pravým tlačítkem na neobsazenému oblast v Návrháři vyberte <strong>přidat</strong>a pak vyberte <strong>Entity</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

V **Přidat entitu** dialogové okno, název entity `Instructor` a nastavte jeho **základní typ** umožňuje `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Klikněte na tlačítko **OK**. Návrhář vytvoří `Instructor` entita, která je odvozena z `Person` entity. Nová entita ještě nemá žádné vlastnosti.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Opakujte postup pro vytvoření `Student` entita, která také se odvozuje od `Person`.

Instruktoři mít dat, proto budete muset přesunout tuto vlastnost z pouze `Person` entita, která má `Instructor` entity. V `Person` entity, klikněte pravým tlačítkem myši `HireDate` vlastnosti a klikněte na tlačítko **Vyjmout**. Klikněte pravým tlačítkem na **vlastnosti** v `Instructor` entity a klikněte na tlačítko **vložit**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

Datum přijetí ze `Instructor` entity nemůže mít hodnotu null. Klikněte pravým tlačítkem na `HireDate` vlastnost, klikněte na tlačítko **vlastnosti**a pak v **vlastnosti** okno Změnit `Nullable` k `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Opakujte postup pro přesun `EnrollmentDate` vlastnost z `Person` entita, která má `Student` entity. Ujistěte se, že můžete také nastavit `Nullable` k `False` pro `EnrollmentDate` vlastnost.

Teď, když `Person` entita obsahuje pouze vlastnosti, které jsou společné pro `Instructor` a `Student` entity (kromě navigační vlastnosti, které nejsou přesunutí), entita jde použít jenom jako základní entitu struktury dědičnosti. Proto je potřeba zajistit, že se nikdy zachází jako nezávislé entity. Klikněte pravým tlačítkem na `Person` entity, vyberte **vlastnosti**a pak v **vlastnosti** okno změnit hodnotu **abstraktní** vlastnost  **Hodnota TRUE**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mapování instruktorem a Student entity do tabulky osoby

Teď budete potřebovat k informování rozhraní Entity Framework, jak rozlišovat mezi `Instructor` a `Student` entit v databázi.

Klikněte pravým tlačítkem myši `Instructor` entity a vyberte **mapování tabulky,**. V **podrobnosti mapování** okna, klikněte na tlačítko **přidat tabulku nebo zobrazení** a vyberte **osoba**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Klikněte na tlačítko **přidat podmínku**a pak vyberte **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Změna **operátor** k **je** a **hodnotu / vlastnost** k **Not Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Opakujte postup `Students` entity, určení, že tato entita se mapuje na `Person` tabulky, když `EnrollmentDate` sloupce není null. Potom uložte a zavřete datový model.

Sestavte projekt, aby bylo možné vytvořit nové entity jako třídy a zpřístupnit je v návrháři.

## <a name="using-the-instructor-and-student-entities"></a>Pomocí kurzů vedených a entity studenta

Při vytváření webových stránek, které pracují s daty studenty a instruktorem, datové vazby je jim `Person` sady entit a filtrované podle `HireDate` nebo `EnrollmentDate` vlastnost omezit na studenty nebo vyučujících vrácená data. Ale nyní při vytvoření vazby každý ovládací prvek zdroje dat `Person` sada entit, můžete zadat pouze `Student` nebo `Instructor` typy entit, měl by být vybrán. Protože Entity Framework ví, jak odlišit studenty a vyučující v `Person` sadu entit, můžete odebrat `Where` nastavení vlastností můžete k tomu zadat ručně.

V návrháři Visual Studio, můžete zadat typ entity `EntityDataSource` ovládací prvek by měl vybrat v **EntityTypeFilter** z rozevíracího seznamu `Configure Data Source` průvodce, jak je znázorněno v následujícím příkladu.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

A **vlastnosti** okna můžete odebrat `Where` klauzule hodnoty, které jsou už je nepotřebujete, jak je znázorněno v následujícím příkladu.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Ale vzhledem k tomu, že jste změnili kód pro `EntityDataSource` ovládacích prvků pro použití `ContextTypeName` atribut, nemůžete spustit **konfigurace zdroje dat** v průvodce `EntityDataSource` ovládací prvky, které jste vytvořili. Proto budete proveďte požadované změny změnou kódu.

Otevřít *Students.aspx* stránky. V `StudentsEntityDataSource` řídit, odeberte `Where` atribut a přidat `EntityTypeFilter="Student"` atribut. Kód bude nyní vypadat podobně jako v následujícím příkladu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Nastavení `EntityTypeFilter` atribut zajišťuje, že `EntityDataSource` ovládací prvek vybere pouze zadaného typu entity. Pokud chcete načíst obě `Student` a `Instructor` typy entit, nebude nastavte tento atribut. (Budete mít možnost načítání více typů entit s jedním `EntityDataSource` řídit jenom v případě, že používáte ovládací prvek pro přístup k datům jen pro čtení. Pokud používáte `EntityDataSource` ovládací prvek pro vložení, aktualizace nebo odstranění entit a pokud je vázán na sadu entit může obsahovat více typů, je možné pracovat pouze s typem jednu entitu a je nutné nastavit tento atribut.)

Opakujte postup pro `SearchEntityDataSource` řídit, s výjimkou odebrat pouze část `Where` atribut, který vybere `Student` entity, místo aby odebrala vlastnost úplně se vynechá. Počáteční značka ovládacího prvku bude nyní vypadat podobně jako v následujícím příkladu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Spuštění stránky a ověřte, že stále funguje stejně jako dříve.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Aktualizace na následujících stránkách, které jste vytvořili v předchozích kurzech tak, aby používaly nové `Student` a `Instructor` entity místo `Person` entity, spusťte ho o ověření, že fungují jako před nástupem prostředí:

- V *StudentsAdd.aspx*, přidejte `EntityTypeFilter="Student"` k `StudentsEntityDataSource` ovládacího prvku. Kód bude nyní vypadat podobně jako v následujícím příkladu: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- V *About.aspx*, přidejte `EntityTypeFilter="Student"` k `StudentStatisticsEntityDataSource` řídit a odebrat `Where="it.EnrollmentDate is not null"`. Kód bude nyní vypadat podobně jako v následujícím příkladu: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- V *Instructors.aspx* a *InstructorsCourses.aspx*, přidejte `EntityTypeFilter="Instructor"` k `InstructorsEntityDataSource` řídit a odebrat `Where="it.HireDate is not null"`. Kód v *Instructors.aspx* teď vypadá podobně jako v následujícím příkladu: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Kód v *InstructorsCourses.aspx* bude nyní vypadat následovně:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

V důsledku těchto změn jsme zlepšili aplikace Contoso University udržovatelnosti několika způsoby. Po přesunutí logiku pro výběr a ověření mimo vrstvě uživatelského rozhraní (*.aspx* značek) a je nedílnou součástí vrstvy přístupu k datům. To pomáhá izolovat kód aplikace před změnami, které můžete provést v budoucnu schématu databáze nebo datový model. Například může rozhodnout, že studenti může tak jako pomůcky učitelů a proto byste získali datum přijetí. Pak můžete přidat nové vlastnosti k odlišení od Instruktoři studenty a aktualizace datového modelu. Žádný kód ve webové aplikaci, bude nutné změnit s výjimkou případů, ve kterém chcete zobrazit datum přijetí pro studenty. Další výhodou přidání `Instructor` a `Student` entity je, že váš kód je srozumitelný snadněji než kdy ji uvedené `Person` objekty, které byly ve skutečnosti studenty nebo vyučujících.

Nyní jste viděli jedním ze způsobů pro implementaci vzoru dědičnosti v Entity Framework. V následujícím kurzu se dozvíte, jak pomocí uložených procedur, pokud chcete mít větší kontrolu nad jak Entity Framework přistupuje k databázi.

> [!div class="step-by-step"]
> [Předchozí](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [další](the-entity-framework-and-aspnet-getting-started-part-7.md)
