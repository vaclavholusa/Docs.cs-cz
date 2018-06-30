---
title: Jádro ASP.NET MVC s EF Core - dědičnosti - 9, 10
author: rick-anderson
description: Tento kurz vám ukáže, jak implementovat dědičnosti v datovém modelu Entity Framework Core v aplikaci ASP.NET Core.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/inheritance
ms.openlocfilehash: a71954297f44f936893a7f1e9d3b0685f81378b9
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/29/2018
ms.locfileid: "37092994"
---
# <a name="aspnet-core-mvc-with-ef-core---inheritance---9-of-10"></a>Jádro ASP.NET MVC s EF Core - dědičnosti - 9, 10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).

V tomto kurzu předchozí zpracování výjimky souběžnosti. Tento kurz vám ukáže, jak implementovat dědičnosti v datovém modelu.

V objektově orientované programování, můžete dědičnost usnadňuje opakované použití kódu. V tomto kurzu změníte `Instructor` a `Student` tak, aby se odvozena od třídy `Person` základní třída, která obsahuje vlastnosti, jako `LastName` , které jsou společné pro vyučující a studenti. Nebude přidat nebo změnit jakékoli webové stránky, ale budete měnit kód, a tyto změny se automaticky zobrazí v databázi.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Možnosti pro mapování dědičnosti tabulkách databáze

`Instructor` a `Student` třídy v datovém modelu školní mají několik vlastností, které jsou stejné:

![Třídy Student a lektorem](inheritance/_static/no-inheritance.png)

Předpokládejme, že chcete odstranit redundantní kód pro vlastnosti, které jsou sdíleny `Instructor` a `Student` entity. Nebo chcete napsat služba, která dokáže formátovat názvy bez caring, zda název pochází lektorem nebo student. Můžete vytvořit `Person` základní třídu, která obsahuje pouze ty sdílené vlastnosti, ujistěte se, `Instructor` a `Student` třídy dědí ze základní třídy, jak je znázorněno na následujícím obrázku:

![Studenty a lektorem třídy odvozené od třídy osoba](inheritance/_static/inheritance.png)

Existuje několik způsobů, které tato struktura dědičnosti může být reprezentován v databázi. Můžete mít tabulku osoba, která obsahuje informace o studenty a vyučující v jediné tabulce. Některé sloupce může použít pouze pro vyučující (HireDate), některé jenom pro studenty (EnrollmentDate), některé na obou (LastName, FirstName). Obvykle se mají sloupce diskriminátoru k určení typů, které se každý řádek představuje. Například možná sloupce diskriminátoru pro vyučující a "Student" pro studenty, "Lektorem".

![Příklad tabulky podle hierarchie](inheritance/_static/tph.png)

Tento vzor generování strukturu dědičnosti entity z jedné tabulky databáze se nazývá tabulky za hierarchie dědičnosti (TPH).

Alternativou je ke změně databáze vzhled strukturu dědičnosti. Například můžete mít jenom pole název v tabulce osoby a mít samostatné lektorem a Student tabulky s poli datum.

![Každý typ tabulky dědičnosti](inheritance/_static/tpt.png)

Tento vzor vytvoření databázové tabulky pro každou třídu entity nazývá za dědičnost typů (TPT).

Další možností je ještě mapování všechny typy neabstraktní na jednotlivé tabulky. Všechny vlastnosti třídy, včetně zděděné vlastnosti, mapování na sloupce odpovídající tabulky. Tento vzor nazývá dědičnost tabulky na konkrétní třídy (TPC). Pokud jste implementovali TPC dědičnosti pro osoby, Student a lektorem třídy, jako je uvedené výše, vypadat Student a lektorem tabulky nejsou jiné po implementace dědičnosti než dříve.

TPC a TPH vzory dědičnosti obecně poskytovat lepší výkon než TPT vzory dědičnosti, protože TPT vzory může mít za následek komplexní spojení dotazy.

Tento kurz ukazuje, jak implementovat dědičnost TPH. TPH je pouze dědičnosti vzor, který podporuje základní Entity Framework.  Co můžete to udělat, je vytvořit `Person` třídy, změňte `Instructor` a `Student` odvozovat ze tříd `Person`, přidejte novou třídu do `DbContext`a vytvořte migrace.

> [!TIP]
> Zvažte možnost uložení kopie projektu před prováděním následujících změn.  Pak pokud narazíte na problémy je nutné začít znovu, je snazší spuštění z projektu uložený místo Prohodit kroky v tomto kurzu nebo má zpět na začátek celé řady.

## <a name="create-the-person-class"></a>Vytvořte třídu osoba

Ve složce modely vytvořte Person.cs a nahraďte kód šablony s následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Ujistěte se, studenty a lektorem třídy dědí osoba

