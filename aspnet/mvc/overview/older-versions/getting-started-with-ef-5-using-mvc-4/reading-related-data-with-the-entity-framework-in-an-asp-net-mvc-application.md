---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Čtení souvisejících dat s Entity Framework v aplikaci ASP.NET MVC (5 10) | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje postup vytvoření aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a sady Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4767b015db0bad09942802827ce54162687fcabc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753234"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Čtení souvisejících dat s Entity Framework v aplikaci ASP.NET MVC (5 10)
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a Visual Studio 2012. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Série kurzů můžete začít od začátku nebo [stáhnout počáteční projekt pro tuto kapitolu](building-the-ef5-mvc4-chapter-downloads.md) a začněte tady.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte sami, [stáhnout dokončený kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a zkuste problém reprodukovat. Porovnáním kód Dokončený kód v obecně najdete řešení problému. Některé běžné chyby a jejich řešení najdete v tématu [chyby a náhradní řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V předchozím kurzu jste dokončili školní datového modelu. V tomto kurzu budete čtení a zobrazení souvisejících dat – to znamená, že data, která načte Entity Framework do navigační vlastnosti.

Na následujících obrázcích stránky, kterou budete pracovat.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Opožděné, nemůžou dočkat, až a explicitní načtení souvisejících dat

Existuje několik způsobů, Entity Framework mohou načíst související data do navigační vlastnosti entity:

- *Opožděné načtení*. Pokud entita je nejdřív přečíst, související data nebude načten. Ale při prvním pokusu o přístup k vlastnosti navigace data požadovaná pro tuto navigační vlastnost je automaticky načte. Výsledkem je více dotazy odeslané do databáze – jeden pro samotné entity a jeden pokaždé, když související data entity musí být načten. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Předběžné načítání*. Při čtení entity související data načíst společně. Obvykle v důsledku jednoho spojení dotaz, který zkopíruje všechna data, který je nezbytný. Předběžné načítání určíte pomocí `Include` metody.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Explicitní načtení*. To se podobá opožděné načtení, s tím rozdílem, že explicitně načíst související data v kódu. je tomu tak není automaticky při přístupu k vlastnosti navigace. Ruční načtení souvisejících dat tím, že získáme položka správce stavu objektu pro entitu a volání `Collection.Load` metodu pro kolekce nebo `Reference.Load` metodu pro vlastnosti, které obsahují jednu entitu. (V následujícím příkladu, pokud chcete načíst správce navigační vlastnost, by byly nahrazeny `Collection(x => x.Courses)` s `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Protože jejich není okamžitě hodnoty vlastností, opožděné načtení a explicitní načtení se obě označují jako *odložené načítání*.

Obecně platí Pokud víte, musíte souvisejících dat pro každou entitu, které načtené, nemůžou dočkat, až načítání nabízí nejlepší výkon, protože je obvykle efektivnější než samostatné dotazy pro každou entitu načíst jednoho dotazu odeslaného do databáze. Například ve výše uvedených příkladech předpokládejme, že každé oddělení má deset související kurzy. Předběžné načítání příklad způsobí pouze jednou (spojení) dotazu a jeden odezvy k databázi. Opožděné načtení a příklady explicitní načtení obě způsobí jedenáct dotazy a jedenáct zpátečních cest k databázi. Další zpátečních cest k databázi jsou zvlášť ztráty výkonu, když má vysokou latenci.

Na druhé straně v některých případech je efektivnější opožděné načtení. Předběžné načítání může vést k velmi složité spojení být vytvořen, kterou SQL Server nemůže zpracovat efektivně. Nebo pokud potřebujete přístup k entity navigační vlastnosti pouze pro dílčí sadu entit se zpracování, opožděné načtení může být lepší provést, protože nemůžou dočkat, až načítání by byl načten více dat, než potřebujete. Pokud je nejdůležitější výkon, je nejvhodnější pro testování výkonu oba způsoby, aby bylo možné správně se rozhodnout.

Obvykle můžete využít explicitní načtení jenom v případě, že aktivovali jsme opožděné načtení vypnout. Jeden scénář, když by měl zapnout opožděné načtení vypnutí se během serializace. Opožděné načtení a serializace Nekombinujte dobře, a pokud nevyloučíte, že můžete skončit dotazování podstatně víc dat než bylo zamýšleno při opožděné načtení je povolená. Serializace obecně funguje tak, že přístup k každou vlastnost v instanci typu. Přístup k vlastnostem aktivuje opožděné načtení a entit opožděné načíst serializují. Procesu serializace následně přistupuje k opožděné načtení entity potenciálně nezpůsobil další opožděné načtení a serializace se jednotlivé vlastnosti. Pokud chcete zabránit této run-away řetězové reakce, zapněte opožděné načtení vypnout před serializovat entity.

Třídy kontextu databáze provádí opožděné načtení ve výchozím nastavení. Existují dva způsoby, jak zakázat opožděné načtení:

- Pro konkrétní navigační vlastnosti vynechat, nechte `virtual` – klíčové slovo deklarovat vlastnost.
- Pro všechny vlastnosti navigace, nastavte `LazyLoadingEnabled` k `false`. Můžete například přidat následující kód v konstruktoru třídy kontextu: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Opožděné načtení může zastínit kód, který způsobuje problémy s výkonem. Kód, který nemá určenou nemůžou dočkat, až nebo explicitní načtení, ale zpracovává velký počet entit a používá několik vlastností navigace v každé iteraci například může být velmi neefektivní (z důvodu počet zpátečních cest k databázi). Aplikace, který provádí i při vývoji pomocí SQL serveru na místní může mít problémy s výkonem při přesunu do Azure SQL Database z důvodu vyšší latence a opožděné načtení. Profiluje dotazy databáze s realistické zkušební zatížení vám pomůže určit, pokud opožděné načtení je vhodné. Další informace najdete v části [uvedení Entity Framework strategie: načítání souvisejících dat](https://msdn.microsoft.com/magazine/hh205756.aspx) a [pomocí Entity Frameworku na snížení latence sítě do SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Vytvoření stránky indexu kurzy nejmenuje zobrazí oddělení

`Course` Entita obsahuje vlastnost navigace, která obsahuje `Department` entity, která je přiřazena celé oddělení. Chcete-li zobrazit název přiřazený oddělení v seznamu kurzů, je potřeba získat `Name` vlastnost z `Department` entity, která je v `Course.Department` navigační vlastnost.

Vytvoření řadiče s názvem `CourseController` pro `Course` typ entity, pomocí stejných možností, které jste provedli dříve pro `Student` řadič, jak je znázorněno na následujícím obrázku (s výjimkou, že na rozdíl od obrázku, je vaší context – třída v oboru názvů DAL, není modely oboru názvů):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Otevřít *Controllers\CourseController.cs* a podívejte se na `Index` metody:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Automatické generování uživatelského rozhraní má zadanou předběžné načítání pro `Department` navigační vlastnost s použitím `Include` metody.

Otevřít *Views\Course\Index.cshtml* a nahraďte existující kód následujícím kódem. Změny jsou zvýrazněny:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Automaticky generovaný kód, které jste udělali následující změny:

- Změnit záhlaví z **Index** k **kurzy**.
- Přesunout řádek odkazy na levé straně.
- Přidání sloupce pod záhlavím **číslo** , který ukazuje `CourseID` hodnotu vlastnosti. (Ve výchozím nastavení, primárních klíčů nejsou automaticky generovaný protože jsou obvykle nemá význam pro koncové uživatele. Ale v tomto případě má smysl primární klíč a chcete zobrazit.)
- Změnit na poslední záhlaví sloupce z **DepartmentID** (název cizího klíče `Department` entity) na **oddělení**.

Všimněte si, že posledního sloupce automaticky generovaný kód zobrazí `Name` vlastnost `Department` entity, který je načten do `Department` navigační vlastnost:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Spuštění stránky (vyberte **kurzy** kartu na domovské stránce Contoso University) zobrazíte seznam s názvy oddělení.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Vytvoření stránky Instruktoři Index, který ukazuje, kurzy a registrace

V této části vytvoříte řadič a zobrazit `Instructor` entitu, aby bylo možné zobrazit Instruktoři indexovou stránku:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Tato stránka načte a zobrazí související data následujícími způsoby:

- V seznamu instruktorů zobrazí související data z `OfficeAssignment` entity. `Instructor` a `OfficeAssignment` entity jsou ve vztahu k nule nebo jednom. Budete používat pro předběžné načítání `OfficeAssignment` entity. Jak jsme vysvětlili výše, předběžné načítání je obvykle mnohem efektivnější, když potřebujete související data pro všechny načtené řádky v tabulce primární. V takovém případě budete chtít zobrazit přiřazení office pro všechny zobrazené Instruktoři.
- Když uživatel vybere instruktorem, související `Course` entity jsou zobrazeny. `Instructor` a `Course` entity jsou v relaci m: m. Budete používat pro předběžné načítání `Course` entit a jejich související `Department` entity. Opožděné načtení v takovém případě může být mnohem efektivnější, protože potřebujete jenom pro vybrané kurzů vedených kurzů. Však tento příklad ukazuje způsob použití předběžné načítání pro navigační vlastnosti v rámci entity, které představují samy o sobě v navigační vlastnosti.
- Když uživatel vybere kurz, související data z `Enrollments` se zobrazí sada entit. `Course` a `Enrollment` entity jsou v vztah jeden mnoho. Přidejte explicitní načtení pro `Enrollment` entit a jejich související `Student` entity. (Explicitní načtení není nutný, protože je povolené opožděné načtení, ale to ukazuje, jak provést explicitní načtení.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Vytvoření modelu zobrazení pro zobrazení indexu instruktorem

Kurzů vedených indexovou stránku ukazuje třech různých tabulkách. Proto vytvoříte zobrazení modelu, který obsahuje tři vlastnosti každé obsahující data pro jednu z tabulek.

V *modely ViewModels* složku, vytvořte *InstructorIndexData.cs* a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Přidání styl pro vybrané řádky

Chcete-li označit vybrané řádky potřebujete jinou barvu pozadí. Snaží poskytnout styl pro toto uživatelské rozhraní, přidejte následující zvýrazněný kód do části `/* info and errors */` v *Content\Site.css*, jak je znázorněno níže:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Vytváření kurzů vedených kontroler a zobrazení

Vytvoření `InstructorController` řadič, jak je znázorněno na následujícím obrázku:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Otevřít *Controllers\InstructorController.cs* a přidejte `using` příkaz pro `ViewModels` obor názvů:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Automaticky generovaný kód v `Index` určuje předběžné načítání pouze pro metody `OfficeAssignment` navigační vlastnost:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Nahradit `Index` metody následující kód pro načtení další související data a vložit ho do modelu zobrazení:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Metoda přijímá data volitelné trasy (`id`) a parametru řetězce dotazu (`courseID`), zadejte hodnoty ID vybrané instruktorem a vybrané kurzu a splní všechna požadovaná data do zobrazení. Parametry jsou poskytovány **vyberte** hypertextové odkazy na stránky.

> [!TIP]
> 
> **Data trasy**
> 
> Data trasy, která jsou data, která v segment adresy URL zadané ve směrovací tabulce nalezen vazač modelu. Například výchozí trasa určuje `controller`, `action`, a `id` segmenty:
> 
> routes.MapRoute(  
>  Název: "Výchozí",  
>  Adresa URL: "{controller} / {action} / {id}",  
>  výchozí hodnoty: new {řadič = "Domů" action = "Index", id = UrlParameter.Optional}  
> );
> 
> V následující adrese URL, výchozí trasu mapuje `Instructor` jako `controller`, `Index` jako `action` a 1 stejně jako `id`; jde o hodnot dat trasy.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID = 2021" je hodnota řetězce dotazu. Vazač modelu budou fungovat i Pokud předáte `id` jako hodnotu řetězce dotazu:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Adresy URL jsou vytvořeny pomocí `ActionLink` příkazy v zobrazení Razor. V následujícím kódu `id` parametr odpovídá výchozí trasu, takže `id` se přidá do data trasy.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> V následujícím kódu `courseID` neodpovídá parametru v této výchozí trase, tak se přidá jako řetězec dotazu.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


Vytváření instance zobrazení modelu a jeho uvedením seznamu instruktorů začíná kód. Předběžné načítání pro Určuje kód `Instructor.OfficeAssignment` a `Instructor.Courses` navigační vlastnost.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

Druhá `Include` metoda načte kurzy a jednotlivých kurzů, který je načten funguje předběžné načítání pro `Course.Department` navigační vlastnost.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Jak už bylo zmíněno dříve, nemůžou dočkat, až načítání se nevyžadují, ale se provádí pro zvýšení výkonu. Protože zobrazení vždycky vyžaduje, aby `OfficeAssignment` entity, je efektivnější pro načtení, které ve stejném dotazu. `Course` entity jsou požadovány při výběru instruktorem na webové stránce, takže předběžné načítání je obecně lepší než opožděné načtení jenom v případě, že na stránce se zobrazí častěji kurzu vybrali než bez.

Pokud jste vybrali ID instruktorem, vybraných kurzů vedených je načten ze seznamu školitelů v modelu zobrazení. Model zobrazení `Courses` vlastnost je pak načten pomocí `Course` entity z této kurzů vedených `Courses` navigační vlastnost.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

`Where` Metoda vrátí kolekci, ale v tomto případě kritéria předat výsledek této metody v jedné `Instructor` entity se vrací. `Single` Metoda převede kolekci do jednoho `Instructor` entity, která umožňuje přístup k dané entity `Courses` vlastnost.

Můžete použít [jeden](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) metody na kolekci, když víte, kolekce budou mít pouze jednu položku. `Single` Metoda vyvolá výjimku, pokud do něho předaný kolekce je prázdná, nebo pokud existuje více než jednu položku. Alternativou je [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), která vrátí výchozí hodnotu (`null` v tomto případě) Pokud kolekce je prázdná. Ale v tomto případě bude stále výsledkem výjimku (z pokusu o nalezení `Courses` vlastnosti `null` odkaz), a zpráva o výjimce by méně jasně ukazovat na příčinu problému. Při volání `Single` metodu, můžete také předat `Where` podmínku namísto volání metody `Where` metoda samostatně:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Namísto:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Dále pokud jste vybrali kurz, vybrané kurzu se načte z seznam kurzů v zobrazení modelu. Potom zobrazení modelu `Enrollments` vlastnosti je načtena s `Enrollment` entity z tohoto kurzu `Enrollments` navigační vlastnost.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Úprava zobrazení indexu instruktorem

V *Views\Instructor\Index.cshtml*, nahraďte existující kód následujícím kódem. Změny jsou zvýrazněny:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Stávající kód, které jste udělali následující změny:

- Změnit třídu modelu `InstructorIndexData`.
- Změnil se název stránky z **Index** k **Instruktoři**.
- Přesunout odkaz sloupce řádku na levé straně.
- Odebrat **FullName** sloupce.
- Přidá **Office** sloupec, který zobrazuje `item.OfficeAssignment.Location` pouze tehdy, pokud `item.OfficeAssignment` nemá hodnotu null. (Protože je to vztah jeden: nula nebo 1, nemusí být se souvisejícím `OfficeAssignment` entity.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Přidání kódu, která dynamicky přidá `class="selectedrow"` k `tr` element vybraných kurzů vedených. Tím se nastaví barvu pozadí vybraného řádku horizontálních oddílů pomocí třídy CSS, který jste vytvořili dříve. ( `valign` Atribut bude hodit v následujícím kurzu přidáte sloupec více řádky v tabulce.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Přidat nový `ActionLink` označené jako **vyberte** bezprostředně před další odkazy na každém řádku, což způsobí, že ID vybraných kurzů vedených k odeslání do `Index` metoda.

Spusťte aplikaci a vyberte **Instruktoři** kartu. Na stránce se zobrazí `Location` související vlastnost `OfficeAssignment` entit a prázdné tabulky buňky při žádné související `OfficeAssignment` entity.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

V *Views\Instructor\Index.cshtml* soubor po zavření `table` – element (na konci souboru), přidejte následující zvýrazněný kód. Zobrazí se seznam kurzů související s instruktorem, pokud je vybrána instruktorem.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Tento kód čte `Courses` vlastnost model zobrazení zobrazíte seznam kurzů. Poskytuje také `Select` hypertextový odkaz, který odesílá ID vybrané kurz `Index` metody akce.

> [!NOTE]
> *.Css* soubor se uloží do mezipaměti prohlížeče. Pokud nevidíte změny při spuštění aplikace, proveďte aktualizaci pevný (podržte stisknutou klávesu CTRL při kliknutí **aktualizovat** tlačítko, nebo stisknutím kláves CTRL + F5).


Spustit na stránku a vybrat instruktorem. Nyní uvidíte tabulku, která zobrazuje kurzy přiřazen k vybrané instruktorem a jednotlivých kurzů se zobrazí název přiřazený oddělení.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Za blok kódu, který jste právě přidali přidejte následující kód. Zobrazí seznam studentů, kteří se zaregistrují v kurzu při výběru tohoto kurzu.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Tento kód čte `Enrollments` vlastnost model zobrazení, aby bylo možné zobrazit seznam studentů zaregistrovaný do kurzu.

Spustit na stránku a vybrat instruktorem. Vyberte kurzu zobrazíte seznam registrovaná studentů a jejich kvality.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Přidání explicitní načtení

Otevřít *InstructorController.cs* a podívejte se jak `Index` metoda načte seznam registrace pro vybrané kurzu:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Když jste získali seznam školitelů, jste zadali předběžné načítání pro `Courses` navigační vlastnost a `Department` vlastnost jednotlivých kurzů. Pak vložíte `Courses` kolekce v modelu zobrazení a nyní získáváte přístup k `Enrollments` navigační vlastnost z entit v této kolekci. Protože jste neurčili, nevložily předběžné načítání pro `Course.Enrollments` navigační vlastnost, data z dané vlastnosti se zobrazuje na stránce v důsledku opožděné načtení.

Pokud jste zakázali opožděné načtení beze změny kódu žádným jiným způsobem, `Enrollments` vlastnost by bez ohledu na to, kolik registrace na kurz ve skutečnosti měla hodnotu null. V takovém případě se načíst `Enrollments` vlastnost, je potřeba zadat předběžné načítání nebo explicitní načtení. Už jste viděli postup předběžné načítání. Chcete-li zobrazit příklad explicitní načtení, nahraďte `Index` metodu s následujícím kódem, který explicitně načte `Enrollments` vlastnost. Zvýrazní se kód změnil.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Po získání vybrané `Course` entity, nový kód explicitně načte tento kurz `Enrollments` navigační vlastnost:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Pak explicitně načte každý `Enrollment` týkající entity `Student` entity:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Všimněte si, že používáte `Collection` metoda načíst vlastnost kolekce, ale pro vlastnost, která obsahuje pouze jednu entitu, můžete použít `Reference` metody. Kurzů vedených indexovou stránku můžete nyní spustit a zobrazí se vám nijak neliší obsah zobrazený na stránce, i když jste změnili, jak načíst data.

## <a name="summary"></a>Souhrn

Nyní využili jste všechny tři způsoby, jak (opožděné, nemůžou dočkat, až a explicitní) načíst související data do navigační vlastnosti. V dalším kurzu dozvíte, jak aktualizovat související data.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k Data technologie ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [další](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
