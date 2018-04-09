---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Začínáme s databáze Entity Framework 4.0 nejprve a ASP.NET 4 webové formuláře – část 3 | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET používající rozhraní Entity Framework. Vzorová aplikace je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 654f3556af5d05ec186e1811421966bbaffd2e21
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Začínáme s databáze Entity Framework 4.0 nejprve a 4 webových formulářů ASP.NET - část 3
====================
Podle [tní Dykstra](https://github.com/tdykstra)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET pomocí sady Visual Studio 2010 a Entity Framework 4.0. Informace o kurzu řady najdete v tématu [první kurz v řadě](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filtrování, řazení a seskupování dat

V předchozích kurzu jste použili `EntityDataSource` ovládací prvek zobrazit a upravit data. V tomto kurzu budete filtrování, řazení a seskupit data. Když to provedete nastavením vlastnosti `EntityDataSource` řízení, se liší od jiných ovládacích prvků zdroje dat syntaxe. Jak zjistíte, ale můžete použít `QueryExtender` ovládací prvek pro minimalizaci tyto rozdíly.

Změníte *Students.aspx* stránky pro filtrování pro studenty, řazení podle názvu a vyhledávání na název. Budete také změnit *Courses.aspx* stránky zobrazující kurzy pro vybrané oddělení a vyhledejte kurzy podle názvu. Nakonec student statistické údaje, které přidáte *About.aspx* stránky.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Pomocí EntityDataSource "Kde" vlastnosti k filtrování dat

Otevřete *Students.aspx* stránku, kterou jste vytvořili v předchozí kurzu. Podle současné konfigurace `GridView` ovládacího prvku stránce zobrazuje všechny názvy z `People` sady entit. Ale chcete zobrazit jenom studenty, které můžete najít tak, že vyberete `Person` entity, které mají data registrace nesmí být nulová.

Přepnout na **návrhu** zobrazit a vybrat `EntityDataSource` ovládacího prvku. V **vlastnosti** nastavte `Where` vlastnost `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

Syntaxe použijete v `Where` vlastnost `EntityDataSource` řízení je Entity SQL. Entita SQL je podobná Transact-SQL, ale je přizpůsobené pro použití s entity, nikoli databázové objekty. Ve výrazu `it.EnrollmentDate is not null`, slovo `it` představuje odkaz na entitu vrácených dotazem. Proto `it.EnrollmentDate` odkazuje `EnrollmentDate` vlastnost `Person` entity, `EntityDataSource` řízení vrátí.

Spuštění stránky. Studenti, kteří seznamu nyní obsahuje pouze studenty. (Nejsou zjištěny žádné řádky, kde se zobrazí datum registrace.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Pomocí vlastnosti EntityDataSource "OrderBy" pořadí dat

Také budete chtít tento seznam, když se napřed zobrazí jako v pořadí název. S *Students.aspx* stránky stále otevřen v **návrhu** zobrazení a `EntityDataSource` ovládací prvek vybrán, v **vlastnosti** okno sady  **OrderBy** vlastnost `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Spuštění stránky. Studenti, kteří seznamu je nyní v pořadí podle příjmení.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Pomocí řízení parametru set pro vlastnost "Kde"

Jako u jiných ovládacích prvků zdroje dat můžete předat hodnot parametrů, které `Where` vlastnost. Na *Courses.aspx* stránka, kterou jste vytvořili v části 2 kurzu, tuto metodu můžete použít k zobrazení kurzy, které jsou přidruženy oddělení, které uživatel vybere ze seznamu rozevíracího seznamu.

Otevřete *Courses.aspx* a přepněte do **návrhu** zobrazení. Přidejte druhý `EntityDataSource` řízení na stránku a pojmenujte ji `CoursesEntityDataSource`. Propojte jej s `SchoolEntities` modelu a vyberte `Courses` jako **EntitySetName** hodnotu.

V **vlastnosti** okně klikněte na tlačítko se třemi tečkami v **kde** pole vlastnosti. (Zajistěte, aby `CoursesEntityDataSource` před použitím je stále vybraný ovládací prvek **vlastnosti** okno.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**Editor výrazů** se zobrazí dialogové okno. V tomto dialogovém okně vyberte **automaticky generovat Where výraz podle parametrů**a potom klikněte na **přidat parametr**. Název parametru `DepartmentID`, vyberte **řízení** jako **Zdroj parametru** hodnotu a vyberte **DepartmentsDropDownList** jako **ControlID**  hodnotu.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Klikněte na tlačítko **zobrazit rozšířené vlastnosti**a v **vlastnosti** okno **Editor výrazů** dialogové okno, změny `Type` vlastnost `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Když jste hotovi, klikněte na tlačítko **OK**.

Pod rozevíracím seznamu, přidejte `GridView` řízení na stránku a pojmenujte ji `CoursesGridView`. Propojte jej s `CoursesEntityDataSource` zdroje dat ovládacího prvku, klikněte na tlačítko **aktualizovat schéma**, klikněte na tlačítko **upravit sloupce**a odeberte `DepartmentID` sloupce. `GridView` Značky ovládacího prvku se podobá následujícímu příkladu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Když uživatel změní vybraného oddělení v rozevíracím seznamu, budete chtít seznam přidružených kurzy změna automaticky. Chcete udělat, vyberte v rozevíracím seznamu a v **vlastnosti** sadu okno `AutoPostBack` vlastnost `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Teď, když jste hotovi, pomocí návrháře, přepněte do **zdroj** zobrazení a nahraďte `ConnectionString` a `DefaultContainer` název vlastnosti `CoursesEntityDataSource` řízení s `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atribut. Když jste hotovi, zápis pro ovládací prvek bude vypadat jako v následujícím příkladu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Spuštění stránky a pomocí rozevíracího seznamu vyberte různá oddělení. Pouze kurzy, které nabízí vybrané oddělení se zobrazují v `GridView` ovládacího prvku.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Pomocí vlastnosti EntityDataSource "GroupBy" k seskupení dat

Předpokládejme, že univerzity Contoso chce statistikami student textu na stránce s jejím o. Konkrétně chce zobrazit rozpis počtu studenty podle data, kdy je zaregistrovat.

Otevřete *About.aspx*a v **zdroj** zobrazit, nahradit existující obsah `BodyContent` řízení s "Statistika Student text" mezi `h2` značky:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Za záhlaví, přidejte `EntityDataSource` řízení a pojmenujte ji `StudentStatisticsEntityDataSource`. Propojte jej s `SchoolEntities`, vyberte `People` entity nastavit a nechat **vyberte** pole v Průvodci beze změny. Nastavte následující vlastnosti **vlastnosti** okno:

- Chcete-li filtrovat pro studenty pouze, nastavte `Where` vlastnost `it.EnrollmentDate is not null`.
- Chcete-li seskupit výsledky podle data registrace, nastavte `GroupBy` vlastnost `it.EnrollmentDate`.
- Chcete-li vybrat datum registrace a počet studenty, nastavte `Select` vlastnost `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Chcete-li k datu registrace řazení výsledků, nastavte `OrderBy` vlastnost `it.EnrollmentDate`.

V **zdroj** zobrazení, nahraďte `ConnectionString` a `DefaultContainer` název vlastnosti `ContextTypeName` vlastnost. `EntityDataSource` Značky ovládacího prvku nyní podobá následujícímu příkladu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

Syntaxe `Select`, `GroupBy`, a `Where` vlastnosti se podobá Transact-SQL s výjimkou `it` klíčové slovo, které určuje aktuální entity.

Přidejte následující kód k vytvoření `GridView` ovládací prvek zobrazí data.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Spuštění stránky pro zobrazení seznamu zobrazuje počet studenty datu registrace.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Použití ovládacího prvku QueryExtender pro filtrování a řazení

`QueryExtender` Řízení poskytuje způsob, jak zadat filtrování a řazení v kódu. Syntaxe je nezávislé na používáte systému správy databáze (databázového systému). Je také obecně nezávislé na rozhraní Entity Framework, s tím rozdílem, že syntaxe, které můžete použít pro navigační vlastnosti je jedinečné pro rozhraní Entity Framework.

V této části kurzu budete používat `QueryExtender` řízení filtrovat a řadit data a jedno pole Order bude navigační vlastnost.

(Pokud byste radši chtěli použít kód místo značek rozšířit dotazy, které jsou automaticky generované `EntityDataSource` ovládací prvek, můžete to udělat zpracování `QueryCreated` událostí. Jedná se jak `QueryExtender` ovládací prvek rozšiřuje `EntityDataSource` také řídit dotazy.)

Otevřete *Courses.aspx* stránky a pod kód, který jste přidali dříve, vložte následující kód k vytvoření záhlaví, textové pole pro zadání hledání řetězců, tlačítko vyhledat a `EntityDataSource` ovládací prvek, který je vázána `Courses` sady entit.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Všimněte si, že `EntityDataSource` ovládacího prvku `Include` je nastavena na `Department`. V databázi `Course` tabulka neobsahuje název oddělení; obsahuje `DepartmentID` sloupec cizího klíče. Pokud byly dotazuje databázi přímo, získat název oddělení spolu s daty kurzu, budete muset připojit `Course` a `Department` tabulky. Nastavením `Include` vlastnost `Department`, určíte, že rozhraní Entity Framework musí provádět práci při získávání související `Department` entity při získá `Course` entity. `Department` Entity je pak uloženy v `Department` vlastnost navigace `Course` entity. (Ve výchozím nastavení, `SchoolEntities` třídu, která byla generována pomocí návrháře dat modelu související data načte, když je to potřeba, a je vázaný prvek zdroje dat do třídy, takže nastavení `Include` vlastnost není nutné. Však jeho nastavení zlepšuje výkon stránky, protože jinak rozhraní Entity Framework by samostatné volání do databáze k načtení dat `Course` entity a související `Department` entity.)

Po `EntityDataSource` řízení, které jste právě vytvořili, vložte následující kód k vytvoření `QueryExtender` ovládací prvek, který je vázaný na který `EntityDataSource` ovládacího prvku.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression` Element určuje, zda chcete vybrat kurzy, jejichž názvy odpovídají hodnotě zadané v textovém poli. Pouze podle počtu znaků se zadávají do textového pole budou porovnány, protože `SearchType` určuje vlastnost `StartsWith`.

`OrderByExpression` Element určuje, že sadu výsledků dotazu bude seřazené podle kurzu název v rámci název oddělení. Všimněte si, jak je zadán název oddělení: `Department.Name`. Protože přidružení mezi `Course` entity a `Department` entita je 1: 1, `Department` obsahuje navigační vlastnost `Department` entity. (Pokud to byly vztah jeden mnoho, vlastnost by obsahuje kolekci.) Chcete-li získat název oddělení, musíte zadat `Name` vlastnost `Department` entity.

Nakonec přidejte `GridView` prvek, který zobrazí seznam kurzů:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

První sloupec je šablona pole, které zobrazuje název oddělení. Určuje výraz datové vazby `Department.Name`, stejně jako jste viděli v `QueryExtender` ovládacího prvku.

Spuštění stránky. Počáteční zobrazení zobrazuje seznam všech kurzů v pořadí podle oddělení a poté podle názvu kurzu.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Zadejte "m" a klikněte na **vyhledávání** zobrazíte všechny kurzy, jejichž názvy začínají řetězcem "m" (hledání není případ citlivé).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Pomocí operátoru "Jako" k filtrování dat

Můžete dosáhnout podobný vliv `QueryExtender` ovládacího prvku `StartsWith`, `Contains`, a `EndsWith` hledat typy pomocí `Like` operátor `EntityDataSource` ovládacího prvku `Where` vlastnost. V této části kurzu, uvidíte, jak používat `Like` operátor hledání student podle názvu.

Otevřete *Students.aspx* v **zdroj** zobrazení. Po `GridView` řídit, přidejte následující kód:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Tento kód je podobná co jste se seznámili s dříve s výjimkou `Where` hodnotu vlastnosti. Druhá část `Where` výraz definuje dílčí řetězec hledání (`LIKE %FirstMidName% or LIKE %LastName%`), vyhledá první a poslední názvy pro ať je zadána v textovém poli.

Spuštění stránky. Původně se všechny na studentů zobrazit, protože výchozí hodnota `StudentName` parametr je "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Do textového pole zadejte písmeno "g" a klikněte na **vyhledávání**. Zobrazí seznam studenty, kteří mají "g" buď jméno nebo příjmení.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Jste nyní zobrazí, aktualizovat, filtrovat, řazení a seskupené dat z jednotlivých tabulek. V dalším kurzu budete začít pracovat s souvisejících dat (scénáře hlavní podrobné).

> [!div class="step-by-step"]
> [Předchozí](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [další](the-entity-framework-and-aspnet-getting-started-part-4.md)
