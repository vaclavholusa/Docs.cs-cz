---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Začínáme s databáze Entity Framework 4.0 nejprve a ASP.NET 4 webové formuláře – část 6 | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET používající rozhraní Entity Framework. Vzorová aplikace je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: b76be25501275ba676c9a9acca8e73333439ee70
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888796"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Začínáme s databáze Entity Framework 4.0 nejprve a 4 webových formulářů ASP.NET - část 6
====================
Podle [tní Dykstra](https://github.com/tdykstra)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET pomocí sady Visual Studio 2010 a Entity Framework 4.0. Informace o kurzu řady najdete v tématu [první kurz v řadě](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementace tabulky za hierarchie dědičnosti

V předchozích kurzu jste pracovali s související data přidávání a odstraňování vztahů a přidáním nové entity, který měl vztah na stávající entitu. Tento kurz vám ukáže, jak implementovat dědičnosti v datovém modelu.

V objektově orientované programování, můžete dědičnost usnadnění práce s související třídy. Například můžete vytvořit `Instructor` a `Student` třídy, které jsou odvozeny od `Person` základní třídy. Můžete vytvořit stejné typy struktur dědičnosti mezi subjektů v rozhraní Entity Framework.

V této části kurzu nebude vytvářet žádné nové webové stránky. Místo toho můžete přidat odvozené entity do datového modelu a upravovat stávající stránky používat nové entity.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabulka za hierarchie versus za typ tabulky dědičnosti

Databáze může uchovávat informace o souvisejících objektů, které v tabulce jeden nebo více tabulek. Například v `School` databáze, `Person` tabulka obsahuje informace o studenty a vyučující v jediné tabulce. Některé sloupce platí pouze pro vyučující (`HireDate`), některé jenom pro studenty (`EnrollmentDate`) a některé do obou (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Můžete nakonfigurovat rozhraní Entity Framework pro vytvoření `Instructor` a `Student` entity, které dědí od `Person` entity. Tento vzor generování strukturu dědičnosti entity z jedné tabulky databáze se nazývá *tabulky na hierarchii* dědičnost (TPH).

Pro kurzy `School` databáze používá jiný model. Online kurzy a dohlížející na bezpečný kurzy jsou uloženy v samostatných tabulkách, z nichž každá má cizí klíč, který odkazuje na `Course` tabulky. Informace, které jsou společné pro oba typy kurzu uloženo pouze v `Course` tabulky.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Datový model Entity Framework můžete nakonfigurovat tak, aby `OnlineCourse` a `OnsiteCourse` dědí entity `Course` entity. Tento vzor generování strukturu dědičnosti entity ze samostatných tabulkách pro každý typ s každou samostatné tabulky zpět odkazující na tabulku, která ukládá data, které jsou společné pro všechny typy, se nazývá *tabulky podle typu* dědičnosti (TPT).

Vzory dědičnosti TPH obecně doručování lepší výkon ve rozhraní Entity Framework než TPT vzory dědičnosti, protože TPT vzory může mít za následek komplexní spojení dotazy. Tento návod ukazuje, jak implementovat dědičnost TPH. Můžete to udělat tak, že provedete následující kroky:

- Vytvoření `Instructor` a `Student` typy entit, které jsou odvozeny od `Person`.
- Přesunutí vlastnosti, které se týkají odvozené entity z `Person` entity, která má odvozené entity.
- Nastavte omezení vlastnosti v u odvozeného typu.
- Ujistěte se, `Person` entity abstraktní entity.
- Mapa každý odvozené entity, která má `Person` tabulku s podmínku, která určuje, jak zjistit, zda `Person` řádek představuje, které odvozeného typu.

## <a name="adding-instructor-and-student-entities"></a>Přidání lektorem a Student entity

Otevřete <em>SchoolModel.edmx</em> souboru, klikněte pravým tlačítkem na oblast neobsazeného v Návrháři vyberte <strong>přidat</strong>, pak vyberte <strong>Entity</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

V **Přidat entitu** dialogové okno, název entity `Instructor` a nastavit jeho **základní typ** možnost k `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Click **OK**. Návrhář vytvoří `Instructor` entita, která je odvozena z `Person` entity. Nová entita ještě nemá žádné vlastnosti.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Opakujte postup pro vytvoření `Student` entita, která také odvozuje od `Person`.

Pouze vyučující mít přijetím data, takže potřebujete přesunout tuto vlastnost z `Person` entity, která má `Instructor` entity. V `Person` entity, klikněte pravým tlačítkem myši `HireDate` vlastnost a klikněte na tlačítko **Vyjmout**. Klikněte pravým tlačítkem **vlastnosti** v `Instructor` entity a klikněte na tlačítko **vložení**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

Datum přijetí ze `Instructor` entity nemůže mít hodnotu null. Klikněte pravým tlačítkem myši `HireDate` vlastnost, klikněte na tlačítko **vlastnosti**a potom v **vlastnosti** okno Změnit `Nullable` k `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Opakujte postup pro přesun `EnrollmentDate` vlastnost z `Person` entity, která má `Student` entity. Ujistěte se, že můžete také nastavit `Nullable` k `False` pro `EnrollmentDate` vlastnost.

Teď, když `Person` entita má pouze vlastnosti, které jsou společné pro `Instructor` a `Student` entity (kromě zajištění dostatečného navigační vlastnosti, které nejsou přesun), entity lze použít pouze jako základní entitu ve struktuře dědičnosti. Proto je potřeba zajistit, že se nikdy zpracovává jako nezávislé entity. Klikněte pravým tlačítkem myši `Person` entity, vyberte **vlastnosti**a potom v **vlastnosti** okno změňte hodnotu **abstraktní** vlastnost, která má  **Hodnota TRUE,**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mapování lektorem a Student entity do tabulky osoba

Nyní budete muset zadat rozhraní Entity Framework postup rozlišovat mezi `Instructor` a `Student` entity v databázi.

Klikněte pravým tlačítkem myši `Instructor` entity a vyberte **mapování tabulky**. V **podrobnosti mapování** okně klikněte na tlačítko **přidat tabulku nebo zobrazení** a vyberte **osoba**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Klikněte na tlačítko **přidat podmínku**a potom vyberte **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Změna **operátor** k **je** a **hodnota nebo vlastnost** k **Not Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Opakujte postup pro `Students` entit a určení, že tuto entitu se mapuje `Person` tabulky při `EnrollmentDate` sloupec není null. Potom uložte a zavřete datový model.

Sestavení projektu pro vytvoření nové entity jako třídy a jejich zpřístupnění v návrháři.

## <a name="using-the-instructor-and-student-entities"></a>Použitím lektorem a Student entit

Při vytvoření webové stránky, které pracují s daty studenty a lektorem, můžete vycentrovat, aby `Person` sady entit a filtrovat na `HireDate` nebo `EnrollmentDate` vlastnost studenty nebo vyučující omezit vrácená data. Ale nyní při vytvoření vazby každého prvku zdroje dat a `Person` sady entit, můžete určit pouze `Student` nebo `Instructor` měla by být vybrána typy entit. Protože rozhraní Entity Framework umí rozlišit studenty a vyučující v `Person` sady entit, můžete odebrat `Where` nastavení vlastností, které jste zadali ručně, abyste to udělat.

V návrháři sady Visual Studio, můžete zadat typ entity, která `EntityDataSource` řízení měli vybrat v **EntityTypeFilter** rozevíracího pole `Configure Data Source` průvodce, jak je znázorněno v následujícím příkladu.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

A v **vlastnosti** okno můžete odebrat `Where` klauzule hodnoty, které již nejsou potřebné, jak je znázorněno v následujícím příkladu.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Ale vzhledem k tomu, že jste změnili značku `EntityDataSource` ovládacích prvků pro použití `ContextTypeName` atribut nelze spustit **konfigurace zdroje dat** Průvodce na `EntityDataSource` ovládacích prvků, které jste už vytvořili. Proto budete proveďte požadované změny změnou značek.

Otevřete *Students.aspx* stránky. V `StudentsEntityDataSource` řídit, odeberte `Where` atribut a přidejte `EntityTypeFilter="Student"` atribut. Kód bude nyní podobat následujícímu příkladu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Nastavení `EntityTypeFilter` atribut zajišťuje, že `EntityDataSource` řízení vybere pouze zadaný typ entity. Pokud chcete načíst i `Student` a `Instructor` typy entit, by tento atribut nastavíte. (Máte možnost načítání více typů entit s jedním `EntityDataSource` řídit jenom v případě, že používáte ovládací prvek pro přístup k datům jen pro čtení. Pokud používáte `EntityDataSource` řízení vložit, aktualizovat nebo odstranit entity a pokud sadu entit, která je vázána na mohou obsahovat více typů, je možné pracovat pouze s typem jednu entitu a budete muset nastavit tento atribut.)

Opakujte postup pro `SearchEntityDataSource` řídit, s výjimkou odebrat jenom část `Where` atribut, který vybere `Student` entity místo úplně odebrat vlastnost. Otevírací značce ovládacího prvku bude nyní vypadat podobně jako v následujícím příkladu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Spusťte stránku a ověřte, že je stále funguje stejně jako předtím.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Aktualizovat tyto stránek, které jste vytvořili v předchozích kurzy tak, aby využívaly nové `Student` a `Instructor` entity místo `Person` entity, pak je k ověření, že fungují stejně jako před spustit:

- V *StudentsAdd.aspx*, přidejte `EntityTypeFilter="Student"` k `StudentsEntityDataSource` ovládacího prvku. Kód bude nyní podobat následujícímu příkladu: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- V *About.aspx*, přidejte `EntityTypeFilter="Student"` k `StudentStatisticsEntityDataSource` řízení a odebrat `Where="it.EnrollmentDate is not null"`. Kód bude nyní podobat následujícímu příkladu: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- V *Instructors.aspx* a *InstructorsCourses.aspx*, přidejte `EntityTypeFilter="Instructor"` k `InstructorsEntityDataSource` řízení a odebrat `Where="it.HireDate is not null"`. Kód v *Instructors.aspx* nyní podobá následujícímu příkladu: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Kód v *InstructorsCourses.aspx* bude nyní podobat následujícímu příkladu:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

V důsledku těchto změn jste vylepšené aplikace Contoso univerzity udržovatelnosti několika způsoby. Jste přesunout logiku pro výběr a ověření mimo vrstvě uživatelského rozhraní (*.aspx* značek) a je nedílnou součástí vrstva přístupu k datům. To pomáhá izolovat od změny, které můžete provést v budoucnu schéma databáze nebo datový model kódu aplikace. Například může rozhodnout, že studenty může být přijat jako pomůcek učitele a proto by získat datum přijetí. Potom můžete přidat novou vlastnost rozlišit studenty z vyučující a aktualizovat data modelu. Žádný kód ve webové aplikaci bude nutné změnit s výjimkou případů, kde chcete zobrazit datum přijetí pro studenty. Další výhodou přidání `Instructor` a `Student` entity je, že je váš kód více snadno nerozumí než kdy ji uvedené `Person` objekty, které byly ve skutečnosti studenty nebo vyučující.

Nyní jste viděli jeden způsob, jak implementovat dědičnosti vzor v rozhraní Entity Framework. V následujícím kurzu dozvíte, jak používat uložené procedury, chcete-li mít větší kontrolu nad jak rozhraní Entity Framework přistupuje k databázi.

> [!div class="step-by-step"]
> [Předchozí](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [další](the-entity-framework-and-aspnet-getting-started-part-7.md)
