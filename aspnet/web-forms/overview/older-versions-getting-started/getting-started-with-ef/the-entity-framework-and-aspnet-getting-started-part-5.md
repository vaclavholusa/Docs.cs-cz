---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Začínáme s Entity Framework 4.0 Database First a technologie ASP.NET 4 webových formulářů – část 5 | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Frameworku. Ukázková aplikace je...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 3209ab3bca58e0dde90cf279732d177418b034e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752505"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Začínáme s Entity Framework 4.0 Database First a 4 webových formulářů ASP.NET – 5. část
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Framework 4.0 a Visual Studio 2010. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Práce se souvisejícími daty, pokračuje se

V předchozím kurzu jste začali používat `EntityDataSource` ovládací prvek pro práci s související data. Zobrazit více úrovní hierarchie a upravovat data v navigační vlastnosti. V tomto kurzu budete dál pracovat se souvisejícími daty přidáním a odstraněním relace a tak, že přidáte novou entitu, která má vztah k existující entity.

Vytvoříte stránky, která přidá kurzů, které jsou přiřazeny k oddělení. Jako vodítko použijte oddělení ještě neexistuje, a když vytvoříte nový kurz, ve stejnou dobu budete vytvoříte vztah mezi ním a existující oddělení.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Také vytvoříte stránky, která funguje s relací many-to-many přiřazením instruktorem kurzu (přidání relace mezi dvěma entitami, které vyberete) nebo jejich odebrání instruktorem kurz (odstranit vztah mezi dvěma entitami, které jste Výběr). V databázi, přidání relace mezi instruktorem a kurzu výsledky na novém řádku, které se přidávají do `CourseInstructor` přidružení tabulky; odebrání relace zahrnuje odstranění řádku ze `CourseInstructor` přidružení tabulky. Však lze provést v Entity Framework nastavením vlastnosti navigace, bez ohledu `CourseInstructor` explicitně tabulky.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Přidání Entity se vztahem k existující Entity

Vytvoření nové webové stránky s názvem *CoursesAdd.aspx* , která používá *Site.Master* stránku předlohy a přidejte následující kód k `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Tento kód vytvoří `EntityDataSource` ovládací prvek, který vybere kurzů, které, který umožňuje vložení a který určuje obslužnou rutinu pro `Inserting` událostí. Obslužná rutina budete používat k aktualizaci `Department` vlastnost navigace, když je nový `Course` vytvořené entity.

Kód vytvoří také `DetailsView` ovládací prvek a použít pro přidání nových `Course` entity. Kód používá vázaného pole pro `Course` vlastností entity. Je nutné zadat `CourseID` hodnotu, protože to není pole ID generované systémem. Místo toho je kurz číslo, které musí být zadán ručně, když se na kurz.

Použijete pole šablony pro `Department` navigační vlastnost protože navigační vlastnosti nelze použít s `BoundField` ovládacích prvků. Pole šablony obsahuje rozevírací seznam a vyberte oddělení. Rozevíracím seznamu je vázán na `Departments` nastavení s použitím entity `Eval` spíše než `Bind`, znovu vzhledem k tomu, že nelze přímo vázat vlastnosti navigace za účelem aktualizace je. Zadejte obslužnou rutinu pro `DropDownList` ovládacího prvku `Init` události tak, aby odkaz na ovládací prvek pro použití můžete ukládat tak, že kód, který aktualizuje `DepartmentID` cizího klíče.

V *CoursesAdd.aspx.cs* hned za deklaraci částečné třídy přidat odkaz na pole třídy `DepartmentsDropDownList` ovládacího prvku:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Přidání obslužné rutiny `DepartmentsDropDownList` ovládacího prvku `Init` události tak, aby uchováváte odkaz na ovládací prvek. To vám umožní získat hodnotu uživatel zadal a pomocí něj aktualizovat `DepartmentID` hodnotu `Course` entity.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Přidání obslužné rutiny `DetailsView` ovládacího prvku `Inserting` události:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Když uživatel klepne `Insert`, `Inserting` událost je aktivována před vložení nového záznamu. Získá kód v obslužné rutině `DepartmentID` z `DropDownList` řídit a použije ho k nastavení hodnoty, který se použije pro `DepartmentID` vlastnost `Course` entity.

Entity Framework se postará o přidání tohoto kurzu a `Courses` navigační vlastnost přidruženou `Department` entity. Přidá také oddělení `Department` vlastnost navigace `Course` entity.

Spuštění stránky.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Zadejte ID, název, číslo kredity a vyberte oddělení a potom klikněte na tlačítko **vložit**.

Spustit *Courses.aspx* stránky a vyberte stejného oddělení zobrazíte nové kurzu.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Práce se vztahy m: N

Vztah mezi `Courses` sady entit a `People` sady entit je relace m: m. A `Course` entita má vlastnost navigace s názvem `People` , který může obsahovat nula, jeden nebo více souvisejících `Person` entity (představující Instruktoři přiřazené představuje kurzu). A `Person` entita má vlastnost navigace s názvem `Courses` , který může obsahovat nula, jeden nebo více souvisejících `Course` entity (představující kurzů představuje je přiřazen tento kurzů vedených). Jeden instruktorem mohou naučit více kurzů a jeden kurz může vedené instruktory více. V této části tohoto návodu budete přidávat a odebírat vztahy mezi `Person` a `Course` entity pomocí aktualizace vlastností navigace souvisejících entit.

Vytvoření nové webové stránky s názvem *InstructorsCourses.aspx* , která používá *Site.Master* stránku předlohy a přidejte následující kód k `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Tento kód vytvoří `EntityDataSource` ovládací prvek, který načte název a `PersonID` z `Person` entity pro vyučující. A `DropDrownList` ovládací prvek vázán `EntityDataSource` ovládacího prvku. `DropDownList` Určuje obslužné rutiny pro ovládací prvek `DataBound` událostí. Pomocí této obslužné rutiny na databind dva rozevírací seznamy, které zobrazují kurzů.

