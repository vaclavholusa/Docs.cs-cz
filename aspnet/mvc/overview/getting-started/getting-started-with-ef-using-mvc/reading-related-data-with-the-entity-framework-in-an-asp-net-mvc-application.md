---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Čtení dat souvisejících s platformou Entity Framework v aplikaci ASP.NET MVC | Microsoft Docs"
author: tdykstra
description: /AJAX/tutorials/Using-AJAX-Control-Toolkit-Controls-and-Control-Extenders-VB
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1f4912bb3113a8f9cdae4211e055a7e317ab2aff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Čtení související Data pomocí rozhraní Entity Framework v aplikaci ASP.NET MVC
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stáhnout PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio 2013. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


V předchozích kurz dokončit školní datový model. V tomto kurzu budete číst a zobrazení souvisejících dat – to znamená, data, která rozhraní Entity Framework se načte do navigační vlastnosti.

Následující ilustrace znázorňuje stránky, že bude fungovat s.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Opožděné, přes a explicitní načítání související Data

Aby rozhraní Entity Framework můžete načíst související data do navigační vlastnosti entity několika způsoby:

- *Opožděného načítání*. Když je nejdřív přečíst entity, není načíst související data. Ale při prvním pokusu o přístup k navigační vlastnost, lze data potřebná pro tuto navigační vlastnost je automaticky načte. To vede k více dotazů odeslal do databáze – jednu pro samotné entity a jeden musí načíst pokaždé, když související data pro entitu. `DbContext` Třída umožňuje opožděného načítání ve výchozím nastavení. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Přes načítání*. Při čtení je entita, související data načtena společně s jeho. To obvykle vede jednoho připojení dotaz, který načte všechna data, která je potřeba. Zadejte přes načítání pomocí `Include` metoda.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Explicitní načítání*. Toto je podobná opožděného načítání, s tím rozdílem, že je explicitně načíst související data v kódu; není provedena automaticky při přístupu k navigační vlastnosti. Ručně načíst související data tím, že získáme položka správce stavu objektu entity a volání [Collection.Load](https://msdn.microsoft.com/en-us/library/gg696220(v=vs.103).aspx) metoda pro kolekce nebo [Reference.Load](https://msdn.microsoft.com/en-us/library/gg679166(v=vs.103).aspx) metoda pro vlastnosti, které mají jedna entita. (V následujícím příkladu, pokud chcete načíst správce navigační vlastnost, měli byste nahradit `Collection(x => x.Courses)` s `Reference(x => x.Administrator)`.) Obvykle byste použili explicitní načítání jenom v případě, že jste zapnuté opožděného načítání vypnout.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Vzhledem k tomu, že nemáte načíst okamžitě hodnoty vlastností, opožděného načítání a explicitní načítání jsou obě známou taky jako *odložené načítání*.

### <a name="performance-considerations"></a>Důležité informace o výkonu

Pokud víte, že potřebujete souvisejících dat pro každou entitu, načíst, přes načítání často nabízí nejlepší výkon, protože je obvykle efektivnější než samostatné dotazy pro každé entity načíst jediný dotaz odeslal do databáze. Předpokládejme například, v uvedených příkladech, že každé oddělení má deset související kurzy. Příklad přes načítání by způsobilo právě dotazu jedním (připojit) a jedinou odezvy do databáze. Příklady explicitní načítání a opožděného načítání obě výsledkem by 11 dotazy a 11 zpátečních cest k databázi. Navíc zpátečních cest k databázi jsou obzvláště škodlivé pro výkon při latence je vysoká.

Na druhé straně v některých případech je opožděného načítání efektivnější. Přes načítání může způsobit velmi složité spojení má být vygenerován, který SQL Server nemůže zpracovat efektivně. Nebo pokud budete potřebovat pro přístup k navigační vlastnosti entity jenom pro podmnožinu sadu entit se zpracování, opožděného načítání může lépe provést, protože přes načítání by načíst více dat, než budete potřebovat. Pokud je důležité výkon, je nejvhodnější pro testování výkonu obou směrech, aby bylo možné nejlepší volbou.

Opožděného načítání maskovat kód, který způsobuje problémy s výkonem. Kód, který neurčuje přes nebo explicitní načítání ale zpracovává velký objem entity a používá několik navigačních vlastností v každé iteraci například může být velmi neefektivní (z důvodu velký počet zpátečních cest k databázi). Aplikace, která provádí i v vývoj pomocí na místním serveru SQL může mít problémy s výkonem při přesunu do Azure SQL Database z důvodu vyšší latence a opožděného načítání. Profilace databázové dotazy s realistické testu zatížení vám pomůže určit, zda je příslušná opožděného načítání. Další informace najdete v části [Demystifying Entity Framework strategie: načítání souvisejících dat](https://msdn.microsoft.com/en-us/magazine/hh205756.aspx) a [pomocí rozhraní Entity Framework snížit latenci sítě do SQL Azure](https://msdn.microsoft.com/en-us/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Zakázat opožděného načítání před serializace

Pokud necháte opožděného načítání povoleno během serializace, můžou skončit dotazování podstatně víc dat, než jste zamýšleli. Serializace se obecně funguje tak, že každé vlastnosti na instanci typu. Přístup k vlastnosti aktivuje opožděného načítání a tyto opožděné načíst entity se serializují. Proces serializace následně přistupuje k každou vlastnost opožděné načíst entit, potenciálně způsobuje i další opožděného načítání a serializace. Pokud chcete zabránit tento run-away řetězovou reakci, zapněte opožděného načítání vypnout před serializovat entity.

Serializace můžete také být ztěžuje třídy proxy, které používá rozhraní Entity Framework, jak je popsáno v [pokročilé scénáře kurzu](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Jedním ze způsobů se chcete vyhnout potížím serializace je serializovat objekty přenos dat (DTOs) namísto objekty entity, jak je znázorněno [pomocí webového rozhraní API s platformou Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) kurzu.

Pokud nepoužijete DTOs, můžete zakázat opožděného načítání a předešli problémům s proxy podle [zakázání vytváření proxy](https://msdn.microsoft.com/en-US/data/jj592886.aspx).

Tady jsou některé jiné [způsoby, jak zakázat opožděného načítání](https://msdn.microsoft.com/en-US/data/jj574232):

- Pro konkrétní navigační vlastnosti, vynechejte `virtual` – klíčové slovo deklarovat vlastnost.
- Pro všechny vlastnosti navigace, nastavte `LazyLoadingEnabled` k `false`, vložte následující kód v konstruktoru vaší třídy kontextu: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>Vytvořte stránku kurzy tento název zobrazí oddělení

`Course` Entity zahrnuje navigační vlastnost, která obsahuje `Department` entity přiřazený během oddělení. Zobrazí se název přiřazené oddělení v seznamu kurzů, které je potřeba získat `Name` vlastnost z `Department` entita, která je v `Course.Department` navigační vlastnost.

Vytvoříte řadič s názvem `CourseController` (ne CoursesController) pro `Course` pomocí stejných možností pro typ entity **kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework** scaffolder, který jste dříve pro `Student` řadič, jak je znázorněno na následujícím obrázku:

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Otevřete *Controllers\CourseController.cs* a podívejte se na `Index` metoda:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Automatické generování uživatelského rozhraní má zadanou přes načítání pro `Department` navigační vlastnost s použitím `Include` metoda.

Otevřete *Views\Course\Index.cshtml* a nahraďte kód šablony s následujícím kódem. Změny se zvýrazněnou:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Automaticky generovaný kód, které jste udělali následující změny:

- Změnit záhlaví z **Index** k **kurzy**.
- Přidat **číslo** sloupec, který ukazuje `CourseID` hodnotu vlastnosti. Ve výchozím nastavení nejsou vygeneroval primární klíče, protože jsou obvykle smysl pro koncové uživatele. Ale v takovém případě má smysl primární klíč a chcete ji zobrazit.
- Přesunout **oddělení** sloupce na pravé straně a změnit jeho záhlaví. Scaffolder správně zvolili zobrazíte `Name` vlastnost z `Department` entity, ale tady na stránce kurzu musí být na záhlaví sloupce **oddělení** místo **název**.

Všimněte si, že ve sloupci oddělení automaticky generovaný kód zobrazuje `Name` vlastnost `Department` entity, který je načten do `Department` navigační vlastnost:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Spuštění stránky (vyberte **kurzy** karta na domovské stránce univerzity Contoso) Chcete-li zobrazit seznam s názvy oddělení.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Vytvořit stránku vyučující, která zobrazuje kurzy a registrace

V této části vytvoříte řadič a zobrazit `Instructor` entity, aby bylo možné zobrazit stránku vyučující:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Tato stránka načte a zobrazí související data následujícími způsoby:

- Zobrazí seznam vyučující souvisejících dat z `OfficeAssignment` entity. `Instructor` a `OfficeAssignment` entity jsou ve vztahu-nula nebo 1. Budete používat pro přes načítání `OfficeAssignment` entity. Jak je popsáno výše, je obvykle efektivnější přes načítání, když potřebujete související data pro všechny načtené řádky v primární tabulce. V takovém případě budete chtít zobrazit přiřazení office pro všechny zobrazené vyučující.
- Když uživatel vybere lektorem související `Course` entity jsou zobrazeny. `Instructor` a `Course` jsou entit v relaci m: n. Budete používat pro přes načítání `Course` entity a jejich související `Department` entity. Opožděného načítání v takovém případě může být efektivnější, protože potřebujete jenom pro vybrané lektorem kurzy. Však tento příklad ukazuje způsob použití přes načítání pro navigační vlastnosti v rámci entit, které samy navigační vlastnosti.
- Když uživatel vybere kurzu, související data z `Enrollments` se zobrazí sada entit. `Course` a `Enrollment` entity jsou v vztah jeden mnoho. Bude potřeba přidat explicitní načítání pro `Enrollment` entity a jejich související `Student` entity. (Explicitní načítání není nutný, protože opožděného načítání je povoleno, ale to ukazuje, jak provést explicitní načítání.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Vytvoření modelu zobrazení pro zobrazení indexu lektorem

Vyučující stránka zobrazuje tři různých tabulek. Proto vytvoříte zobrazení model, který zahrnuje tři vlastnosti každého která uchovává data pro jednu z tabulek.

V *ViewModels* složku vytvořit *InstructorIndexData.cs* a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Vytvoření řadiče lektorem a zobrazení

Vytvoření `InstructorController` (ne InstructorsController) kontroler s akcemi čtení/zápisu EF, jak je znázorněno na následujícím obrázku:

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Otevřete *Controllers\InstructorController.cs* a přidejte `using` příkaz pro `ViewModels` obor názvů:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Automaticky generovaný kód `Index` metoda určuje přes načítání pouze pro `OfficeAssignment` navigační vlastnost:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Nahraďte `Index` metoda s následující kód pro načtení další související dat a umístit ho v modelu zobrazení:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Metoda přijímá data volitelné trasy (`id`) a parametr řetězce dotazu (`courseID`), zadejte hodnoty ID vybrané lektorem a vybraný kurz a předá všechna požadovaná data k zobrazení. Parametry jsou poskytovány **vyberte** hypertextové odkazy na stránce.

Vytvoření instance modelu zobrazení a vložení v něm seznam vyučující začne kód. Určuje kód přes načítání pro `Instructor.OfficeAssignment` a `Instructor.Courses` navigační vlastnost.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

Druhý `Include` metoda načte kurzy a pro každou kurz, který je načten nemá přes načítání `Course.Department` navigační vlastnost.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Jak je uvedeno nahoře, přes načítání není povinný, ale je potřeba pro zlepšení výkonu. Vzhledem k tomu, že zobrazení vždycky vyžaduje, aby `OfficeAssignment` entita, je efektivnější načíst, ve stejném dotazu. `Course`entity jsou požadovány, pokud je vybrána lektorem na webové stránce, takže přes načítání je lepší, než opožděného načítání jenom v případě, že stránka se zobrazí více často ke kurzu vybrané než bez.

Pokud jste vybrali ID lektorem, vybrané lektorem se načítají ze seznamu vyučující v zobrazení modelu. Model zobrazení `Courses` vlastnost je pak načten pomocí `Course` entity z této lektorem `Courses` navigační vlastnost.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`Where` Metoda vrátí kolekci, ale v takovém případě kritéria předaný výsledek této metody pouze do jedné `Instructor` nevrátila entity. `Single` Metoda převede kolekci do jednoho `Instructor` entity, která umožňuje přístup k dané entity `Courses` vlastnost.

Můžete použít [jeden](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.single.aspx) metoda na kolekci, když víte kolekce budou mít jen jednu položku. `Single` Metoda vyvolá výjimku, pokud je kolekce do ní předán prázdný nebo pokud existuje více než jednu položku. Alternativou je [SingleOrDefault](https://msdn.microsoft.com/en-us/library/bb342451.aspx), která vrací výchozí hodnotu (`null` v tomto případě) Pokud je kolekce prázdná. Ale v takovém případě stále vznikly by výjimku (z pokusu o vyhledání `Courses` vlastnost `null` odkaz), a zpráva o výjimce by méně jasně ukazovat na příčinu problému. Při volání `Single` metodu, můžete také předat v `Where` podmínku namísto volání `Where` metoda samostatně:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Namísto:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Dále pokud jste vybrali kurzu, vybraný kurz se načítají seznam kurzů ve model zobrazení. Potom zobrazení modelu `Enrollments` vlastnosti je načtena s `Enrollment` entity z tohoto kurzu `Enrollments` navigační vlastnost.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Upravit zobrazení indexu lektorem

V *Views\Instructor\Index.cshtml*, nahraďte kód šablony s následujícím kódem. Změny se zvýrazněnou:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Stávající kód, které jste udělali následující změny:

- Změnit třídu modelu k `InstructorIndexData`.
- Změnit název stránky z **Index** k **vyučující**.
- Přidat **Office** sloupec, který zobrazuje `item.OfficeAssignment.Location` pouze v případě `item.OfficeAssignment` není null. (Protože jedná se o relaci-nula nebo 1, nemusí být související `OfficeAssignment` entity.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Přidaného kódu, který dynamicky přidá `class="success"` k `tr` element vybrané lektorem. Toto nastaví barvu pozadí vybraného řádku s využitím [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) třídy. 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Přidat novou `ActionLink` s názvem bez přípony **vyberte** bezprostředně před další odkazy v každém řádku, což způsobí, že ID vybrané lektorem k odeslání `Index` metoda.

Spusťte aplikaci a vyberte **vyučující** kartě. Na stránce se zobrazuje `Location` vlastnost související `OfficeAssignment` entity a prázdná tabulka buňky při žádné související `OfficeAssignment` entity.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

V *Views\Instructor\Index.cshtml* soubor po zavření `table` – element (na konci souboru), přidejte následující kód. Tento kód zobrazí seznam kurzů související s lektorem, pokud je vybrána lektorem.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Tento kód čte `Courses` vlastnost modelu zobrazení zobrazíte seznam kurzů. Poskytuje také `Select` hypertextový odkaz, který odesílá ID vybraný kurz k `Index` metody akce.

Spuštění stránky a vyberte lektorem. Nyní se zobrazí v mřížce, která zobrazuje kurzy přiřazen k vybrané lektorem a pro každou kurz zobrazí název přiřazené oddělení.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Za blok kódu, který jste právě přidali přidejte následující kód. Zobrazí seznam studenty, kteří jsou zaregistrované v kurzu při výběru tohoto kurzu.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Tento kód čte `Enrollments` vlastnost modelu zobrazení, aby bylo možné zobrazit seznam studenty zařazen do kurzu.

Spuštění stránky a vyberte lektorem. Pak vyberte kurzu chcete zobrazit seznam registrovaných studenty a jejich tříd.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>Přidání explicitní načítání

Otevřete *InstructorController.cs* a podívejte se na jak `Index` metoda získá seznam registrace pro vybraný kurz:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Pokud jste získali seznam vyučující, jste zadali přes načítání pro `Courses` navigační vlastnost a `Department` vlastnosti každého kurzu. Pak můžete umístit `Courses` kolekci v modelu zobrazení, a teď chcete získat přístup `Enrollments` navigační vlastnost z jedné entity v dané kolekci. Protože jste neurčili přes načítání pro `Course.Enrollments` navigační vlastnost, data z této vlastnosti se zobrazí na stránce v důsledku opožděného načítání.

Pokud jste zakázali opožděného načítání beze změny kódu jiným způsobem, `Enrollments` vlastnost by bez ohledu na to, kolik registrace během ve skutečnosti měl hodnotu null. V takovém případě se načíst `Enrollments` vlastnost, bude potřeba zadat buď přes načítání nebo explicitní načítání. Seznámili jste už postup přes načítání. Chcete-li zobrazit příklad explicitní načítání, nahraďte `Index` metoda následující kód, který načte explicitně `Enrollments` vlastnost. Jsou vyznačené kód změnit.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Po získání vybraný `Course` entity, nový kód načte explicitně tohoto kurzu `Enrollments` navigační vlastnost:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Pak explicitně načte každý `Enrollment` týkající entity `Student` entity:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Všimněte si, že používáte `Collection` metoda načíst vlastnost kolekce, ale pro vlastnost, která obsahuje pouze jednu entitu, můžete použít `Reference` metoda.

Nyní spusťte lektorem indexovou stránku a uvidíte žádné rozdíly v co se zobrazí na stránce, i když jste změnili, jak jsou načtena data.

## <a name="summary"></a>Souhrn

Nyní jste použili všechny tři způsoby (opožděné, přes a explicitní) a načte související data do navigační vlastnosti. V dalším kurzu dozvíte, jak aktualizovat data v relaci.

Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit. Můžete také vyžádat nová témata na [zobrazit mi jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Odkazy na další zdroje Entity Framework najdete v [přístup k datům ASP.NET - doporučené prostředky](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Předchozí](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[další](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
