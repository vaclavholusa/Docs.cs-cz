---
title: "Jádro ASP.NET MVC s EF Core - číst související Data - 6 10"
author: tdykstra
description: "V tomto kurzu budete číst a zobrazení souvisejících dat – to znamená, data, která rozhraní Entity Framework se načte do navigační vlastnosti."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 2333ac70c77847ece1f90c9ff22eec30bc35fea1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a>Čtení související data – základní EF s kurz k ASP.NET MVC jádra (6 10)

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).

V tomto kurzu předchozí dokončit školní datový model. V tomto kurzu budete pro čtení a zobrazení souvisejících dat – to znamená, data, která rozhraní Entity Framework se načte do navigační vlastnosti.

Následující ilustrace znázorňuje stránky, že bude fungovat s.

![Kurzy indexovou stránku](read-related-data/_static/courses-index.png)

![Index stránky vyučující](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Přes, explicitní a opožděného načítání souvisejících dat

Existuje několik způsobů softwaru relační objekt mapování (ORM), jako je rozhraní Entity Framework můžete načíst související data do navigační vlastnosti entity:

* Přes načítání. Při čtení je entita, související data načtena společně s jeho. To obvykle vede jednoho připojení dotaz, který načte všechna data, která je potřeba. Zadejte přes načítání v Entity Framework Core pomocí `Include` a `ThenInclude` metody.

  ![Příklad přes načítání](read-related-data/_static/eager-loading.png)

  Můžete načíst některá data v samostatné dotazy a EF "oprav" navigační vlastnosti.  To znamená EF automaticky přidá samostatně načtené entit, které patří ve vlastnosti navigace entit. dříve načteny. Pro dotaz, který načte související data, můžete použít `Load` metody místo metodu, která vrátí seznam nebo objekt, jako například `ToList` nebo `Single`.

  ![Příklad samostatné dotazy](read-related-data/_static/separate-queries.png)

* Explicitní načítání. Když je nejdřív přečíst entity, není načíst související data. Můžete napsat kód, který načte související data, pokud je to potřeba. Jako v případě eager načítání s samostatné dotazy explicitní načítání více dotazů má za následek odeslal do databáze. Rozdíl je, s explicitní načítání, určuje kód navigační vlastnosti, které mají být načtena. V Entity Framework Core 1.1 můžete použít `Load` metoda udělat explicitní načítání. Příklad:

  ![Příklad explicitní načítání](read-related-data/_static/explicit-loading.png)

* Opožděného načítání. Když je nejdřív přečíst entity, není načíst související data. Ale při prvním pokusu o přístup k navigační vlastnost, lze data potřebná pro tuto navigační vlastnost je automaticky načte. Dotaz je odeslal do databáze při každém pokusu o získání dat z navigační vlastnosti pro první. Základní Entity Framework 1.0 nepodporuje opožděného načítání.

### <a name="performance-considerations"></a>Důležité informace o výkonu

Pokud víte, že potřebujete souvisejících dat pro každou entitu, načíst, přes načítání často nabízí nejlepší výkon, protože je obvykle efektivnější než samostatné dotazy pro každé entity načíst jediný dotaz odeslal do databáze. Předpokládejme například, že každé oddělení má deset související kurzy. Přes načítání všechna související data by způsobilo právě dotazu jedním (připojit) a jedinou odezvy do databáze. Samostatné dotazu pro kurzy pro každé oddělení by způsobilo 11 zpátečních cest k databázi. Navíc zpátečních cest k databázi jsou obzvláště škodlivé pro výkon při latence je vysoká.

Na druhé straně v některých případech je efektivnější samostatné dotazy. Přes načítání všechna související data v jednom dotazu může způsobit velmi složité spojení má být vygenerován, který SQL Server nemůže zpracovat efektivně. Nebo pokud potřebujete pro přístup k navigační vlastnosti entity jenom pro podmnožinu sady entit, které jste zpracování, samostatné dotazy může lépe provést, protože by přes načítání všechno předem načíst více dat, než budete potřebovat. Pokud je důležité výkon, je nejvhodnější pro testování výkonu obou směrech, aby bylo možné nejlepší volbou.

## <a name="create-a-courses-page-that-displays-department-name"></a>Vytvořte stránku kurzy, která zobrazí název oddělení

Entity kurzu zahrnuje navigační vlastnost, která obsahuje entitu oddělení přiřazený během oddělení. Zobrazí se název přiřazené oddělení v seznamu kurzů, potřebujete získat vlastnosti Name z oddělení entity, která je v `Course.Department` navigační vlastnost.

Vytvoříte řadič s názvem CoursesController pro typ entity kurzu, pomocí stejných možností pro **kontroler MVC se zobrazeními s využitím nástroje Entity Framework** scaffolder, který jste dříve pro studenty řadič, jak je znázorněno Následující obrázek:

![Přidat řadič kurzy](read-related-data/_static/add-courses-controller.png)

Otevřete *CoursesController.cs* a prozkoumat `Index` metoda. Automatické generování uživatelského rozhraní má zadanou přes načítání pro `Department` navigační vlastnost s použitím `Include` metoda.

Nahraďte `Index` metoda s následující kód, který používá vhodnější název `IQueryable` kurzu entity, který vrací (`courses` místo `schoolContext`):

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

Otevřete *Views/Courses/Index.cshtml* a nahraďte kód šablony s následujícím kódem. Změny se zvýrazněnou:

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Automaticky generovaný kód, které jste udělali následující změny:

* Změnit záhlaví od indexu na kurzy.

* Přidat **číslo** sloupec, který ukazuje `CourseID` hodnotu vlastnosti. Ve výchozím nastavení nejsou vygeneroval primární klíče, protože jsou obvykle smysl pro koncové uživatele. Ale v takovém případě má smysl primární klíč a chcete ji zobrazit.

* Změnit **oddělení** sloupec, který se zobrazí název oddělení. Kód zobrazí `Name` vlastnost oddělení entity, který je načten do `Department` navigační vlastnost:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Spusťte aplikaci a vyberte **kurzy** karty zobrazíte seznam s názvy oddělení.

![Kurzy indexovou stránku](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Vytvořit stránku vyučující, která zobrazuje kurzy a registrace

V této části vytvoříte řadič a zobrazení pro entitu lektorem aby bylo možné zobrazit stránku vyučující:

![Index stránky vyučující](read-related-data/_static/instructors-index.png)

Tato stránka načte a zobrazí související data následujícími způsoby:

* Zobrazí seznam vyučující související data ze OfficeAssignment entity. Lektorem a OfficeAssignment entity jsou ve vztahu-nula nebo 1. Přes načítání budete používat pro OfficeAssignment entity. Jak je popsáno výše, je obvykle efektivnější přes načítání, když potřebujete související data pro všechny načtené řádky v primární tabulce. V takovém případě budete chtít zobrazit přiřazení office pro všechny zobrazené vyučující.

* Když uživatel vybere lektorem, zobrazí se během entit v relaci. Lektorem a kurzu entity jsou v relaci m: n. Přes načítání budete používat pro kurzu entity a jejich oddělení entit v relaci. Samostatné dotazy v tomto případě může být efektivnější, protože potřebujete jenom pro vybrané lektorem kurzy. Však tento příklad ukazuje způsob použití přes načítání pro navigační vlastnosti v rámci entit, které samy navigační vlastnosti.

* Když uživatel vybere kurzu, zobrazí se související data ze sady entit registrace. Průběh a registrace entity jsou vztah jeden mnoho. Samostatné dotazy budete používat pro registraci a jejich související Student entit.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Vytvoření modelu zobrazení pro Index lektorem zobrazení

Stránka vyučující zobrazuje data ze tří různých tabulek. Proto vytvoříte zobrazení model, který zahrnuje tři vlastnosti každého která uchovává data pro jednu z tabulek.

V *SchoolViewModels* složku vytvořit *InstructorIndexData.cs* a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Vytvoření řadiče lektorem a zobrazení

Vytvoření řadiče vyučující s akcemi čtení/zápisu EF, jak je znázorněno na následujícím obrázku:

![Přidat řadič vyučující](read-related-data/_static/add-instructors-controller.png)

Otevřete *InstructorsController.cs* a přidat, pomocí příkazu pro obor názvů ViewModels:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

Nahraďte metodu Index následující kód do udělat přes načítání souvisejících dat a umístit ho v zobrazení modelu.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

Metoda přijímá data volitelné trasy (`id`) a parametr řetězce dotazu (`courseID`), zadejte hodnoty ID vybrané lektorem a vybraný kurz. Parametry jsou poskytovány **vyberte** hypertextové odkazy na stránce.

Vytvoření instance modelu zobrazení a vložení v něm seznam vyučující začne kód. Určuje kód přes načítání pro `Instructor.OfficeAssignment` a `Instructor.CourseAssignments` navigační vlastnosti. V rámci `CourseAssignments` vlastnost, `Course` vlastnost je načíst a v rámci které, `Enrollments` a `Department` vlastnosti jsou načíst a v každém `Enrollment` entity `Student` vlastnosti je načtena.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

Vzhledem k tomu, že zobrazení vždy vyžaduje OfficeAssignment entity, je efektivnější načíst, ve stejném dotazu. Během entity se vyžadují při lektorem je vybrána na webové stránce, takže jeden dotaz je lepší, než více dotazů pouze v případě, že stránka se zobrazí více často ke kurzu vybrané než bez.

Kód opakuje `CourseAssignments` a `Course` vzhledem k tomu, že budete potřebovat dvě vlastnosti z `Course`. První řetězec `ThenInclude` volá získá `CourseAssignment.Course`, `Course.Enrollments`, a `Enrollment.Student`.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

V tomto bodě v kódu jiné `ThenInclude` by pro navigační vlastnosti `Student`, které nepotřebujete. Ale volání `Include` začíná přes `Instructor` vlastnosti, takže budete muset projít řetězu znovu, tento čas zadání `Course.Department` místo `Course.Enrollments`.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

Následující kód provede, když se lektorem byl vybrán. Vybrané lektorem se načítají ze seznamu vyučující v zobrazení modelu. Model zobrazení `Courses` vlastnost je pak načten pomocí postupu entity z této lektorem `CourseAssignments` navigační vlastnost.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

`Where` Metoda vrátí kolekci, ale v takovém případě kritéria předaný jejichž metoda výsledkem jsou pouze jednu entitu lektorem nevrátila. `Single` Metoda převede kolekci na jednu entitu lektorem, která umožňuje přístup k dané entity `CourseAssignments` vlastnost. `CourseAssignments` Vlastnost obsahuje `CourseAssignment` entity, z nichž chcete pouze související `Course` entity.

Můžete použít `Single` metoda na kolekci, když víte kolekce budou mít jen jednu položku. Jeden metoda vyvolá výjimku, pokud je kolekce do ní předán prázdný nebo pokud existuje více než jednu položku. Alternativou je `SingleOrDefault`, která vrací výchozí hodnotu (null v tomto případě) Pokud je kolekce prázdná. Ale v takovém případě stále vznikly by výjimku (z pokusu o vyhledání `Courses` vlastnost na hodnotu Null), a zpráva o výjimce by méně jasně ukazovat na příčinu problému. Při volání `Single` metodu, můžete také předat v Where podmínku namísto volání `Where` metoda samostatně:

```csharp
.Single(i => i.ID == id.Value)
```

Namísto:

```csharp
.Where(I => i.ID == id.Value).Single()
```

Dále pokud jste vybrali kurzu, vybraný kurz se načítají seznam kurzů ve model zobrazení. Potom zobrazení modelu `Enrollments` vlastnosti je načtena s registrace entity z tohoto kurzu `Enrollments` navigační vlastnost.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a>Upravit zobrazení lektorem indexu

V *Views/Instructors/Index.cshtml*, nahraďte kód šablony s následujícím kódem. Změny se zvýrazněnou.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

Stávající kód, které jste udělali následující změny:

* Změnit třídu modelu k `InstructorIndexData`.

* Změnit název stránky z **Index** k **vyučující**.

* Přidat **Office** sloupec, který zobrazuje `item.OfficeAssignment.Location` pouze v případě `item.OfficeAssignment` není null. (Protože je vztah jeden pro žádná nebo jedna, nemusí být související entity OfficeAssignment.)

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Přidat **kurzy** sloupec, který zobrazuje kurzy výukové podle jednotlivých lektorem. V tématu [explicitní přechod na řádek s `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) Další informace o tuto syntaxi razor.

* Přidání kódu, která dynamicky přidá `class="success"` k `tr` element vybrané lektorem. Toto nastaví barvu pozadí vybraného řádku použití Bootstrap třídy.

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Přidat nový hypertextový odkaz s názvem bez přípony **vyberte** bezprostředně před další odkazy v každém řádku, což způsobí, že ID vybrané lektorem k odeslání `Index` metoda.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Spusťte aplikaci a vyberte **vyučující** kartě. Na stránce zobrazuje vlastnosti umístění entit v relaci OfficeAssignment a prázdná tabulka buňky po žádné související entity OfficeAssignment.

![Vyučující indexovou stránku nic zvolené](read-related-data/_static/instructors-index-no-selection.png)

V *Views/Instructors/Index.cshtml* soubor po zavření tabulky – element (na konci souboru), přidejte následující kód. Tento kód zobrazí seznam kurzů související s lektorem, pokud je vybrána lektorem.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

Tento kód čte `Courses` vlastnost modelu zobrazení zobrazíte seznam kurzů. Poskytuje také **vyberte** hypertextový odkaz, který odesílá ID vybraný kurz k `Index` metody akce.

Aktualizujte stránku a vyberte lektorem. Nyní se zobrazí v mřížce, která zobrazuje kurzy přiřazen k vybrané lektorem a pro každou kurz zobrazí název přiřazené oddělení.

![Vyučující Index stránky lektorem vybrané](read-related-data/_static/instructors-index-instructor-selected.png)

Za blok kódu, který jste právě přidali přidejte následující kód. Zobrazí seznam studenty, kteří jsou zaregistrované v kurzu při výběru tohoto kurzu.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

Tento kód načte vlastnost registrace zobrazení modelu Chcete-li zobrazit seznam studenty zařazen do kurzu.

Aktualizujte stránku znovu a vyberte lektorem. Pak vyberte kurzu chcete zobrazit seznam registrovaných studenty a jejich tříd.

![Vyučující Index stránky lektorem a kurzu vybrán](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a>explicitní načítání

Pokud jste získali seznam vyučující v *InstructorsController.cs*, jste zadali přes načítání pro `CourseAssignments` navigační vlastnost.

Předpokládejme, že očekává uživatelům jen zřídka rádi viděli v vybrané lektorem a během registrace. V takovém případě můžete chtít načíst data registrace pouze v případě, že se požaduje. Chcete-li zobrazit příklad, jak to provést explicitní načítání, nahraďte `Index` metoda následující kód, který odebere přes načítání pro registraci a načte vlastnosti explicitně. Změny kódu jsou vyznačené.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

Zahodí nový kód *ThenInclude* metoda volá zápisu dat z kódu, který načte lektorem entity. Pokud je vybrána lektorem a kurzu, načte zvýrazněný registrace pro vybraný kurz a Student entit pro každý registrace.

Spuštění aplikace, přejděte na stránku vyučující Index a budete vidět žádné rozdíly v co se zobrazí na stránce, i když jste změnili, jak jsou načtena data.

## <a name="summary"></a>Souhrn

Můžete si teď použít přes načítání s jeden dotaz a více dotazů ke čtení související data do navigační vlastnosti. V dalším kurzu dozvíte, jak aktualizovat data v relaci.

>[!div class="step-by-step"]
>[Předchozí](complex-data-model.md)
>[další](update-related-data.md)  
