---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: "Začínáme s databáze Entity Framework 4.0 nejprve a ASP.NET 4 webové formuláře – část 4 | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET používající rozhraní Entity Framework. Vzorová aplikace je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 06d129384fc78db21ad1b9224781deab6a0e91a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Začínáme s databáze Entity Framework 4.0 nejprve a 4 webových formulářů ASP.NET - část 4
====================
podle [tní Dykstra](https://github.com/tdykstra)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET pomocí sady Visual Studio 2010 a Entity Framework 4.0. Informace o kurzu řady najdete v tématu [první kurz v řadě](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Práce s související Data

V předchozích kurzu jste použili `EntityDataSource` řízení k filtrování, řazení a data skupiny. V tomto kurzu můžete zobrazit a aktualizovat data v relaci.

Vytvoříte vyučující stránky, která obsahuje seznam vyučující. Když vyberete lektorem, zobrazí se seznam kurzů výukové podle této lektorem. Když vyberete kurzu, najdete v podrobnostech během a seznam studenty zařazen do kurzu. Můžete upravit název lektorem, datum přijetí a přiřazení office. Přiřazení office je sada samostatné entity, které přistupujete prostřednictvím navigační vlastnost.

Hlavní data můžete propojit podrobná data v kódu nebo v kódu. V této části kurzu budete používat obě metody.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Zobrazení a aktualizace entit v relaci v ovládacím prvku GridView

Vytvořit novou webovou stránku s názvem *Instructors.aspx* používající *Site.Master* hlavní stránky a přidejte následující kód do `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Tento kód vytvoří `EntityDataSource` ovládací prvek, který vybere vyučující a umožňuje aktualizace. `div` Element konfiguruje značky k vykreslení na levé straně, aby sloupec na pravé straně můžete přidat později.

Mezi `EntityDataSource` kódu a zavření `</div>` značky, přidejte následující kód, který vytvoří `GridView` řízení a `Label` ovládací prvek, který budete používat pro chybové zprávy:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

To `GridView` řízení umožňuje výběr řádků, označuje vybraný řádek s světla šedou barvu a určuje obslužné rutiny (které vytvoříte později) `SelectedIndexChanged` a `Updating` události. Určuje také `PersonID` pro `DataKeyNames` vlastnost tak, aby hodnota klíče vybraného řádku můžete předat na jiný ovládací prvek, který bude potřeba přidat později.

Poslední sloupec obsahuje přiřazení office lektorem, která je uložena v vlastnost navigace `Person` entity protože pochází z související entity. Všimněte si, že `EditItemTemplate` určuje element `Eval` místo `Bind`, protože `GridView` ovládacího prvku nelze vytvořit vazbu přímo na navigační vlastnosti za účelem jejich aktualizace. Budete aktualizovat office přiřazení v kódu. Kvůli tomu budete potřebovat odkaz na `TextBox` řízení a budete získat a že v Uložit `TextBox` ovládacího prvku `Init` událostí.

Následující `GridView` ovládacího prvku `Label` ovládací prvek, který se používá pro chybové zprávy. Ovládacího prvku `Visible` vlastnost je `false`, a stav zobrazení je vypnutý, tak, aby popisek se zobrazí, pouze pokud kód je viditelný v odpovědi na chybu.

Otevřete *Instructors.aspx.cs* souboru a přidejte následující `using` příkaz:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Přidáte pole Soukromá třída okamžitě po deklaraci název částečné třídy pro uložení odkaz na textového pole přiřazení office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Přidat zástupnou proceduru pro `SelectedIndexChanged` obslužné rutiny události, že přidáte kód pro pozdější. Také přidat obslužnou rutinu pro přiřazení office `TextBox` ovládacího prvku `Init` událostí tak, aby uchováváte odkaz na `TextBox` ovládacího prvku. Tento odkaz budete používat k získání hodnoty zadané uživatelem za účelem aktualizace entity přidružené navigační vlastnost.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Budete používat `GridView` ovládacího prvku `Updating` událost aktualizace `Location` vlastnosti přidruženého `OfficeAssignment` entity. Přidejte následující obslužnou rutinu pro `Updating` událostí:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Tento kód se spustí, když uživatel klikne **aktualizace** v `GridView` řádek. Kód používá technologii LINQ to Entities načíst `OfficeAssignment` entity, který je spojen s aktuálním `Person` entity, pomocí `PersonID` vybraného řádku z argumentu události.

Jeden z následujících akcí v závislosti na hodnotě v kód pak dojde `InstructorOfficeTextBox` ovládacího prvku:

- Pokud se textové pole má hodnotu a je žádné `OfficeAssignment` entity, které chcete aktualizovat, vytvoří jeden.
- Pokud textového pole má hodnotu a dojde `OfficeAssignment` entity, aktualizuje `Location` hodnotu vlastnosti.
- Pokud jsou textové pole je prázdné a `OfficeAssignment` entita existuje, odstraní entitu.

Po této uloží změny do databáze. Pokud dojde k výjimce, zobrazí chybovou zprávu.

Spuštění stránky.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Klikněte na tlačítko **upravit** a změnit všechna pole do textových polí.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Změna těchto hodnot, včetně **Office přiřazení**. Klikněte na tlačítko **aktualizace** a uvidíte provedené změny v seznamu.

## <a name="displaying-related-entities-in-a-separate-control"></a>Zobrazení entit v relaci v samostatných ovládacího prvku

Každý lektorem můžete naučit jeden nebo více kurzy, takže přidáte `EntityDataSource` řízení a `GridView` ovládacího prvku seznam kurzy spojené s kteroukoli lektorem je vybraný v vyučující `GridView` ovládacího prvku. K vytvoření nadpisu a `EntityDataSource` řízení pro kurzy entity, přidejte následující kód mezi chybová zpráva `Label` řízení a zavření `</div>` značky:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where` Parametr obsahuje hodnotu `PersonID` z lektorem, jejichž řádek je vybrán v `InstructorsGridView` ovládacího prvku. `Where` Vlastnost obsahuje výběru příkaz, který získá všechny přidružené `Person` entity z `Course` entity `People` navigační vlastnost a vybere `Course` entity jenom Pokud jeden z přidružených `Person`entity obsahuje vybranou `PersonID` hodnotu.

K vytvoření `GridView` řízení. přidejte následující kód bezprostředně po `CoursesEntityDataSource` ovládací prvek (před uzavírací `</div>` značka):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Protože žádné kurzy se zobrazí, pokud je vybrána žádná lektorem, `EmptyDataTemplate` element je součástí.

Spuštění stránky.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Vyberte lektorem, který má jeden nebo více kurzy přiřazen, a kurz nebo kurzy, které jsou uvedeny v tomto seznamu. (Poznámka: Přestože schéma databáze umožňuje více kurzy, v testovací dat uvedených v databázi žádná lektorem ve skutečnosti má více než jeden kurzu. Můžete přidat kurzy k databázi sami pomocí **Průzkumníka serveru** okno nebo *CoursesAdd.aspx* stránky, které přidáte novější kurzu.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView` Ovládací prvek se zobrazuje pouze několik polí kurzu. Pokud chcete zobrazit všechny podrobnosti kurzu, budete používat `DetailsView` ovládací prvek pro kurz, který si uživatel vybere. V *Instructors.aspx*, přidejte následující kód po zavření `</div>` značky (ujistěte se, umístěte tento kód **po** div uzavírací značku neplatný před):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Tento kód vytvoří `EntityDataSource` ovládací prvek, který je vázána `Courses` sady entit. `Where` Vlastnost vybere kurzu pomocí `CourseID` hodnotu vybraného řádku na kurzech `GridView` ovládacího prvku. Určuje kód obslužné rutiny pro `Selected` událost, která později budete používat pro zobrazení známek studentů, což je jinou úroveň níže v hierarchii.

V *Instructors.aspx.cs*, vytvořte následující se zakázaným inzerováním pro `CourseDetailsEntityDataSource_Selected` metoda. (Budete vyplnit tento se zakázaným inzerováním později v tomto kurzu, momentálně se to potřebujete, aby se zkompilování a spuštění stránky.)

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Spuštění stránky.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Původně nejsou žádné podrobnosti o kurzu vzhledem k tomu, že je vybrána žádná kurzu. Vyberte lektorem, který má kurzu přiřazen a pak vyberte kurzu a zobrazit podrobnosti.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Pomocí EntityDataSource "vybranou" událost pro zobrazení souvisejících dat

Nakonec chcete zobrazit všechny zaregistrovaná studenty a jejich tříd pro vybraný kurz. K tomuto účelu použijete `Selected` události `EntityDataSource` ovládací prvek vázán ke kurzu `DetailsView`.

V *Instructors.aspx*, přidejte následující kód po `DetailsView` ovládacího prvku:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Tento kód vytvoří `ListView` ovládací prvek, který zobrazí seznam studenty a jejich tříd pro vybraný kurz. Není zadán žádný zdroj dat, protože budete databind ovládacího prvku v kódu. `EmptyDataTemplate` Element poskytuje zprávu zobrazit, pokud je vybrána žádná kurzu – v takovém případě neexistují žádné studenty k zobrazení. `LayoutTemplate` Element vytvoří tabulky HTML k zobrazení seznamu a `ItemTemplate` určuje sloupce k zobrazení. Student ID a student úrovni jsou z `StudentGrade` entity a název student je z `Person` entita, která zpřístupňuje rozhraní Entity Framework v `Person` vlastnost navigace `StudentGrade` entity.

V *Instructors.aspx.cs*, nahraďte prázdná out `CourseDetailsEntityDataSource_Selected` metoda následujícím kódem:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

Vybraná data ve formě kolekci, která má nulové položky, pokud není nic vybráno nebo o jednu položku poskytuje argumentu události pro tuto událost, pokud `Course` entity je vybrána. Pokud `Course` entity je vybraná, kód používá `First` způsobů, jak převést kolekci pro jediný objekt. Potom získá `StudentGrade` entity z navigační vlastnost, je převede na kolekci a váže `GradesListView` ovládacího prvku do kolekce.

Toto je dostatečná k zobrazení tříd, ale chcete zajistit, že zpráva v šabloně prázdné data se zobrazí stránka se zobrazí v prvním a vždy, když není vybraná kurzu. To lze provést, vytvořte následující metodu, která budete volat ze dvou míst:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Tato nová metoda z volání `Page_Load` metodu pro zobrazení doby šablony první dat prázdná stránka se zobrazí. A volat z `InstructorsGridView_SelectedIndexChanged` metoda vzhledem k tomu, že událost se vyvolá, když je vybrána lektorem, což znamená, že nové kurzy jsou načteny do kurzy `GridView` řízení a žádná zatím není vybraná. Zde jsou dvě volání:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Spuštění stránky.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Vyberte lektorem, který má kurzu přiřazené a pak vyberte kurzu.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Nyní jste viděli několik způsobů, jak pracovat s související data. V následujícím kurzu budete informace o postupu přidání vztahy mezi existující entity, jak odebrat relace a postup přidání nové entity, která má vztah na stávající entitu.

>[!div class="step-by-step"]
[Předchozí](the-entity-framework-and-aspnet-getting-started-part-3.md)
[další](the-entity-framework-and-aspnet-getting-started-part-5.md)