Kód vytvoří také následující skupiny ovládacích prvků pro použití pro přiřazení k vybraných kurzů vedených kurzu:

- A `DropDownList` ovládací prvek pro výběr kurzu přiřazení. Tento ovládací prvek se vyplní kurzů, které nejsou přiřazeny k vybrané instruktorem.
- A `Button` ovládacího prvku k zahájení přiřazení.
- A `Label` ovládacího prvku se zobrazí chybová zpráva, pokud přiřazení selže.

Nakonec kód také vytvoří skupinu ovládacích prvků pro použití pro odebrání vybraných kurzů vedených kurzu.

V *InstructorsCourses.aspx.cs*, přidejte using – příkaz:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Přidejte metodu pro naplnění dva rozevírací seznamy, které zobrazují kurzy:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Tento kód získá všechny kurzy od `Courses` entity nastavení a získá kurzy od `Courses` vlastnost navigace `Person` entity pro vybrané instruktorem. Poté určí, které kurzy jsou přiřazeny k této instruktorem a odpovídajícím způsobem naplní rozevírací seznamy.

Přidání obslužné rutiny `Assign` tlačítka `Click` události:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Tento kód získá `Person` entity pro vybrané instruktorem, získá `Course` entity pro vybraný kurz a přidá vybrané kurz `Courses` navigační vlastnost kurzů vedených `Person` entity. Pak uloží změny do databáze a znovu naplní rozevírací seznamy, takže výsledky uvidíte okamžitě.

Přidání obslužné rutiny `Remove` tlačítka `Click` události:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Tento kód získá `Person` entity pro vybrané instruktorem, získá `Course` entity pro vybraný kurz a také odebere vybraného kurzu z `Person` entity `Courses` navigační vlastnost. Pak uloží změny do databáze a znovu naplní rozevírací seznamy, takže výsledky uvidíte okamžitě.

Přidejte kód, který `Page_Load` metodu, která zajišťuje chybové zprávy se nezobrazí, pokud se nezobrazí žádná chyba pro sestavu a přidejte obslužné rutiny pro `DataBound` a `SelectedIndexChanged` událostem Instruktoři rozevíracího seznamu k naplnění kurzy rozevírací seznamy:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Spuštění stránky.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Vyberte instruktorem. <strong>Přiřadit kurz</strong> rozevíracím seznamu zobrazí kurzů, které kurzů vedených není naučit, a <strong>odebrat kurz</strong> rozevíracím seznamu zobrazí kurzů, které kurzů vedených je už přiřazený k. V <strong>přiřadit kurz</strong> Vyberte kurz a potom klikněte na tlačítko <strong>přiřadit</strong>. Kurz přesune <strong>odebrat kurz</strong> rozevíracího seznamu. Vyberte kurz v <strong>odebrat kurz</strong> části a klikněte na tlačítko <strong>odebrat</strong><em>.</em> Kurz přesune <strong>přiřadit kurz</strong> rozevíracího seznamu.

Nyní jste viděli některé další způsoby, jak pracovat se souvisejícími daty. V následujícím kurzu se dozvíte, jak zlepšit udržovatelnosti vaší aplikace pomocí dědičnosti v datovém modelu.

> [!div class="step-by-step"]
> [Předchozí](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [další](the-entity-framework-and-aspnet-getting-started-part-6.md)