V *Instructor.cs*, lektorem třída odvozena od třídy osoby a odeberte pole klíče a název. Kód bude vypadat jako v následujícím příkladu:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

Provést stejné změny v *Student.cs*.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>Přidat typ uživatel entity do datového modelu

Přidejte typ entity osoby k *SchoolContext.cs*. Jsou vyznačené nové řádky.

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

To je vše, chcete-li nakonfigurovat tabulky za hierarchie dědičnosti vyžadující rozhraní Entity Framework. Jak zjistíte, když se aktualizuje databázi, bude mít tabulku osoba místo Student a lektorem tabulky.

## <a name="create-and-customize-migration-code"></a>Vytvářet a přizpůsobovat migrace kódu

Uložte změny a sestavte projekt. Potom otevřete příkazové okno ve složce projektu a zadejte následující příkaz:

```console
dotnet ef migrations add Inheritance
```

Nemůžete spustit `database update` ještě příkaz. Tento příkaz povede ke ztrátě dat. protože bude odpojit tabulku lektorem a přejmenovat tabulku Student osobě. Je třeba zadat vlastní kód, aby byla zachována stávající data.

Otevřete *migrace nebo\<časové razítko > _Inheritance.cs* a nahraďte `Up` metoda následujícím kódem:

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

Tento kód má na starosti následující úlohy aktualizace databáze:

* Odebere omezení cizího klíče a indexy, které ukazují na Student tabulku.

* Tabulku lektorem jako uživatel přejmenuje a provede změny, které jsou potřeba pro ni k ukládání dat Student:

* Přidá EnrollmentDate s možnou hodnotou Null pro studenty.

* Přidá sloupce diskriminátoru označuje, zda řádek pro student nebo lektorem.

* Vzhledem k tomu, že řádky student nebude mít přijetím kalendářních dat, díky HireDate s možnou hodnotou Null.

* Přidá dočasné pole, které se použije k aktualizaci cizí klíče, které odkazují na studenty. Při kopírování studenty do tabulky osob získají nové hodnot primárního klíče.

* Kopíruje data z tabulky studenty do tabulky osob. To způsobí, že studentů přiřazovány nové hodnot primárního klíče.

* Opravy hodnoty cizího klíče, které odkazují na studenty.

* Znovu vytvoří omezení cizího klíče a indexy, nyní je odkazující na tabulky osoba.

(Při použití GUID místo celé číslo jako typ primární klíče, student hodnot primárního klíče nebude muset změnit a některé z těchto kroků by byly vynechány.)

Spustit `database update` příkaz:

```console
dotnet ef database update
```

(V produkční systému by uděláte odpovídající změny `Down` metodu v případě, kdy bylo nutné použít, se vrátíte k předchozí verzi databáze. V tomto kurzu nebudete používat `Down` metoda.)

> [!NOTE]
> Je možné získat další chyby při provádění změn schématu v databázi, která obsahuje stávající data. Pokud dojde k chybám migrace, které nelze vyřešit, můžete změnit název databáze v připojovacím řetězci nebo odstraňte tuto databázi. S novou databázi nejsou žádná data k migraci a příkazu update-database je pravděpodobnější dokončeno bez chyb. Pokud chcete odstranit databázi, použijte SSOX nebo spusťte `database drop` rozhraní příkazového řádku příkaz.

## <a name="test-with-inheritance-implemented"></a>Testování s použitím dědičnosti implementována

Spusťte aplikaci a zkuste stránky. Všechno funguje stejným způsobem jako předtím.

V **Průzkumník objektů systému SQL Server**, rozbalte položku **datové připojení nebo SchoolContext** a potom **tabulky**, a zjistíte, že byly nahrazeny Student a lektorem tabulky Tabulka osoby. Otevřete návrháře tabulky osoby a obsahuje všechny sloupce, které používají jako Student a lektorem tabulky.

![Osoba tabulky v SSOX](inheritance/_static/ssox-person-table.png)

Klikněte pravým tlačítkem na tabulky osoba a pak klikněte na **zobrazit Data tabulky** zobrazíte sloupce diskriminátoru.

![Osoba tabulky v SSOX - data tabulky](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>Souhrn

Jste implementovali tabulky za hierarchie dědičnosti pro `Person`, `Student`, a `Instructor` třídy. Další informace o dědičnosti v Entity Framework Core najdete v tématu [dědičnosti](https://docs.microsoft.com/ef/core/modeling/inheritance). V dalším kurzu se zobrazí, jak bude zpracováván celou řadu relativně pokročilých scénářích rozhraní Entity Framework.

::: moniker-end

> [!div class="step-by-step"]
> [Předchozí](concurrency.md)
> [další](advanced.md)
