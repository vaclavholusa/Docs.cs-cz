---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: "Začínáme s databáze Entity Framework 4.0 nejprve a ASP.NET 4 webové formuláře – část 5 | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET používající rozhraní Entity Framework. Vzorová aplikace je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 5efc5ff367d5da5df060eba0028399af898a69fa
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Začínáme s databáze Entity Framework 4.0 nejprve a 4 webových formulářů ASP.NET - část 5
====================
podle [tní Dykstra](https://github.com/tdykstra)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET pomocí sady Visual Studio 2010 a Entity Framework 4.0. Informace o kurzu řady najdete v tématu [první kurz v řadě](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Práce s Data v relaci, dál

V tomto kurzu předchozí začalo použít `EntityDataSource` ovládací prvek pro práci s související data. Zobrazí více úrovní hierarchie a upravovat data v navigační vlastnosti. V tomto kurzu budete nadále pracovat s souvisejících dat, přidávání a odstraňování vztahů a přidáním nové entity, která má vztah na stávající entitu.

Vytvoříte stránky, který se přidá kurzy, které jsou přiřazeny k oddělení. Jako vodítko použijte oddělení již existují a když vytvoříte nový kurz, ve stejnou dobu budete navázání vztahu mezi serverem a existující oddělení.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Také vytvoříte stránky, který funguje s relací m: n přiřazením lektorem kurzu (Přidat relaci mezi dvěma entitami, které jste vybrali) nebo odebrání lektorem z průběhu (odebrání vztahu mezi dvěma entitami, které Vyberte). V databázi, Přidat relaci lektorem až kurz výsledkem nový řádek, který se přidává do `CourseInstructor` tabulky přidružení; odebírání vztahu zahrnuje odstranění řádků z `CourseInstructor` tabulky přidružení. Však lze provést v Entity Framework nastavením navigační vlastnosti, bez ohledu `CourseInstructor` explicitně tabulky.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Přidání Entity s relací na stávající entitu

Vytvořit novou webovou stránku s názvem *CoursesAdd.aspx* používající *Site.Master* hlavní stránky a přidejte následující kód do `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Tento kód vytvoří `EntityDataSource` ovládací prvek, který vybere kurzy, která umožňuje vkládání a který určuje obslužnou rutinu pro `Inserting` událostí. Použijete-li obslužná rutina aktualizace `Department` navigační vlastnosti při novou `Course` vytvořena entita.

Kód vytvoří také `DetailsView` ovládací prvek pro použití při přidávání nové `Course` entity. Kód používá vázaná pole pro `Course` vlastností entity. Je nutné zadat `CourseID` hodnota, protože to není pole ID generované systémem. Místo toho je během číslo, které je zadat ručně, když je vytvořena během.

Použít šablonu pole pro `Department` navigační vlastnost protože navigačních vlastností nelze použít s `BoundField` ovládací prvky. Pole šablony poskytuje rozevíracího seznamu vyberte oddělení. Rozevíracím seznamu je vázána `Departments` entity nastavit pomocí `Eval` místo `Bind`, znovu protože nelze přímo svázat navigační vlastnosti za účelem aktualizace je. Zadejte obslužnou rutinu pro `DropDownList` ovládacího prvku `Init` událostí tak, aby uchováváte odkaz na ovládací prvek pro použití kódem, který aktualizuje `DepartmentID` cizí klíč.

V *CoursesAdd.aspx.cs* právě po deklaraci částečné třídy přidat pole třídy pro uložení odkaz na `DepartmentsDropDownList` ovládacího prvku:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Přidání obslužné rutiny `DepartmentsDropDownList` ovládacího prvku `Init` událostí tak, aby uchováváte odkaz na ovládací prvek. To vám umožní získat uživatel zadal hodnotu a použít ho k aktualizaci `DepartmentID` hodnotu `Course` entity.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Přidání obslužné rutiny `DetailsView` ovládacího prvku `Inserting` událostí:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Když uživatel klikne na `Insert`, `Inserting` událost se vyvolá, než je vložit nový záznam. Získá kód obslužné rutiny `DepartmentID` z `DropDownList` řízení a použije ho k nastavit hodnotu, která se použije pro `DepartmentID` vlastnost `Course` entity.

Rozhraní Entity Framework se postará o přidání tohoto kurzu k `Courses` navigační vlastnost přidruženého `Department` entity. Aby oddělení také přidá `Department` vlastnost navigace `Course` entity.

Spuštění stránky.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Zadejte ID, název, počet kredity a vyberte oddělení a potom klikněte na tlačítko **vložit**.

Spustit *Courses.aspx* a vyberte zobrazíte nové během stejného oddělení.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Práce s relací m: N

Vztah mezi `Courses` sady entit a `People` sady entit je relace m: n. A `Course` entity má navigační vlastnost s názvem `People` , může obsahovat nula, jednu nebo více souvisejících `Person` entity (představující vyučující přiřazené naučit tohoto kurzu). A `Person` entity má navigační vlastnost s názvem `Courses` , může obsahovat nula, jednu nebo více souvisejících `Course` entity (představující kurzy této lektorem je přiřazena k výuce). Jeden lektorem může naučit více kurzy a může být jeden kurzu výukové ve více vyučující. V této části návodu budete přidávat a odebírat vztahy mezi `Person` a `Course` entity tím, že aktualizuje vlastnosti navigace entit v relaci.

Vytvořit novou webovou stránku s názvem *InstructorsCourses.aspx* používající *Site.Master* hlavní stránky a přidejte následující kód do `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Tento kód vytvoří `EntityDataSource` ovládací prvek, který načte název a `PersonID` z `Person` entity pro vyučující. A `DropDrownList` ovládací prvek je vázán `EntityDataSource` ovládacího prvku. `DropDownList` Určuje obslužné rutiny pro ovládací prvek `DataBound` událostí. Budete používat databind dva rozevírací seznamy, které zobrazují kurzy této obslužné rutiny.

Kód vytvoří také následující skupiny ovládacích prvků, které chcete použít pro přiřazení k vybrané lektorem kurzu:

- A `DropDownList` ovládací prvek pro výběr kurzu přiřadit. Tento ovládací prvek bude možné naplnit kurzy, které nejsou přiřazeny k vybrané lektorem.
- A `Button` řízení zahájíte přiřazení.
- A `Label` ovládací prvek zobrazí chybovou zprávu, pokud selže přiřazení.

Nakonec kód také vytvoří skupinu ovládacích prvků pro použití pro odebrání z vybrané lektorem kurzu.

V *InstructorsCourses.aspx.cs*, přidat, pomocí příkazu:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Přidání metody pro sestavování dva rozevírací seznamy, které zobrazují kurzy:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Tento kód získá všechny kurzy od `Courses` entity nastavit a získá kurzy od `Courses` vlastnost navigace `Person` entity pro vybrané lektorem. Poté určí, které kurzy jsou přiřazeny k této lektorem a naplní rozevírací seznamy odpovídajícím způsobem.

Přidání obslužné rutiny `Assign` tlačítka `Click` událostí:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Tento kód získá `Person` získá entity pro vybrané lektorem `Course` entity pro vybraný kurz a přidá vybraný kurz k `Courses` vlastnost navigace lektorem `Person` entity. Pak uloží změny do databáze a znovu naplní rozevírací seznamy, takže se okamžitě zobrazí výsledky.

Přidání obslužné rutiny `Remove` tlačítka `Click` událostí:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Tento kód získá `Person` získá entity pro vybrané lektorem `Course` entity pro vybraný kurz a také odebere vybraný kurz z `Person` entity `Courses` navigační vlastnost. Pak uloží změny do databáze a znovu naplní rozevírací seznamy, takže se okamžitě zobrazí výsledky.

Přidejte kód, který `Page_Load` metoda, která zajišťuje chybové zprávy, které nejsou viditelné, když se nezobrazí žádná chyba sestavy a přidejte obslužné rutiny pro `DataBound` a `SelectedIndexChanged` události z rozevíracího seznamu vyučující k naplnění rozevíracího seznamu kurzy:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Spuštění stránky.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Vyberte lektorem. **Přiřadit kurzu** kurzů, které není lektorem naučit, zobrazí se seznam rozevíracího seznamu a **odebrat kurzu** rozevíracím seznamu zobrazí kurzů, které lektorem je již přiřazen k. V **přiřadit kurzu** vyberte kurzu a pak klikněte na tlačítko **přiřadit**. Během přesune **odebrat kurzu** rozevíracího seznamu. Vyberte kurz v **odebrat kurzu** části a klikněte na tlačítko **odebrat ***.* Během přesune **přiřadit kurzu** rozevíracího seznamu.

Nyní jste viděli některé další způsoby, jak pracovat s související data. V následujícím kurzu budete Další informace o použití dědičnosti v datovém modelu ke zlepšení udržovatelnosti vaší aplikace.

>[!div class="step-by-step"]
[Předchozí](the-entity-framework-and-aspnet-getting-started-part-4.md)
[další](the-entity-framework-and-aspnet-getting-started-part-6.md)
