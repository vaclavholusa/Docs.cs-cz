---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Začínáme s Entity Framework 4.0 Database First a technologie ASP.NET 4 webových formulářů – 4. část | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Frameworku. Ukázková aplikace je...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 5f8b1c15fbfd2d65b603013db3902b42faa40665
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836785"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Začínáme s Entity Framework 4.0 Database First a 4 webových formulářů ASP.NET – 4. část
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Framework 4.0 a Visual Studio 2010. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Práce s souvisejících dat

V předchozím kurzu jste použili `EntityDataSource` ovládacího prvku k filtrování, řazení a seskupení dat. V tomto kurzu budete zobrazit a aktualizovat související data.

Vytvoříte Instruktoři stránku, která zobrazuje seznam Instruktoři. Když vyberete instruktorem, zobrazí se seznam kurzy vedené této instruktorem. Při výběru kurz se zobrazí podrobnosti o kurz a seznam studentů zaregistrovaný do kurzu. Můžete upravit název instruktorem, datum přijetí a přiřazení kanceláře. Přiřazení office je sada entit, ke které přistupujete prostřednictvím navigační vlastnost.

Můžete propojit s hlavními daty podrobná data v kódu nebo v kódu. V této části kurzu budete používat obě metody.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Zobrazení a aktualizace související entity v ovládacím prvku GridView

Vytvoření nové webové stránky s názvem *Instructors.aspx* , která používá *Site.Master* stránku předlohy a přidejte následující kód k `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Tento kód vytvoří `EntityDataSource` ovládací prvek, který vybere Instruktoři a umožňuje aktualizace. `div` Element konfiguruje značky k vykreslení na levé straně, kde sloupec na pravé straně můžete přidat později.

Mezi `EntityDataSource` kódu a zavření `</div>` značky, přidejte následující kód, který vytváří `GridView` ovládacího prvku a `Label` ovládací prvek, který budete používat pro chybové zprávy:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

To `GridView` ovládací prvek umožňuje výběr řádku, zvýrazní vybraný řádek s malým šedou barvu a určuje obslužné rutiny (které vytvoříte později) `SelectedIndexChanged` a `Updating` události. Určuje také `PersonID` pro `DataKeyNames` vlastnosti, tak, aby hodnota klíče vybraného řádku může být předán jiný ovládací prvek, který přidáte později.

Obsahuje přiřazení kanceláře instruktorem, který je uložený ve vlastnosti navigace z posledního sloupce `Person` entity protože pochází z související entity. Všimněte si, že `EditItemTemplate` prvek určuje `Eval` místo `Bind`, protože `GridView` ovládací prvek nemůže vytvořit vazbu přímo na vlastnosti navigace za účelem jejich aktualizace. Budete aktualizovat přiřazení kanceláře v kódu. K tomu budete potřebovat odkaz na `TextBox` ovládací prvek a budete získat a uložit v `TextBox` ovládacího prvku `Init` událostí.

Následující `GridView` je ovládací prvek `Label` ovládací prvek, který se používá pro chybové zprávy. Ovládací prvek `Visible` vlastnost `false`, a stav zobrazení je vypnutý, takže popisku se zobrazí, pouze když kód umožněno její zobrazení v odpovědi na chybu.

Otevřít *Instructors.aspx.cs* soubor a přidejte následující `using` – příkaz:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Přidejte pole soukromé třídy hned po deklaraci částečné třídy název k uložení odkazu do textového pole přiřazení sady office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Zástupné procedury pro přidání `SelectedIndexChanged` obslužná rutina události, že přidáte kód pro pozdější. Také přidejte obslužnou rutinu pro přiřazení kanceláře `TextBox` ovládacího prvku `Init` události tak, aby uchováváte odkaz na `TextBox` ovládacího prvku. Použijete tento odkaz k získání hodnoty za účelem aktualizace entity přidružené navigační vlastnosti zadané uživatelem.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Budete používat `GridView` ovládacího prvku `Updating` událost k aktualizaci `Location` vlastnosti přidruženého `OfficeAssignment` entity. Přidejte následující obslužnou rutinu pro `Updating` události:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Tento kód se spustí, když uživatel klikne **aktualizace** v `GridView` řádek. Kód používá technologii LINQ to Entities k načtení `OfficeAssignment` entity, který je spojen s aktuálním `Person` entity, pomocí `PersonID` vybraného řádku z argumentu události.

Kód provede jednu z následujících akcí v závislosti na hodnotě v `InstructorOfficeTextBox` ovládacího prvku:

- Pokud textové pole má hodnotu a neexistuje žádná `OfficeAssignment` entitu, kterou chcete aktualizovat, ho vytvoří.
- Pokud textové pole má hodnotu a dojde `OfficeAssignment` entity, aktualizuje `Location` hodnotu vlastnosti.
- Pokud textové pole je prázdné a `OfficeAssignment` entita existuje, odstraní entitu.

Potom ho uloží změny do databáze. Pokud dojde k výjimce, zobrazí chybovou zprávu.

Spuštění stránky.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Klikněte na tlačítko **upravit** a změnit všechna pole do textových polí.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Změňte některou z těchto hodnot, včetně **přiřazení kanceláře**. Klikněte na tlačítko **aktualizace** a zobrazí se změny projeví v seznamu.

## <a name="displaying-related-entities-in-a-separate-control"></a>Zobrazení souvisejících entit v samostatném ovládacím prvku

Každý kurzů vedených můžete představuje jeden či více kurzů, tak přidáte `EntityDataSource` ovládacího prvku a `GridView` ovládacího prvku na seznam kurzů spojené s instruktorem, podle toho, která je vybrána v Instruktoři `GridView` ovládacího prvku. K vytvoření nadpisu a `EntityDataSource` ovládací prvek pro entity, kurzy, přidejte následující kód mezi chybová zpráva `Label` ovládacího prvku a uzavírací `</div>` značky:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where` Parametru obsahuje hodnotu `PersonID` z kurzů vedených výběru jehož řádku v `InstructorsGridView` ovládacího prvku. `Where` Vlastnost obsahuje výběru příkaz, který získá všechny přidružené `Person` entit na základě `Course` entity `People` navigační vlastnost a vybere `Course` entity pouze tehdy, pokud jeden z přidružených `Person`obsahuje vybrané entity `PersonID` hodnotu.

