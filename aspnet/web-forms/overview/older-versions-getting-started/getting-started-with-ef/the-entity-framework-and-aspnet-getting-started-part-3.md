---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Začínáme s Entity Framework 4.0 Database First a technologie ASP.NET 4 webových formulářů – část 3 | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Frameworku. Ukázková aplikace je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 51964f86e66e21d63105f1043641c0096d1d41b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391144"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Začínáme s Entity Framework 4.0 Database First a 4 webových formulářů ASP.NET – část 3
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Framework 4.0 a Visual Studio 2010. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filtrování, řazení a seskupování dat

V předchozím kurzu jste použili `EntityDataSource` ovládacího prvku můžete zobrazit a upravit data. V tomto kurzu budete filtrování, řazení a seskupení dat. Když to uděláte to tak, že nastavíte vlastnosti `EntityDataSource` ovládací prvek, se liší od jiné ovládací prvky zdroje dat syntaxe. Jak uvidíte, ale můžete použít `QueryExtender` ovládací prvek pro minimalizaci tyto rozdíly.

Změníte *Students.aspx* stránky k filtrování pro studenty, seřadit podle názvu a vyhledat název. Budete také změnit *Courses.aspx* stránka pro zobrazení kurzů pro vybranou oddělení a vyhledejte kurzy podle názvu. Nakonec přidejte studenty statistiky *About.aspx* stránky.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Používat EntityDataSource "Kde" vlastnost k filtrování dat

Otevřít *Students.aspx* stránku, kterou jste vytvořili v předchozím kurzu. Podle současné konfigurace `GridView` ovládací prvek na stránce zobrazí všechny názvy z `People` sady entit. Ale chcete zobrazit pouze studenti, které můžete vyhledat tak, že vyberete `Person` entity, které mají data registrace jinou hodnotu než null.

Přepnout na **návrhu** zobrazit a vybrat `EntityDataSource` ovládacího prvku. V **vlastnosti** okno, nastaveno `Where` vlastnost `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

Můžete použít v syntaxi `Where` vlastnost `EntityDataSource` Entity SQL je ovládací prvek. Entity SQL se podobá jazyku Transact-SQL, ale je tento příklad upravený pro použití s entitami, ne databázové objekty. Ve výrazu `it.EnrollmentDate is not null`, slovo `it` představuje odkaz na entitu vrácených dotazem. Proto `it.EnrollmentDate` odkazuje `EnrollmentDate` vlastnost `Person` entity, který `EntityDataSource` ovládací prvek vrátí.

Spuštění stránky. V seznamu students nyní obsahuje pouze studenti. (Nejsou zjištěné žádné řádky, ve kterém se zobrazí není žádné datum registrace.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Vlastnost "OrderBy" EntityDataSource pořadí dat

Chcete také tento seznam podle názvu se při prvním zobrazení. S *Students.aspx* stránky stále otevřen v **návrhu** zobrazení a `EntityDataSource` ovládací prvek vybrán, v **vlastnosti** okno sady  **Řadit podle** vlastnost `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Spuštění stránky. Seznam studentů je nyní v pořadí podle jména.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Pomocí parametru ovládací prvek nastavit vlastnost "Kde"

Jak se ostatní ovládací prvky zdroje dat, můžete předat hodnoty parametrů pro `Where` vlastnost. Na *Courses.aspx* stránce, kterou jste vytvořili v části 2 tohoto kurzu, tato metoda slouží k zobrazení kurzů, které jsou spojeny s oddělení, které uživatel vybere z rozevíracího seznamu.

Otevřít *Courses.aspx* a přepněte se na **návrhu** zobrazení. Přidejte druhý `EntityDataSource` ovládací prvek na stránce a pojmenujte ho `CoursesEntityDataSource`. Propojte jej s `SchoolEntities` model a vyberte `Courses` jako **EntitySetName** hodnotu.

