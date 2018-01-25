---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: "Začínáme s databáze Entity Framework 4.0 nejprve a ASP.NET 4 webové formuláře – část 7 | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET používající rozhraní Entity Framework. Vzorová aplikace je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: aeea122636f5235364e6a40cb6e041b1fe221317
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Začínáme s databáze Entity Framework 4.0 nejprve a 4 webových formulářů ASP.NET - část 7
====================
podle [tní Dykstra](https://github.com/tdykstra)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET pomocí sady Visual Studio 2010 a Entity Framework 4.0. Informace o kurzu řady najdete v tématu [první kurz v řadě](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Použití uložených procedur

V tomto kurzu předchozí implementována vzor tabulky na hierarchii dědičnosti. Tento kurz vám ukáže, jak získat lepší kontrolu nad přístup k databázi pomocí uložené procedury.

Rozhraní Entity Framework umožňuje určit, že se mají používat uložené procedury pro přístup k databázi. Pro jakýkoli typ entity můžete používat k vytváření, aktualizaci a odstraňování entity typu uložené procedury. Potom v datovém modelu přidáte odkazy na uložené procedury, které můžete použít k provádění úloh, jako je například načítání sady entit.

Použití uložených procedur je běžné požadavek pro přístup k databázi. V některých případech může správce databáze vyžadovat, aby veškerý přístup databáze projít uložené procedury z bezpečnostních důvodů. V ostatních případech můžete chtít obchodní logiky sestavení do některé z procesů, které rozhraní Entity Framework se používá při aktualizaci databáze. Vždy, když se odstraní entitu můžete chtít zkopírujte jej do archivu databáze. Nebo při každé aktualizaci řádku můžete chtít zapsat řádek do tabulky protokolování této záznamy, kteří provedli změny. Tyto druhy úloh můžete provádět v uložené procedury, která je volána, když rozhraní Entity Framework odstraní entitu nebo aktualizuje entitu.

Jako v předchozích kurzu vytvoříte nejsou žádné nové stránky. Místo toho změníte způsob rozhraní Entity Framework přistupovat k databázi pro některý ze stránek, které jste už vytvořili.

V tomto kurzu vytvoříte uložené procedury v databázi pro vkládání `Student` a `Instructor` entity. Je přidáte do datového modelu, a specifikujete, že rozhraní Entity Framework by měl použít pro přidání `Student` a `Instructor` entity do databáze. Také vytvoříte uložené procedury, která můžete použít k načtení `Course` entity.

## <a name="creating-stored-procedures-in-the-database"></a>Vytváření uložené procedury v databázi

(Pokud používáte *School.mdf* souboru z projektu, které jsou k dispozici ke stažení v tomto kurzu, můžete přeskočit v této části, protože již existuje uložené procedury.)

V **Průzkumníka serveru**, rozbalte položku *School.mdf*, klikněte pravým tlačítkem na **uložené procedury**a vyberte **přidat novou uloženou proceduru**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Zkopírujte následující příkazy SQL a vložte je do okna uložené procedury nahrazení kostru uložené procedury.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student`entit obsahuje čtyři vlastnosti: `PersonID`, `LastName`, `FirstName`, a `EnrollmentDate`. Databáze automaticky generuje hodnotu ID a uložené procedury přijímá parametry pro další tři. Uložená procedura vrací hodnoty klíče záznamu pro nový řádek tak, aby rozhraní Entity Framework můžete sledovat určité, ve verzi entity, který udržuje v paměti.

Uložte a zavřete okno uložené procedury.

Vytvoření `InsertInstructor` uložené procedury stejným způsobem, pomocí následujících příkazů SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Vytvoření `Update` uložené procedury pro `Student` a `Instructor` entity také. (Databáze již obsahuje `DeletePerson` uložené procedury, která bude funkční pro oba `Instructor` a `Student` entity.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

V tomto kurzu budete mapovat všechny tři funkce – insert, update a delete – ke každému typu entity. Rozhraní Entity Framework verze 4 umožňuje mapovat pouze jeden nebo dva z těchto funkcí k uloženým procedurám bez mapování ostatní, s jednou výjimkou: Pokud namapujete funkce aktualizace, ale není funkce odstranění, rozhraní Entity Framework vyvolá výjimku při můžete došlo k pokusu o odstranění entity. V rozhraní Entity Framework verze 3.5 neměl tolik flexibilitu při mapování uložené procedury: Pokud jste namapovali jednu funkci jste potřeba mapovat všechny tři.

Pokud chcete vytvořit uložené procedury, která čte než aktualizuje data, vytvořte ten, který vybere všechny `Course` entity, pomocí následujících příkazů SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Přidávání do datového modelu uložené procedury

Uložené procedury jsou nyní definována v databázi, ale je nutné je přidat do datového modelu, aby byly dostupné Entity Framework. Otevřete *SchoolModel.edmx*, klikněte pravým tlačítkem na návrhovou plochu a vyberte **aktualizovat Model z databáze**. V **přidat** kartě **zvolte si databázové objekty** dialogové okno, rozbalte seznam **uložené procedury**, vyberte nově vytvořenou uložené procedury a `DeletePerson` uložené procedury a potom klikněte na **Dokončit**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Mapování uložené procedury

V Návrháři modelu dat, klikněte pravým tlačítkem myši `Student` entity a vyberte **uložené procedury mapování**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**Podrobnosti mapování** zobrazí se okno, ve kterém můžete zadat uložené procedury, které by měl používat rozhraní Entity Framework pro vložení, aktualizaci a odstraňování entit tohoto typu.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Nastavte **vložit** funkci, aby se **InsertStudent**. Okno zobrazuje seznam parametrů uložené procedury, každý z nich musí být namapovaný na vlastnost entity. Dvě z nich jsou mapovány automaticky protože názvy jsou stejné. Neexistuje vlastnost s názvem entity `FirstName`, takže musíte ručně vybrat `FirstMidName` z rozevíracího seznamu, která se zobrazují vlastnosti dostupné entity. (Je to proto, že jste změnili název `FirstName` vlastnost `FirstMidName` z prvního kurzu.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

Ve stejném **podrobnosti mapování** okno, mapy `Update` funkci, aby se `UpdateStudent` uložené procedury (je nutné zadat `FirstMidName` jako hodnota parametru pro `FirstName`, stejně jako u `Insert` uložené procedury) a `Delete` funkci, aby se `DeletePerson` uložené procedury.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Použijte stejný postup mapování insert, update a delete uložené procedury pro vyučující k `Instructor` entity.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Pro uložené procedury, které čtou místo aktualizace dat, můžete použít **prohlížeče modelu** okno Mapovat uloženou proceduru v entitě typu vrátí. V Návrháři modelu dat, klikněte pravým tlačítkem na návrhové plochy a vyberte možnost **prohlížeče modelu**. Otevřete **SchoolModel.Store** uzel a potom otevřete **uložené procedury** uzlu. Klikněte pravým tlačítkem `GetCourses` uložené procedury a vyberte **přidat importu funkce**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

V **přidat importu funkce** dialogovém **vrátí kolekce z** vyberte **entity**a potom vyberte `Course` vrácena jako typ entity. Když jste hotovi, klikněte na tlačítko **OK**. Uložte a zavřete *.edmx* souboru.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Pomocí vložit, aktualizovat a odstraňovat uložené procedury

Uložené procedury chcete vložit, aktualizovat a odstraňovat data jsou používány rozhraní Entity Framework automaticky po jejich přidání do datového modelu a mapovat je na odpovídající entity. Teď můžete spustit *StudentsAdd.aspx* stránky, a pokaždé, když vytvoříte nový student, bude používat rozhraní Entity Framework `InsertStudent` uložené procedury, chcete-li přidat nový řádek, abyste `Student` tabulky.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Spustit *Students.aspx* stránky a Nový student se zobrazí v seznamu.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Změňte název ověřte, že funkce aktualizace funguje a pak odstraňte studenty do ověřte, že funkce Odstranit funguje.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Použití vyberte uložených procedur

Rozhraní Entity Framework nespustí automaticky uložené procedury, jako `GetCourses`, a nebude možné použít s `EntityDataSource` ovládacího prvku. Jejich použití, můžete je volat z kódu.

Otevřete *InstructorsCourses.aspx.cs* souboru. `PopulateDropDownLists` Metoda používá k načtení všech entit kurzu tak, aby můžete projít v seznamu a zjistit, které z nich je přiřazen lektorem a ty, které nepřiřazené dotaz LINQ entity:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Nahraďte ho následujícím kódem:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

Stránka teď používá `GetCourses` uložené procedury načíst seznam všech kurzů. Spusťte stránku a ověřte, že funguje jako předtím.

(Vlastnosti navigace entit získaných pomocí uložené procedury nemusí být automaticky naplněna data související s těmito entitami, v závislosti na `ObjectContext` výchozí nastavení. Další informace najdete v tématu [načítání související objekty](https://msdn.microsoft.com/library/bb896272.aspx) v knihovně MSDN.)

V dalším kurzu dozvíte, jak bylo snazší program a testování pravidla formátování a ověření dat pomocí funkce Dynamická Data. Místo zadání na každý pravidla webové stránky, jako jsou třeba řetězce formátu data a zda pole je požadováno, tato pravidla můžete zadat v metadatech datového modelu a automaticky se použijí na každé stránce.

>[!div class="step-by-step"]
[Předchozí](the-entity-framework-and-aspnet-getting-started-part-6.md)
[další](the-entity-framework-and-aspnet-getting-started-part-8.md)
