---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Začínáme s Entity Framework 4.0 Database First a technologie ASP.NET 4 webových formulářů – 7. část | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Frameworku. Ukázková aplikace je...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb1abe52b913785f47b7f8ec9822ed9db5ee8a05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823403"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Začínáme s Entity Framework 4.0 Database First a 4 webových formulářů ASP.NET – 7. část
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Framework 4.0 a Visual Studio 2010. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Použití uložených procedur

V předchozím kurzu jste implementovali vzoru pro tabulky na hierarchii dědičnosti. Tomto kurzu se dozvíte, jak získat lepší kontrolu nad přístup k databázi pomocí uložené procedury.

Entity Framework umožňuje určit, že ji by měl používat uložené procedury pro přístup k databázi. Pro každý typ entity můžete zadat uloženou proceduru pro vytvoření, aktualizace nebo odstranění entity daného typu. Potom v datovém modelu můžete přidat odkazy na uložené procedury, které můžete použít k provedení úlohy, třeba načítání sady entit.

Použití uložených procedur je běžné požadavky pro přístup k databázi. V některých případech správce databáze může vyžadovat, aby všechny přístup k databázi projít uložené procedury z bezpečnostních důvodů. V ostatních případech můžete chtít vytvořit obchodní logiky na některé procesy, které používá Entity Framework při aktualizaci databáze. Například pokaždé, když se odstranění entity můžete zkopírovat do archivu databáze. Nebo pokaždé, když se aktualizuje řádek můžete zapsat řádek do tabulky protokolování, která zaznamenává, kteří provedli změnu. Tyto druhy úloh můžete provádět v uloženou proceduru, která je volána pokaždé, když se odstraní entitu rozhraní Entity Framework nebo aktualizuje entitu.

Stejně jako v předchozím kurzu vytvoříte nejsou žádné nové stránky. Místo toho změníte způsob, jak Entity Framework připojí se k databázi pro některý ze stránky, které jste už vytvořili.

V tomto kurzu vytvoříte uložené procedury v databázi pro vkládání `Student` a `Instructor` entity. Přidejte je do datového modelu, a specifikujete, že rozhraní Entity Framework by měl použít pro přidání `Student` a `Instructor` entity do databáze. Také vytvoříte uloženou proceduru, která slouží k načtení `Course` entity.

## <a name="creating-stored-procedures-in-the-database"></a>Vytvoření uložené procedury v databázi

(Pokud používáte *School.mdf* soubor z projektu, které jsou k dispozici ke stažení v tomto kurzu, můžete tuto část přeskočit protože úložné procedury již neexistují.)