Chcete-li vytvořit `GridView` ovládacího prvku. přidejte následující kód bezprostředně `CoursesEntityDataSource` ovládacího prvku (před uzavírací značku `</div>` značky):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Vzhledem k tomu, že žádné kurzů se zobrazí, pokud je vybrána žádná instruktorem, `EmptyDataTemplate` element je součástí.

Spuštění stránky.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Vyberte instruktorem, který má jeden či více kurzů přiřazené a kurzu nebo kurzů se zobrazí v seznamu. (Poznámka: i když je schéma databáze umožňuje více, v testu dat uvedených v databázi žádná kurzů vedených ve skutečnosti má více než jeden kurz. Můžete přidat kurzy k databázi sami pomocí **Průzkumníka serveru** okno nebo *CoursesAdd.aspx* stránky, které přidáte v pozdějších kurzech.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView` Ovládací prvek zobrazuje jenom několik polí kurzu. Chcete-li zobrazit všechny podrobnosti o kurzu, budete používat `DetailsView` ovládací prvek pro kurz, který uživatel vybere. V *Instructors.aspx*, přidejte následující kód za uzavírací `</div>` značky (ujistěte se, že umístěte tento kód **po** div uzavírací značku, ne před):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Tento kód vytvoří `EntityDataSource` ovládací prvek, který je vázán `Courses` sady entit. `Where` Vlastnost vybere kurz pomocí `CourseID` hodnotu vybraný řádek v kurzy `GridView` ovládacího prvku. Určuje kód obslužné rutiny pro `Selected` událost, které budete používat později pro zobrazení známek studentů, což je další úroveň v hierarchii níže.

V *Instructors.aspx.cs*, vytvořte následující zástupné procedury pro `CourseDetailsEntityDataSource_Selected` metody. (Budete vyplníte tuto zástupnou proceduru později v tomto kurzu, teď ho potřebujete, aby se na stránce kompilace a spuštění)

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Spuštění stránky.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Zpočátku nejsou žádné podrobnosti o kurzu protože není vybrána žádná kurzu. Vyberte instruktorem, který má přiřazené kurz a pak vyberte kurzu zobrazíte podrobnosti.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Používat EntityDataSource "vybranou" událost pro zobrazení souvisejících dat

A konečně budete chtít zobrazit všechny registrovaná studentů a jejich známek pro vybraný kurz. K tomuto účelu použijete `Selected` událost `EntityDataSource` ovládací prvek vázán ke kurzu `DetailsView`.

V *Instructors.aspx*, přidejte následující kód za `DetailsView` ovládacího prvku:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Tento kód vytvoří `ListView` ovládací prvek, který zobrazí seznam studentů a jejich známek pro vybraný kurz. Není zadán žádný zdroj dat, protože budete databind ovládacího prvku v kódu. `EmptyDataTemplate` Element poskytuje zpráva má zobrazit, když je vybrána žádná kurzu – v takovém případě nejsou žádní studenti. Chcete-li zobrazit. `LayoutTemplate` Element vytvoří tabulku HTML k zobrazení seznamu a `ItemTemplate` určuje sloupce, které chcete zobrazit. ID studenta a studenta způsobují `StudentGrade` entity a jméno studenta je z `Person` entita, která je k dispozici v Entity Framework `Person` navigační vlastnost `StudentGrade` entity.

V *Instructors.aspx.cs*, nahraďte podložit out `CourseDetailsEntityDataSource_Selected` metodu s následujícím kódem:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

Argument události pro tuto událost poskytuje vybraná data v podobě kolekce, který má nulové položky, pokud není nic vybráno nebo jednu položku Pokud `Course` vybrané entity. Pokud `Course` vybraná entita, tento kód použije `First` způsobů, jak převést kolekci na jeden objekt. Potom získá `StudentGrade` entity z navigační vlastnost, převede je do kolekce a vytvoří vazbu `GradesListView` ovládacího prvku do kolekce.

To stačí k zobrazení tříd, ale můžete chtít mít jistotu, že zpráva v šabloně prázdné data se zobrazí stránka se zobrazí v prvním a vždy, když není vybrán kurzu. K tomuto účelu vytvořte následující metodu, která budete volat ze dvou míst:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Tato nová metoda z volání `Page_Load` metodu pro zobrazení prázdných datových čas šablony první stránka se zobrazí. A volejte jej z `InstructorsGridView_SelectedIndexChanged` metoda vzhledem k tomu, že tuto událost se vyvolá, když je vybrána instruktorem, což znamená, že nové kurzy se načtou do kurzy `GridView` ovládacího prvku a žádné zatím není vybraná. Tady jsou dvě volání:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Spuštění stránky.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Vyberte instruktorem, který má kurzu přiřazen a pak vyberte kurzu.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Nyní jste viděli několik způsobů, jak pracovat se souvisejícími daty. V následujícím kurzu se dozvíte víc o přidání relace mezi existující entity, jak odebrat relace a tom, jak přidat novou entitu, která má vztah k existující entity.

> [!div class="step-by-step"]
> [Předchozí](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [další](the-entity-framework-and-aspnet-getting-started-part-5.md)