V **vlastnosti** okna, klikněte na tlačítko se třemi tečkami v **kde** vlastnosti. (Ujistěte se, že `CoursesEntityDataSource` před použitím je stále vybrán ovládací prvek **vlastnosti** okna.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**Editor výrazů** se zobrazí dialogové okno. V tomto dialogovém okně vyberte **Where automaticky generovat výraz podle zadaných parametrů**a potom klikněte na tlačítko **přidat parametr**. Název parametru `DepartmentID`vyberte **ovládací prvek** jako **zdroji parametru** hodnotu a vyberte **DepartmentsDropDownList** jako **ControlID**  hodnotu.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Klikněte na tlačítko **zobrazit upřesňující vlastnosti**a **vlastnosti** okno **Editor výrazů** dialogovém okně Změnit `Type` vlastnost `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Jakmile budete hotovi, klikněte na tlačítko **OK**.

Pod rozevíracím seznamu, přidejte `GridView` na stránku ovládací prvky a pojmenovat ho `CoursesGridView`. Propojte jej s `CoursesEntityDataSource` zdroje dat ovládacího prvku, klikněte na tlačítko **aktualizovat schéma**, klikněte na tlačítko **upravit sloupce**a odeberte `DepartmentID` sloupce. `GridView` Značky ovládacího prvku se podobá následujícímu příkladu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Když uživatel změní oddělení vybrané v rozevíracím seznamu, budete chtít seznam související kurzy, které automaticky změnit. Aby to chcete udělat, vyberte rozevíracím seznamu a v **vlastnosti** okno sady `AutoPostBack` vlastnost `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Teď, když budete hotovi, pomocí návrháře, přepněte na **zdroj** zobrazení a nahraďte `ConnectionString` a `DefaultContainer` název vlastnosti `CoursesEntityDataSource` ovládacím prvkem `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atribut. Jakmile budete hotovi, značky pro ovládací prvek bude vypadat jako v následujícím příkladu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Spuštění stránky a pomocí rozevíracího seznamu vyberte různá oddělení. Jsou zobrazeny pouze kurzů, které nabízí vybraného oddělení v `GridView` ovládacího prvku.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Vlastnost "GroupBy" EntityDataSource k seskupení dat

Předpokládejme, že Contoso University chce statistikami student text na stránce o. Konkrétně chce zobrazit rozpis počtu studentů podle data, kdy je zaregistrovat.

Otevřít *About.aspx*a v **zdroj** zobrazení, nahraďte existující obsah `BodyContent` řízení s "Statistiky Student text" mezi `h2` značky:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Za záhlaví, přidejte `EntityDataSource` řídit a pojmenujte ho `StudentStatisticsEntityDataSource`. Propojte jej s `SchoolEntities`, vyberte `People` entity nastavit a dál necháte **vyberte** pole v Průvodci beze změny. Nastavte následující vlastnosti **vlastnosti** okno:

- Chcete-li filtrovat pouze studenti, nastavte `Where` vlastnost `it.EnrollmentDate is not null`.
- Seskupení výsledků podle data registrace, nastavte `GroupBy` vlastnost `it.EnrollmentDate`.
- Chcete-li vybrat datum registrace a počet studentů, nastavte `Select` vlastnost `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Řazení výsledků podle data registrace, nastavte `OrderBy` vlastnost `it.EnrollmentDate`.

V **zdroj** zobrazení, nahraďte `ConnectionString` a `DefaultContainer` název vlastnosti `ContextTypeName` vlastnost. `EntityDataSource` Značky ovládacího prvku teď vypadá podobně jako v následujícím příkladu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

Syntaxe `Select`, `GroupBy`, a `Where` vlastnosti se podobá jazyku Transact-SQL s výjimkou `it` klíčové slovo, které určuje aktuální entitu.

Přidejte následující kód k vytvoření `GridView` ovládací prvek pro zobrazení údajů.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Spustíte stránku, aby zobrazil seznam zobrazující počet studentů podle data registrace.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Použití ovládacího prvku QueryExtender pro filtrování a řazení

`QueryExtender` Řízení poskytuje způsob, jak určit filtrování a řazení v kódu. Syntaxe je nezávislé na použití systému správy databáze (DBMS). Také je obecně nezávisle na Entity Framework, s tím rozdílem, že je jedinečné pro Entity Framework syntaxi, kterou můžete použít pro navigační vlastnosti.

V této části kurzu budete používat `QueryExtender` ovládacího prvku na filtrovat a řadit data a jedno z polí klauzule order by budou navigační vlastnost.

(Pokud byste radši chtěli použít kód místo značek k rozšíření dotazy, které jsou automaticky generovány `EntityDataSource` ovládacího prvku, můžete to udělat zpracování `QueryCreated` událostí. Toto je způsob, jakým `QueryExtender` ovládací prvek rozšiřuje `EntityDataSource` také řídit dotazů.)

Otevřít *Courses.aspx* stránce a pod kód, který jste přidali dříve, vložte následující kód k vytvoření nadpisu, textovým polem pro zadání vyhledávacích řetězců, tlačítko hledání a `EntityDataSource` ovládací prvek, který je vázán `Courses` sady entit.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Všimněte si, `EntityDataSource` ovládacího prvku `Include` je nastavena na `Department`. V databázi `Course` tabulka neobsahuje název oddělení; obsahuje `DepartmentID` sloupec cizího klíče. Pokud se dotazuje databáze přímo, a získat tak název oddělení spolu s daty pro kurz, potřebujete připojit `Course` a `Department` tabulky. Nastavením `Include` vlastnost `Department`, určíte, že rozhraní Entity Framework by měl pracovat získání související `Department` entity, když se načte `Course` entity. `Department` Entity se pak ukládá v `Department` vlastnost navigace `Course` entity. (Ve výchozím nastavení, `SchoolEntities` třídu, která byla vygenerována návrháře modelů dat načte související data, když je to potřeba, a je vázaný ovládací prvek zdroje dat pro tuto třídu, takže nastavení `Include` vlastnost není nutné. Však nastavení zlepší výkon, stránky, protože jinak s Entity Framework žádným samostatná volání databáze k načtení dat `Course` entity a související `Department` entity.)

Po `EntityDataSource` ovládacího prvku, které jste právě vytvořili, vložte následující kód k vytvoření `QueryExtender` ovládací prvek, který je vázán na, který `EntityDataSource` ovládacího prvku.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression` Element určuje, že chcete k vybraným kurzům, jejichž názvy odpovídají hodnotě zadané v textovém poli. Pouze podle počtu znaků jsou zadány v textovém poli se bude porovnávat, protože `SearchType` vlastnost určuje `StartsWith`.

`OrderByExpression` Element určuje, že se sadu výsledků dotazu seřazené podle názvu kurzu v rámci název oddělení. Všimněte si, jak je zadán název oddělení: `Department.Name`. Protože přidružení mezi `Course` entity a `Department` entita je 1: 1, `Department` obsahuje navigační vlastnost `Department` entity. (Pokud to vztah jeden mnoho, vlastnost by obsahovat kolekce.) Pokud chcete získat název oddělení, je nutné zadat `Name` vlastnost `Department` entity.

Nakonec přidejte `GridView` ovládací prvek zobrazil seznam kurzů:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

První sloupec je šablona pole, která zobrazuje název oddělení. Určuje výraz datové vazby `Department.Name`, stejně jako jste viděli v `QueryExtender` ovládacího prvku.

Spuštění stránky. Počáteční zobrazení zobrazuje seznam všech kurzů v pořadí podle oddělení a poté podle názvu kurzu.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Zadejte "m" a klikněte na tlačítko **hledání** chcete zobrazit všechny kurzy, jejichž názvy začínají řetězcem "m" (hledání není případ citlivé).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Pomocí operátoru "Jako" k filtrování dat

Můžete dosáhnout účinku podobně jako `QueryExtender` ovládacího prvku `StartsWith`, `Contains`, a `EndsWith` vyhledejte typy pomocí `Like` operátor `EntityDataSource` ovládacího prvku `Where` vlastnost. V této části kurzu uvidíte, jak používat `Like` operátor vyhledání student podle názvu.

Otevřít *Students.aspx* v **zdroj** zobrazení. Po `GridView` řídit, přidejte následující kód:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Tento kód se podobá co jste viděli dříve s výjimkou `Where` hodnotu vlastnosti. Druhá část `Where` výraz definuje hledání dílčího řetězce (`LIKE %FirstMidName% or LIKE %LastName%`), která vyhledá první a poslední názvy pro všechno, co je zadaný v textovém poli.

Spuštění stránky. Zpočátku uvidíte všechny studenty protože výchozí hodnota `StudentName` je parametr "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Do textového pole zadejte písmeno "g" a klikněte na tlačítko **hledání**. Zobrazí seznam studentům, kteří mají "g" buď v prvním nebo posledním název.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Právě jste zobrazí, aktualizovat, filtrovat, uspořádané a seskupení dat z jednotlivých tabulek. V dalším kurzu vám začít pracovat se souvisejícími daty (hlavní podrobnosti scénáře).

> [!div class="step-by-step"]
> [Předchozí](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [další](the-entity-framework-and-aspnet-getting-started-part-4.md)