V **Průzkumníka serveru**, rozbalte *School.mdf*, klikněte pravým tlačítkem na **uložené procedury**a vyberte **přidat novou uloženou proceduru**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Zkopírujte následující příkazy SQL a vložte je do okna uloženou proceduru nahrazení kostru uloženou proceduru.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` entity nemají čtyři vlastnosti: `PersonID`, `LastName`, `FirstName`, a `EnrollmentDate`. Databáze automaticky generuje hodnotu ID a uložené procedury přijímá parametry pro další tři. Bude procedura vracet hodnoty klíče záznamu nový řádek tak, aby rozhraní Entity Framework můžete udržovat přehled o, který ve verzi entity, který udržuje v paměti.

Uložte a zavřete okno uloženou proceduru.

Vytvoření `InsertInstructor` uloženou proceduru stejným způsobem, pomocí následujících příkazů SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Vytvoření `Update` uložené procedury pro `Student` a `Instructor` entity také. (Databáze již obsahuje `DeletePerson` uloženou proceduru, která bude fungovat pro obě `Instructor` a `Student` entity.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

V tomto kurzu namapujete všechny tři funkce--insert, update a delete – pro každý typ entity. Rozhraní Entity Framework verze 4 umožňuje namapovat pouze jednu nebo dvě z nich fungovat tak, aby uložené procedury bez mapování jiné, s jednou výjimkou: Pokud namapujete funkce aktualizace, ale ne funkci odstranit, Entity Framework vyvolá výjimku při vám Pokus o odstranění entity. V rozhraní Entity Framework verze 3.5, neměl tolik flexibilitu při mapování uložené procedury: Pokud jste namapovali jednu funkci bylo nutné namapovat všechny tři.

Vytvořit uloženou proceduru, která čte spíše než aktualizuje data, vytvořte ten, který vybere všechny `Course` entity, pomocí následujících příkazů SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Přidání uložených procedur do datového modelu

Uložené procedury je nyní definována v databázi, ale musí být přidán do datového modelu, aby byly dostupné pro Entity Framework. Otevřít *SchoolModel.edmx*, klikněte pravým tlačítkem na návrhové ploše a vyberte **aktualizace modelů z databáze**. V **přidat** karty **zvolte vaše databázové objekty** dialogového okna rozbalte **uložené procedury**, vyberte nově vytvořený uložených procedur a `DeletePerson` uložené procedury a potom klikněte na tlačítko **Dokončit**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Mapování uložené procedury

V Návrháři modelů dat, klikněte pravým tlačítkem myši `Student` entity a vyberte **mapování uložené procedury**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**Podrobnosti mapování** zobrazí se okno, ve kterém můžete zadat uložené procedury, které by měl používat Entity Framework pro vkládání, aktualizaci a odstraňování entit tohoto typu.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Nastavte **vložit** funkce **InsertStudent**. V okně se seznamem parametrů uložené procedury, z nichž každý musí být namapována na vlastnost entity. Automaticky dvě z nich jsou mapovány názvy jsou stejné. Není žádná vlastnost entity s názvem `FirstName`, proto musíte ručně vybrat `FirstMidName` z rozevíracího seznamu, který zobrazuje vlastnosti dostupné entity. (Je to proto, že jste změnili název `FirstName` vlastnost `FirstMidName` v první kurz.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

Ve stejném **podrobnosti mapování** okna, mapování `Update` funkce `UpdateStudent` uložené procedury (ujistěte se, že zadáte `FirstMidName` jako hodnota parametru pro `FirstName`, jako jste to udělali `Insert` uložená procedura) a `Delete` funkce `DeletePerson` uložené procedury.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Použijte stejný postup k mapování insert, update a delete Instruktoři do uložených procedur `Instructor` entity.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Pro uložené procedury, které čtou místo aktualizace dat, je použít **prohlížeč modelu** okna pro mapování na entitu uloženou proceduru zadejte jej vrátí. V Návrháři modelů dat, klikněte pravým tlačítkem na návrhové ploše a vyberte **prohlížeč modelu**. Otevřete **SchoolModel.Store** uzlu a pak otevřete **uložené procedury** uzlu. Klepněte pravým tlačítkem myši `GetCourses` uložené procedury a vyberte **přidání importované funkce**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

V **přidání importované funkce** dialogovém okně **vrátí kolekce z** vyberte **entity**a pak vyberte `Course` vrácena jako typ entity. Jakmile budete hotovi, klikněte na tlačítko **OK**. Uložte a zavřete *edmx* souboru.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Pomocí vložení, aktualizace a odstranění uložených procedur

Uložené procedury pro vložení, aktualizace a odstranění dat se používají rozhraním Entity Framework automaticky po přidání do datového modelu a namapované na odpovídající entity. Teď můžete spustit *StudentsAdd.aspx* stránky, a při každém vytvoření nového objektu student používá Entity Framework `InsertStudent` uložené procedury, chcete-li přidat nový řádek `Student` tabulky.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Spustit *Students.aspx* stránky a nového objektu student se zobrazí v seznamu.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Změnit název, který chcete ověřit, že aktualizace funkce funguje a pak odstraňte studenta ověřit fungování funkce odstranit.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Použití uložených procedur Select

Entity Framework nespustí automaticky uložené procedury jako `GetCourses`, a nelze je použít `EntityDataSource` ovládacího prvku. K jejich použití, můžete je volat z kódu.

Otevřít *InstructorsCourses.aspx.cs* souboru. `PopulateDropDownLists` Metoda používá dotaz LINQ entity k načtení všech entit kurzu tak, aby můžete projít seznam a určit ty, které je přiřazeno instruktorem a ty, které nepřiřazené:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Nahraďte následujícím kódem:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

Na stránce teď používá `GetCourses` uložené procedury k získání seznamu všech kurzů. Spuštění stránky a zkontrolujte, jestli funguje stejně jako dříve.

(Vlastnosti navigace entit pomocí uložené procedury nemusí být automaticky vyplní data související s těmito entitami, v závislosti na `ObjectContext` výchozí nastavení. Další informace najdete v tématu [načítání související objekty](https://msdn.microsoft.com/library/bb896272.aspx) v knihovně MSDN.)

V dalším kurzu se naučíte jak usnadňují programu a test pravidla formátování i ověřování dat pomocí funkce Dynamická Data. Místo zadání na každé webové stránky pravidla, jako jsou třeba řetězce formátu data a určuje, jestli pole je povinné, tato pravidla můžete zadat v metadatech datového modelu a automaticky se použijí na každé stránce.

> [!div class="step-by-step"]
> [Předchozí](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [další](the-entity-framework-and-aspnet-getting-started-part-8.md)
