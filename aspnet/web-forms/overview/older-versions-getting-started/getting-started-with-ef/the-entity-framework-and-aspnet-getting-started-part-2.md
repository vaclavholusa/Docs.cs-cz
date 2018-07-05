---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Začínáme s Entity Framework 4.0 Database First a technologie ASP.NET 4 webových formulářů – část 2 | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Frameworku. Ukázková aplikace je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: 476f3e45608bf79a6d2665424eba09cbfccd78fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371100"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Začínáme s Entity Framework 4.0 Database First a 4 webových formulářů ASP.NET – část 2
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Framework 4.0 a Visual Studio 2010. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>Ovládací prvek EntityDataSource

V předchozím kurzu jste vytvořili webovou stránku, databáze a datového modelu. V tomto kurzu jste pracovat `EntityDataSource` ovládací prvek, který technologie ASP.NET poskytuje, aby bylo možné usnadňují práci s datový model Entity Framework. Vytvoříte `GridView` ovládací prvek pro zobrazování a upravování dat studentů `DetailsView` ovládacího prvku pro přidání nové studenty a `DropDownList` ovládací prvek pro výběr oddělení (které budete používat později pro zobrazení související kurzy).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Všimněte si, že v této aplikaci vás nebude přidávat ověření vstupu na stránky, které aktualizace databáze a některé zpracování chyb nebude tak robustní jako by to bylo požadováno v produkční aplikace. Který udržuje kurz, zaměřuje na rozhraní Entity Framework a udržuje v přístupu příliš dlouhý. Podrobnosti o tom, jak přidat tyto funkce do svojí aplikace najdete v tématu [ověřování uživatelského vstupu v ASP.NET Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx) a [zpracování chyb v ASP.NET stránek a aplikací](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Přidání a konfigurace ovládacího prvku EntityDataSource

Zahájíte tím, že nakonfigurujete `EntityDataSource` řízení ke čtení `Person` entit na základě `People` sady entit.

Ujistěte se, že máte Visual Studio otevřete a, že pracujete s projektem, kterou jste vytvořili v části 1. Pokud od vytvoření datového modelu nebo od poslední změny provedené na ni dosud sestavili projekt, sestavte projekt nyní. Změny do datového modelu nejsou k dispozici Návrhář dokud sestavení projektu.

Vytvořit novou webovou stránku pomocí **webový formulář používající stránku předlohy** šablony a pojmenujte ho *Students.aspx*.

[![image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Zadejte *Site.Master* jako stránky předlohy. Všechny stránky, které vytvoříte pro tyto kurzy se pomocí této hlavní stránky.

[![image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

V **zdroj** zobrazovat, přidávat `h2` na nadpis `Content` ovládací prvek s názvem `Content2`, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Z **Data** karty **nástrojů**, přetáhněte `EntityDataSource` ovládací prvek na stránce, klesne pod nadpisem a změnit ID `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Přepnout na **návrhu** zobrazení, klikněte na inteligentní značku ovládací prvek zdroje dat a pak klikněte na tlačítko **konfigurace zdroje dat** ke spuštění **konfigurace zdroje dat** průvodce.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

V **Konfigurovat ObjectContext** kroku průvodce, vyberte **SchoolEntities** hodnotu **připojení s názvem**a vyberte **SchoolEntities**jako **DefaultContainerName** hodnotu. Pak klikněte na tlačítko **Další**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Poznámka: Pokud se zobrazí následující dialogové okno v tomto okamžiku budete muset sestavit projekt před pokračováním.

[![image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

V **Konfigurovat výběr dat** kroku, vyberte **lidé** hodnotu **EntitySetName**. V části **vyberte**, ujistěte se, že **vyberte A** ll zaškrtávací políčko je zaškrtnuto. Vyberte požadované možnosti pro povolit aktualizaci a odstraňování. Jakmile budete hotovi, klikněte na tlačítko **Dokončit**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Konfigurace pravidel databáze, povolíte odstranění

Vytvoříte stránky, který umožňuje uživateli odstranit studenti z `Person` tabulku, která obsahuje tři relace s jinými tabulkami (`Course`, `StudentGrade`, a `OfficeAssignment`). Ve výchozím nastavení, databáze, nebudete odstranění řádku v `Person` Pokud existují související řádky v jedné z jiné tabulky. Můžete ručně odstranit řádky v relaci nejprve, nebo můžete nakonfigurovat databázi k jejich odstranění automaticky při odstranění `Person` řádek. Pro studenty záznamů v tomto kurzu nakonfigurujete databáze, kterou chcete automaticky odstranit související data. Protože mají studenti související řádky jenom ve `StudentGrade` tabulky, je nutné nakonfigurovat pouze jeden z tři relace.

Pokud používáte *School.mdf* souboru, že jste si stáhli z projektu, který se odkazuje v tomto kurzu, tuto část přeskočit, protože tyto změny konfigurace už jsme udělali. Pokud jste vytvořili databázi spuštěním skriptu, nakonfigurujte databázi pomocí následujících postupů.

V **Průzkumníka serveru**, otevřete diagram databáze, kterou jste vytvořili v části 1. Klikněte pravým tlačítkem na vztah mezi `Person` a `StudentGrade` (čáry mezi tabulkami) a pak vyberte **vlastnosti**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

V **vlastnosti** okna rozbalte **INSERT a UPDATE specifikace** a nastavit **DeletRule** vlastnost **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Uložte a zavřete diagramu. Pokud budete vyzváni, zda chcete aktualizovat databázi, klikněte na tlačítko **Ano**.

Pokud chcete mít jistotu, že zajišťuje model entity, které jsou v paměti, které jsou synchronizované s činnosti databáze, musíte nastavit odpovídající pravidla v datovém modelu. Otevřít *SchoolModel.edmx*, pravým tlačítkem myši na Asociační čára mezi `Person` a `StudentGrade`a pak vyberte **vlastnosti**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

V **vlastnosti** okno, nastavte **elementy OnDelete End1** k **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Uložte a zavřete *SchoolModel.edmx* souboru a znovu sestavte projekt.

Obecně platí při změně databáze, máte několik možností, jak synchronizovat model:

- Pro některé typy změn (například přidávání nebo aktualizaci tabulky, zobrazení nebo uložené procedury), klikněte pravým tlačítkem na návrháře a vyberte **aktualizace modelů z databáze** mít návrháře zkontrolujte změny automaticky.
- Znovu vygenerovat datového modelu.
- Proveďte ruční aktualizace podobný následujícímu.

V takovém případě může být znovu vygenerován modelu nebo aktualizaci tabulky ovlivněné změnou vztah, ale pak budete muset znovu provést změnu názvu pole (z `FirstName` k `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Použití ovládacího prvku GridView číst a aktualizovat entity

V této části budete používat `GridView` ovládací prvek zobrazovat, aktualizovat nebo odstranit studentů.

Otevřete nebo přepněte do *Students.aspx* a přepněte se na **návrhu** zobrazení. Z **Data** karty **nástrojů**, přetáhněte `GridView` ovládací prvek vpravo od `EntityDataSource` řídit, pojmenujte ho `StudentsGridView`, klikněte na inteligentní značku a potom vyberte  **StudentsEntityDataSource** jako zdroj dat.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Klikněte na tlačítko **aktualizovat schéma** (klikněte na tlačítko **Ano** Pokud budete vyzváni k potvrzení), potom klikněte na tlačítko **povolit stránkování**, **Povolit řazení**, **Povolit úpravy**, a **Povolit odstranění**.

Klikněte na tlačítko **upravit sloupce**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

V **vybraná pole** okně Odstranit **PersonID**, **LastName**, a **HireDate**. Je obvykle nezobrazí klíč záznamu pro uživatele, datum přijetí se nevztahuje na studenty a můžete jako umístění vyberu obě části názvu v jednom poli, takže vám stačí jedno z polí název.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Vyberte **FirstMidName** pole a potom klikněte na tlačítko **převést toto pole TemplateField**.

Totéž proveďte pro **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Klikněte na tlačítko **OK** a přepněte se do **zdroj** zobrazení. Zbývající změny bude snazší přímo v kódu. `GridView` Řídit kód teď vypadá jako v následujícím příkladu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

První sloupec po příkazu pole je pole šablony, které aktuálně zobrazuje křestní jméno. Změňte kód pro toto pole šablony, aby vypadala jako v následujícím příkladu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

V režimu zobrazení dvou `Label` ovládací prvky zobrazované jméno a příjmení. V režimu úprav jsou k dispozici dvě textová pole, můžete změnit název první a poslední. Stejně jako u `Label` ovládacích prvků v režimu zobrazení, použijete `Bind` a `Eval` výrazy přesně tak, jak jste je sdíleli s ASP.NET ovládací prvky zdroje dat, která se připojují přímo k databázím. Jediným rozdílem je, že zadáváte vlastností entity místo sloupce databáze.

Poslední sloupec je pole šablony, který zobrazí datum registrace. Změňte kód pro toto pole, aby vypadala jako v následujícím příkladu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

V obou zobrazení a režim, úpravě formátovací řetězec "{0, d}" způsobí, že se data mají být zobrazeny ve formátu "krátké datum". (Váš počítač může být nakonfigurovaný k zobrazení tohoto formátu odlišně od obrázky obrazovky je znázorněno v tomto kurzu.)

Všimněte si, že v každém z těchto polí šablona návrháře používat `Bind` výraz ve výchozím nastavení, ale jste změnili, aby `Eval` výrazu v `ItemTemplate` elementy. `Bind` Výraz zpřístupňuje je v `GridView` vlastnosti ovládacího prvku v případě, že budete potřebovat pro přístup k datům v kódu. Na této stránce není potřeba přístup k těmto datům v kódu, abyste mohli používat `Eval`, což je efektivnější. Další informace najdete v tématu [získávají se vaše data mimo ovládací prvky dat](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Úprava značky ovládacího prvku třídu EntityDataSource platí pro zvýšení výkonu

Do značky `EntityDataSource` řídit, odeberte `ConnectionString` a `DefaultContainerName` atributy a nahradíte je s `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atribut. Jedná se o změnu, měli byste si pokaždé, když vytvoříte `EntityDataSource` ovládací prvek, pokud je třeba použít připojení, které se liší od ten, který je pevně zakódovaný ve třídě objektu kontextu. Použití `ContextTypeName` atribut poskytuje následující výhody:

- Lepší výkon. Když `EntityDataSource` inicializuje ovládací prvek modelu data s využitím `ConnectionString` a `DefaultContainerName` atributy, provede další práci pro načtení metadat u každého požadavku. To není nutné v případě, že zadáte `ContextTypeName` atribut.
- Opožděné načtení je zapnutá ve výchozím nastavení ve třídách kontextu generovaný objekt (například `SchoolEntities` v tomto kurzu) v Entity Framework 4.0. To znamená, že navigační vlastnosti jsou načteny se souvisejícími daty automaticky přímo, když je potřebujete. Opožděné načtení je podrobněji vysvětleny dále v tomto kurzu.
- Všechna vlastní nastavení, které jste použili u objektu třídy kontextu (v tomto případě `SchoolEntities` třídy) bude k dispozici pro ovládací prvky, které používají `EntityDataSource` ovládacího prvku. Přizpůsobení třídy objektu kontextu je rozšířená, pro které neplatí v této sérii kurzů. Další informace najdete v tématu [rozšíření Entity Framework vygenerovaných typů](https://msdn.microsoft.com/library/dd456844.aspx).

Značky teď bude vypadat následovně (pořadí vlastností lišit):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening` Atribut odkazuje na funkci, která je potřeba v dřívějších verzích rozhraní Entity Framework, protože sloupce cizích klíčů nebyla vystavena jako vlastnosti entity. Aktuální verze je možné použít *přidružení cizího klíče*, což znamená, že vlastnosti cizího klíče jsou přístupné pro všechny kromě many-to-many přidružení. Pokud vaše entity nemají vlastnosti cizího klíče a ne [komplexní typy](https://msdn.microsoft.com/library/bb738472.aspx), můžete nechat tento atribut nastavený na `False`. Neodebírejte atribut z kódu, protože výchozí hodnota je `True`. Další informace najdete v tématu [sloučení objektů (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Spuštění stránky a zobrazit seznam studenty a zaměstnance (bude filtrovat pouze studentů v dalším kurzu). Křestní jméno a příjmení se zobrazí společně.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Zobrazení seřadit, klikněte na název sloupce.

Klikněte na tlačítko **upravit** do libovolného řádku. Textová pole se zobrazí, kde můžete změnit název první a poslední.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**Odstranit** tlačítko také funguje. Klikněte na tlačítko Odstranit pro řádek, který má datum zápisu a řádek zmizí. (Řádky bez registrace data představují Instruktoři a můžete obdržet chybu referenční integritu. V dalším kurzu můžete filtrovat tento seznam, aby zahrnovala studenty jenom.)

## <a name="displaying-data-from-a-navigation-property"></a>Zobrazení dat z navigační vlastnost

Nyní předpokládejme, že budete chtít vědět, kolik kurzech, které každý student je zaregistrované v. Poskytuje tyto informace v Entity Framework `StudentGrades` navigační vlastnost `Person` entity. Protože návrh databáze nepovoluje student byla zaregistrovaná v kurzu bez nutnosti na podnikové úrovni, přiřazeno, pro účely tohoto kurzu můžete předpokládat této s řádek `StudentGrade` řádkem tabulky, který je přiřazen ke kurzu je stejný jako registrované v kurzu. ( `Courses` Navigační vlastnost je určena pouze pro vyučující.)

Při použití `ContextTypeName` atribut `EntityDataSource` ovládacího prvku, Entity Framework automaticky načte informace pro navigační vlastnost při přístupu k vlastnosti. Tento postup se nazývá *opožděné načtení*. Nicméně to může být neefektivní, protože výsledkem samostatné volání databáze, kterou je potřeba další informace o jednotlivých čase. Pokud potřebujete data z navigační vlastnosti pro každou entitu vrácené `EntityDataSource` ovládacího prvku, je efektivnější načíst související data spolu s samotná entita v jednom volání do databáze. Tento postup se nazývá *předběžné načítání*, a určete předběžné načítání pro navigační vlastnost nastavením `Include` vlastnost `EntityDataSource` ovládacího prvku.

V *Students.aspx*, kterou chcete zobrazit počet kurzů pro všechny studenty, takže předběžné načítání je nejlepší volbou. Pokud byla zobrazení Všichni studenti, ale zobrazuje počet kurzy pouze pro několik z nich (které by vyžadovaly psaním kódu spolu se značkou), opožděné načtení může být lepší volbou.

Otevřete nebo přepněte do *Students.aspx*, přepnout na **návrhu** zobrazit, vyberte možnost `StudentsEntityDataSource`a **vlastnosti** okno sady **zahrnout**vlastnost **StudentGrades**. (Pokud chcete získat více navigačních vlastností, můžete zadat jejich názvů oddělených čárkami, například **StudentGrades, kurzy**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Přepnout na **zdroj** zobrazení. V `StudentsGridView` ovládacího prvku za poslední `asp:TemplateField` prvku, přidejte následující pole nové šablony:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

V `Eval` výrazu, můžete odkazovat na vlastnost navigace `StudentGrades`. Vzhledem k tomu, že tato vlastnost obsahuje kolekce, má `Count` vlastnost, která slouží k zobrazení počtu kurzů, ve kterých se zaregistruje studenta. V pozdějších kurzech uvidíte, jak zobrazit data z navigační vlastnosti, které obsahují jednotlivých entit místo kolekcí. (Všimněte si, že nemůžete použít `BoundField` prvků, které se zobrazují data z navigační vlastnosti.)

Spuštění stránky a nyní uvidíte, kolik kurzy studenta je zaregistrované ve službě.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Použití ovládacího prvku DetailsView pro vložení entity

Dalším krokem je vytvoření stránky, který má `DetailsView` ovládací prvek, který vám umožní přidat nové studenty. Zavřete prohlížeč a pak vytvořit novou webovou stránku pomocí *Site.Master* stránky předlohy. Pojmenujte stránku *StudentsAdd.aspx*a pak přepnout do **zdroj** zobrazení.

Přidejte následující kód, který chcete nahradit existující značky pro `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Tento kód vytvoří `EntityDataSource` ovládací prvek, který je podobná té, kterou jste vytvořili v *Students.aspx*, s výjimkou umožňuje vložení. Stejně jako u `GridView` ovládací prvek vázaných polí `DetailsView` ovládací prvek se kódují přesně tak, jak by se u ovládacího prvku, který se připojuje přímo k databázi, s tím rozdílem, že odkazují vlastností entity. V takovém případě `DetailsView` ovládací prvek se používá pouze pro vkládání řádků, takže jste nastavili výchozí režim na `Insert`.

Spuštění stránky a přidání nového studenta.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Co se stane po vložení nového objektu student, ale pokud nyní spustíte *Students.aspx*, zobrazí se nové informace studentů.

## <a name="displaying-data-in-a-drop-down-list"></a>Zobrazení dat v rozevíracím seznamu

V následujícím postupu budete databind `DropDownList` ovládací prvek do sady s použitím entity `EntityDataSource` ovládacího prvku. V této části kurzu se nebude provádět většinu pomocí tohoto seznamu. V další části ale použijete seznamu umožňuje uživatelům vybrat oddělení Zobrazit kurzy, které jsou přidružené k oddělení.

Vytvoření nové webové stránky s názvem *Courses.aspx*. V **zdroj** zobrazit, přidat nadpis `Content` ovládací prvek, který má název `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

V **návrhu** zobrazovat, přidávat `EntityDataSource` ovládací prvek na stránce stejně jako dříve, tentokrát s názvem `DepartmentsEntityDataSource`. Vyberte **oddělení** jako **EntitySetName** hodnotu a vyberte jenom **DepartmentID** a **název** vlastnosti.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Z **standardní** karty **nástrojů**, přetáhněte `DropDownList` ovládací prvek na stránce, pojmenujte ho `DepartmentsDropDownList`, klikněte na inteligentní značku a vyberte **zvolit zdroj dat** do Spustit **Průvodce konfigurací zdroje dat**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

V **vyberte zdroj dat** kroku, vyberte **DepartmentsEntityDataSource** jako zdroj dat, klikněte na tlačítko **aktualizovat schéma**a pak vyberte **název** jako datové pole k zobrazení a **DepartmentID** jako datové pole hodnota. Klikněte na tlačítko **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Metoda vytvoření datové vazby ovládacího prvku pomocí Entity Framework je stejný jako s ostatními daty ASP.NET ovládací prvky zdroje s výjimkou případů, které jste zadání entit a vlastnosti entity.

Přepnout na **zdroj** zobrazit a přidat "Vyberte oddělení:" bezprostředně před `DropDownList` ovládacího prvku.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Připomínáme, změňte kód pro `EntityDataSource` ovládacího prvku v tuto chvíli nahrazením `ConnectionString` a `DefaultContainerName` atributy s `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atribut. Často je nejlepší počkat, až vytvoříte ovládací prvek vázaný na data, který je propojený s ovládací prvek zdroje dat, před změnou `EntityDataSource` ovládací prvek kódu, protože po provedení změny Návrhář nebude poskytovat vám **aktualizovat Schéma** možnost v ovládacím prvku vázané na data.

Spuštění stránky a z rozevíracího seznamu můžete vybrat oddělení.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Dokončení tohoto postupu Úvod k používání `EntityDataSource` ovládacího prvku. Práce s tímto ovládacím prvkem je obecně nijak neliší od práce s ostatními daty ASP.NET ovládací prvky zdroje, s tím rozdílem, že odkazujete entit a vlastnosti namísto tabulky a sloupce. Jedinou výjimkou je, když chcete získat přístup k vlastnosti navigace. V dalším kurzu uvidíte, že syntaxe pomocí `EntityDataSource` ovládacího prvku může také lišit od jiné ovládací prvky zdroje dat, filtrování, seskupení a řazení dat.

> [!div class="step-by-step"]
> [Předchozí](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [další](the-entity-framework-and-aspnet-getting-started-part-3.md)
