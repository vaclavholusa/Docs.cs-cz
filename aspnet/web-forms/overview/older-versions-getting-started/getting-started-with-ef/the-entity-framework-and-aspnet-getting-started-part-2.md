---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: "Začínáme s databáze Entity Framework 4.0 nejprve a ASP.NET 4 webové formuláře – část 2 | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET používající rozhraní Entity Framework. Vzorová aplikace je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: 4e2a3176aaedccd40ef6b619efa3c4052dd8470b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Začínáme s databáze Entity Framework 4.0 nejprve a 4 webových formulářů ASP.NET - část 2
====================
podle [tní Dykstra](https://github.com/tdykstra)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET pomocí sady Visual Studio 2010 a Entity Framework 4.0. Informace o kurzu řady najdete v tématu [první kurz v řadě](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>Ovládací prvek EntityDataSource

V předchozích kurzu jste vytvořili webovou stránku, databázi a datový model. V tomto kurzu pracujete s `EntityDataSource` ovládací prvek, který technologie ASP.NET poskytuje, aby bylo možné snadno pracovat s datovým modelem Entity Framework. Vytvoříte `GridView` řízení pro zobrazení a úpravy dat student `DetailsView` ovládací prvek pro přidání nové studenty a `DropDownList` ovládací prvek pro výběr oddělení (který později budete používat pro zobrazení přidružené kurzy).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Upozorňujeme, že v této aplikaci můžete nebude přidávat ověření vstupu na stránky, které aktualizace databáze a některé zpracování chyb nebude robustní, jako by byly zapotřebí v produkční aplikaci. Který udržuje tento kurz se zaměřuje na rozhraní Entity Framework a udržuje ho z získávání příliš dlouhý. Podrobnosti o tom, jak do aplikace přidat tyto funkce najdete v tématu [ověření vstupu uživatele na webových stránkách ASP.NET](https://msdn.microsoft.com/en-us/library/7kh55542.aspx) a [zpracování chyb stránek ASP.NET a v aplikacích](https://msdn.microsoft.com/en-us/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Přidání a konfigurace ovládacího prvku EntityDataSource

Budete začněte tím, že konfigurace `EntityDataSource` řízení číst `Person` entity z `People` sady entit.

Ujistěte se, že máte Visual Studio otevřete a že pracujete s projektem jste vytvořili v části 1. Pokud od vytvoření datového modelu nebo od poslední změny provedené na ni, nebyly sestavili projekt, sestavení projektu nyní. Změny do datového modelu nejsou k dispozici návrháře dokud sestavení projektu.

Vytvořit novou webovou stránku pomocí **webového formuláře pomocí stránky předlohy** šablony a pojmenujte ji *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Zadejte *Site.Master* jako stránky předlohy. Použije všechny stránky, které vytvoříte pro tyto kurzy této stránky předlohy.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

V **zdroj** zobrazit, přidat `h2` nadpis k `Content` ovládací prvek s názvem `Content2`, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Z **Data** kartě **sada nástrojů**, přetáhněte `EntityDataSource` řízení na stránku, vyřaďte ji pod nadpisem a změnit ID na `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Přepnout na **návrhu** zobrazení, klikněte na inteligentní značky ovládacího prvku zdroje dat a pak klikněte na tlačítko **konfigurace zdroje dat** ke spuštění **konfigurace zdroje dat** průvodce.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

V **konfigurace ObjectContext** kroku průvodce vyberte **SchoolEntities** hodnotu **připojení s názvem**a vyberte **SchoolEntities**jako **DefaultContainerName** hodnotu. Pak klikněte na tlačítko **Další**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Poznámka: Pokud se v tomto okamžiku zobrazí následující dialogové okno, budete muset sestavení projektu, než budete pokračovat.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

V **Konfigurovat výběr dat** krok, vyberte **osoby** hodnotu **EntitySetName**. V části **vyberte**, zajistěte, aby **vyberte A** udou zaškrtnutí políčka. Vyberte požadované možnosti pro povolit aktualizace a odstranění. Když jste hotovi, klikněte na tlačítko **Dokončit**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Konfigurace pravidel databáze, povolíte odstranění

Budete vytváření stránky, který umožňuje uživatelům odstranit studenty z `Person` tabulky, který má tři relace s jinou tabulkou (`Course`, `StudentGrade`, a `OfficeAssignment`). Ve výchozím nastavení, bude databáze zabránit v odstranění řádku `Person` Pokud nejsou zjištěny související řádky v jednom z jiné tabulky. Můžete ručně odstranit řádky v relaci nejprve, nebo můžete nakonfigurovat databázi můžete odstranit je automaticky, když odstraníte `Person` řádek. Pro záznamy studentů v tomto kurzu budete konfigurovat databáze automaticky odstranit související data. Protože studenty může mít související řádky pouze v `StudentGrade` tabulky, je nutné nakonfigurovat pouze jeden z tři vztahy.

Pokud používáte *School.mdf* souboru, který jste si stáhli z projektu, který prochází v tomto kurzu, protože již byla hotové tyto změny konfigurace, můžete tuto část přeskočit. Pokud jste vytvořili databázi spuštěním skriptu, nakonfigurujte databázi tak, že provedete následující postupy.

V **Průzkumníka serveru**, otevřete diagram databáze, kterou jste vytvořili v části 1. Klikněte pravým tlačítkem na vztah mezi `Person` a `StudentGrade` (řádek mezi tabulkami) a potom vyberte **vlastnosti**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

V **vlastnosti** okně rozbalte **INSERT a UPDATE specifikace** a nastavte **DeletRule** vlastnost **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Uložte a zavřete diagramu. Pokud se dotaz, jestli chcete aktualizovat databázi, klikněte na tlačítko **Ano**.

Abyste měli jistotu, že udržuje modelu entit, které jsou v paměti, které jsou synchronizované s činnosti databáze, musíte nastavit odpovídajících pravidel v datovém modelu. Otevřete *SchoolModel.edmx*, klikněte pravým tlačítkem na řádek přidružení mezi `Person` a `StudentGrade`a potom vyberte **vlastnosti**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

V **vlastnosti** nastavte **End1 OnDelete** k **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Uložte a zavřete *SchoolModel.edmx* souboru a pak znovu vytvořte projekt.

Obecně platí při změně databáze, máte několik možností, jak synchronizovat model:

- Pro určité typy změn (například přidání nebo aktualizace tabulek, zobrazení nebo uložené procedury), klikněte pravým tlačítkem myši v návrháři a vyberte **aktualizovat Model z databáze** tak, aby měl návrháře zkontrolujte změny automaticky.
- Znovu vygenerujte datový model.
- Proveďte ruční aktualizace podobné následujícímu.

V takovém případě může mít znovu vygenerovat modelu nebo aktualizovat tabulky vliv změna relace, ale pak budete muset znovu proveďte požadovanou změnu název pole (z `FirstName` k `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Použití ovládacího prvku GridView číst a aktualizovat entity

V této části budete používat `GridView` řízení zobrazení, aktualizovat nebo odstranit studenty.

Otevřete nebo přepněte do *Students.aspx* a přepněte do **návrhu** zobrazení. Z **Data** kartě **sada nástrojů**, přetáhněte `GridView` řízení napravo od `EntityDataSource` řídit, pojmenujte ji `StudentsGridView`, klikněte na inteligentní značku a potom vyberte  **StudentsEntityDataSource** jako zdroj dat.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Klikněte na tlačítko **aktualizovat schéma** (klikněte na tlačítko **Ano** Pokud se zobrazí výzva k potvrzení), pak klikněte na tlačítko **povolit stránkování**, **Povolit řazení**, **Povolení úprav**, a **povolení odstraňování**.

Klikněte na tlačítko **upravit sloupce**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

V **vybrané pole** pole, odstraňte **PersonID**, **LastName**, a **HireDate**. Je obvykle nezobrazí záznam klíč pro uživatele, datum přijetí není relevantní pro studenty, a budete vložíte obě části názvu do jednoho pole, takže je třeba pouze jeden název pole.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Vyberte **FirstMidName** pole a pak klikněte na **převést toto pole na TemplateField**.

Totéž proveďte pro **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Klikněte na tlačítko **OK** a potom přepnout na **zdroj** zobrazení. Zbývající změny bude jednodušší přímo v kódu. `GridView` Řízení značek nyní vypadá jako v následujícím příkladu.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

První sloupec po příkaz pole je pole šablony, které aktuálně zobrazí křestní jméno. Změňte kód v tomto poli šablonu, aby vypadala jako v následujícím příkladu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

V režimu zobrazení dvě `Label` ovládací prvky zobrazovaný název první a poslední. V režimu úprav dvě textová pole jsou k dispozici, takže můžete změnit název první a poslední. Stejně jako u `Label` zobrazení ovládacích prvků v režimu, je použít `Bind` a `Eval` výrazy přesně tak, jak můžete by s ASP.NET ovládací prvky zdroje dat které se připojují přímo do databáze. Jediným rozdílem je, že zadáváte vlastností entity místo sloupců databáze.

Poslední sloupec je šablona pole, které zobrazuje datum registrace. Změňte kód v tomto poli, aby vypadala jako v následujícím příkladu:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

V obou zobrazení a úpravě režimu řetězec formátu "{0, d}" způsobí, že data se zobrazí ve formátu "krátkodobých data". (Váš počítač může být nakonfigurován k zobrazení tohoto formátu jinak než Image obrazovce uvedené v tomto kurzu.)

Všimněte si, že v každém z těchto polí šablony návrháře používat `Bind` výraz ve výchozím nastavení, ale jste změnili, aby `Eval` výrazu v `ItemTemplate` elementy. `Bind` Výraz zpřístupní data v `GridView` vlastností ovládacího prvku v případě, že potřebujete přístup k datům v kódu. Na této stránce nepotřebujete přístup k těmto datům v kódu, abyste mohli používat `Eval`, což je efektivnější. Další informace najdete v tématu [získávání dat mimo ovládací prvky datových](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Úprava značek ovládacího prvku EntityDataSource ke zlepšení výkonu

Do značky `EntityDataSource` řídit, odeberte `ConnectionString` a `DefaultContainerName` atributy a nahradíte je s `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atribut. Jedná se o změnu, měli byste si pokaždé, když vytvoříte `EntityDataSource` řízení, pokud je potřeba použít připojení, které se liší od ten, který je pevně zakódovaná ve třídě kontextu objektu. Pomocí `ContextTypeName` atribut poskytuje následující výhody:

- Lepší výkon. Když `EntityDataSource` řízení inicializuje data pomocí modelu `ConnectionString` a `DefaultContainerName` atributy, provede načíst metadata u každého požadavku v práci. Tato akce není nutné v případě, že zadáte `ContextTypeName` atribut.
- Opožděného načítání je zapnutá ve výchozím nastavení v objekt generovaný kontext třídy (například `SchoolEntities` v tomto kurzu) v Entity Framework 4.0. To znamená, že navigační vlastnosti jsou načtená související data automaticky přímo v případě potřeby. Opožděného načítání je vysvětlené podrobněji později v tomto kurzu.
- Všechna přizpůsobení, která jste použili na objekt context – třída (v takovém případě `SchoolEntities` třída) bude k dispozici pro ovládací prvky, které používají `EntityDataSource` ovládací prvek. Přizpůsobení třídu objektu kontextu je rozšířená který není součástí tohoto kurzu řady. Další informace najdete v tématu [rozšíření Entity Framework generované typy](https://msdn.microsoft.com/en-us/library/dd456844.aspx).

Kód bude nyní vypadat v následujícím příkladu (pořadí vlastnosti může být odlišné):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening` Atribut odkazuje na funkci, která je potřeba v dřívějších verzích rozhraní Entity Framework, protože nebyly zveřejněné sloupce cizích klíčů jako vlastnosti entity. Aktuální verze je možné použít *přidružení cizího klíče*, což znamená, že vlastnosti cizího klíče jsou viditelné pro všechny ale m: n přidružení. Pokud vaše entity vlastnosti cizího klíče a ne [komplexní typy](https://msdn.microsoft.com/en-us/library/bb738472.aspx), můžete nechat tento atribut nastavit na `False`. Nemáte odeberte atribut z kódu, protože výchozí hodnota je `True`. Další informace najdete v tématu [vyrovnání objekty (EntityDataSource)](https://msdn.microsoft.com/en-us/library/ee404746.aspx).

Spuštění stránky a zobrazte seznam všech studenty a zaměstnanci (budete filtrovat jenom studentů v dalším kurzu). Křestní jméno a příjmení se zobrazují společně.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Chcete-li seřadit zobrazení, klikněte na název sloupce.

Klikněte na tlačítko **upravit** v některém z řádků. Textová pole se zobrazí, kde můžete změnit název první a poslední.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**Odstranit** tlačítko také funguje. Klikněte na tlačítko Odstranit pro řádek, který má datum zápisu a řádek zmizí. (Řádků bez registrace data představují vyučující a může dojít k chybě referenční integrity. V dalším kurzu budete filtr seznamu zahrnete jenom studenty.)

## <a name="displaying-data-from-a-navigation-property"></a>Zobrazení dat z navigační vlastnosti

Nyní předpokládejme, že budete chtít vědět, kolik kurzy každý student zapsán v. Rozhraní Entity Framework poskytuje tyto informace v `StudentGrades` vlastnost navigace `Person` entity. Protože návrh databáze nepovoluje student se můžou registrovat v kurzu bez nutnosti úrovni přiřazen, pro účely tohoto kurzu můžete předpokládat, že má řádek `StudentGrade` řádkem tabulky, který je přidružen ke kurzu je stejný jako během registrace v průběhu. ( `Courses` Navigační vlastnost je jen pro vyučující.)

Při použití `ContextTypeName` atribut `EntityDataSource` řízení, rozhraní Entity Framework automaticky načte informace pro navigační vlastnost při přístupu k této vlastnosti. To se označuje jako *opožděného načítání*. Ale to může být neefektivní, protože výsledkem samostatné volání databáze, kterou je potřeba další informace o jednotlivých času. Pokud potřebujete data z navigační vlastnosti pro každou entitu vrácený `EntityDataSource` ovládací prvek, je efektivnější načíst souvisejících dat společně s samotné entity v jediném volání do databáze. To se označuje jako *přes načítání*, a zadáte přes načítání pro navigační vlastnost nastavením `Include` vlastnost `EntityDataSource` ovládacího prvku.

V *Students.aspx*, které chcete zobrazit počet kurzy pro každý student, takže přes načítání je nejlepší volbou. Pokud byly zobrazení všech studentů ale zobrazující počet kurzy pouze pro několik z nich (které by vyžadovaly psaní některé kódu kromě kód), opožděného načítání může být vhodnější.

Otevřete nebo přepněte do *Students.aspx*, přepněte do **návrhu** zobrazit, vyberte možnost `StudentsEntityDataSource`a v **vlastnosti** sadu okno **zahrnout**vlastnost **StudentGrades**. (Pokud jste chtěli získat více navigační vlastnosti, můžete zadat jejich názvy oddělené čárkami – například **StudentGrades, kurzy**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Přepnout na **zdroj** zobrazení. V `StudentsGridView` ovládacího prvku za poslední `asp:TemplateField` elementu, přidat následující pole nové šablony:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

V `Eval` výrazu, můžete odkazovat navigační vlastnost `StudentGrades`. Vzhledem k tomu, že tato vlastnost obsahuje kolekci, má `Count` vlastnost, která můžete použít k zobrazení počet kurzy, ve kterých je student zapsán. Novější kurzu uvidíte, jak zobrazit data z navigační vlastnosti, které obsahují jednotlivých entit místo kolekcí. (Všimněte si, že nemůžete použít `BoundField` elementy pro zobrazení dat z navigační vlastnosti.)

Spuštění stránky a nyní zobrazí, kolik kurzy student je zaregistrované ve službě.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Použití ovládacího prvku DetailsView vložení entity

Dalším krokem je vytvoření stránky, který má `DetailsView` ovládací prvek, který vám umožní přidat nové studenty. Zavřete prohlížeč a poté vytvořit novou webovou stránku pomocí *Site.Master* stránky předlohy. Název stránky *StudentsAdd.aspx*a potom přepnout na **zdroj** zobrazení.

Přidejte následující značku nahradit existující kód pro `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Tento kód vytvoří `EntityDataSource` ovládací prvek, který je podobnou té, kterou jste vytvořili v *Students.aspx*, s výjimkou umožňuje vložení. Stejně jako u `GridView` řídit, vázaných polí `DetailsView` řízení jsou programového přesně tak, jak by se data ovládacího prvku, který připojuje přímo k databázi, s tím rozdílem, že jejich vlastnosti entity odkazovat. V takovém případě `DetailsView` řízení se používá pouze pro vložení řádků, takže jste nastavili výchozí režim `Insert`.

Spuštění stránky a přidejte nový student.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Není provedena žádná akce po vložení nové studenty, ale pokud nyní spustíte *Students.aspx*, zobrazí se nové informace o student.

## <a name="displaying-data-in-a-drop-down-list"></a>Zobrazení dat v rozevíracím seznamu

V následujících krocích budete databind `DropDownList` ovládacího prvku na entitu nastavit pomocí `EntityDataSource` ovládacího prvku. V této části kurzu nebude provádět většinu pomocí tohoto seznamu. V další části ale budete používat seznamu tak, aby uživatelé vyberte oddělení zobrazíte kurzy přidružené k oddělení.

Vytvořit novou webovou stránku s názvem *Courses.aspx*. V **zdroj** zobrazit, přidat nadpis `Content` ovládací prvek, který je pojmenován `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

V **návrhu** zobrazit, přidat `EntityDataSource` ovládacího prvku na stránku jako jste to udělali dřív, ale s výjimkou název `DepartmentsEntityDataSource`. Vyberte **oddělení** jako **EntitySetName** hodnotu a vyberte pouze **DepartmentID** a **název** vlastnosti.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Z **standardní** kartě **sada nástrojů**, přetáhněte `DropDownList` řízení na stránku, pojmenujte ji `DepartmentsDropDownList`, klikněte na inteligentní značky a vyberte **zvolit zdroj dat** na spuštění **Průvodce konfigurací zdroje dat**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

V **zvolit zdroj dat** krok, vyberte **DepartmentsEntityDataSource** jako zdroj dat, klikněte na tlačítko **aktualizovat schéma**a potom vyberte **název** jako pole data k zobrazení a **DepartmentID** jako pole hodnoty dat. Click **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Metodu, kterou používáte vázání dat ovládacího prvku pomocí rozhraní Entity Framework je stejný jako s jinými daty ASP.NET se zdroj ovládací prvky s výjimkou je určení entit a vlastnosti entity.

Přepnout na **zdroj** zobrazení a přidání "Vyberte oddělení:" bezprostředně před `DropDownList` ovládacího prvku.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Připomínáme, změňte značku `EntityDataSource` ovládací prvek v tomto okamžiku nahrazením `ConnectionString` a `DefaultContainerName` atributy s `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atribut. Je často nejlepším řešením počkejte po vytvoření vázané na data ovládací prvek, který je propojený na prvek zdroje dat, než změníte `EntityDataSource` řízení značek, protože po provedení změny návrháře nebude poskytovat vám **aktualizovat Schéma** možnost v ovládacím prvku vázané na data.

Spuštění stránky a z rozevíracího seznamu můžete vybrat oddělení.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Tím dokončíte Úvod k použití `EntityDataSource` ovládacího prvku. Práce s Tento ovládací prvek je obecně nijak neliší od práce s ostatními daty ASP.NET ovládací prvky zdroje, s tím rozdílem, že odkazujete entit a vlastnosti místo tabulky a sloupce. Jedinou výjimkou je, pokud chcete přístup k navigační vlastnosti. V dalším kurzu se zobrazí, že syntaxe, můžete používat s `EntityDataSource` řízení může se také liší od jiných ovládacích prvků zdroje dat při filtrování, skupiny a řazení dat.

>[!div class="step-by-step"]
[Předchozí](the-entity-framework-and-aspnet-getting-started-part-1.md)
[další](the-entity-framework-and-aspnet-getting-started-part-3.md)
