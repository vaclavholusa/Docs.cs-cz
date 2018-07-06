---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Použití rozhraní Entity Framework 4.0 a ovládací prvek ObjectDataSource, 3. část: řazení a filtrování | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila Začínáme s Entity Framework 4.0 série kurzů. I...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 15d9a7ecd3119af02576cbcf728828353748a513
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806995"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Použití rozhraní Entity Framework 4.0 a ovládací prvek ObjectDataSource, 3. část: řazení a filtrování
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila [Začínáme s Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série kurzů. Pokud nebyla dokončena v předchozích kurzech, jako výchozí bod pro účely tohoto kurzu můžete [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili. Můžete také [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , který vytvoří kompletní série kurzů. Pokud máte dotazy týkající se těchto kurzů, můžete je publikovat [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


V předchozím kurzu jste implementovali úložiště vzor n vrstvá webové aplikace, která používá Entity Framework a `ObjectDataSource` ovládacího prvku. Tento kurz ukazuje, jak provést řazení a filtrování a zpracování scénářů hlavní podrobnosti. Přidejte následující vylepšení *Departments.aspx* stránky:

- Textové pole, chcete-li povolit uživatelům výběr oddělení podle názvu.
- Seznam kurzů pro každé oddělení, který se zobrazí v mřížce.
- Možnost řadit kliknutím na záhlaví sloupců.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Přidání možnosti řazení sloupce GridView

Otevřít *Departments.aspx* stránky a přidat `SortParameterName="sortExpression"` atribut `ObjectDataSource` ovládací prvek s názvem `DepartmentsObjectDataSource`. (Později vytvoříte `GetDepartments` metodu, která přebírá parametr s názvem `sortExpression`.) Značky pro počáteční značka ovládacího prvku teď vypadá podobně jako v následujícím příkladu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Přidat `AllowSorting="true"` atribut otevírací značce `GridView` ovládacího prvku. Značky pro počáteční značka ovládacího prvku teď vypadá podobně jako v následujícím příkladu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

V *Departments.aspx.cs*, nastavit výchozí pořadí řazení voláním `GridView` ovládacího prvku `Sort` metodu z `Page_Load` metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Můžete přidat kód, který seřadí nebo filtry ve třídě obchodní logiku nebo třídu úložiště. Pokud ji uděláte ve třídě obchodní logiky, řazení a filtrování pracovních se provede po načtení dat z databáze, protože třída obchodní logiky pracuje s `IEnumerable` vrácený úložiště. Pokud přidáte řazení a filtrování kód v třídě úložiště a proveďte před výrazu LINQ nebo dotaz na objekt byl převeden na `IEnumerable` objektu, vaše příkazy se budou předávat na databázi pro zpracování, což je obvykle efektivnější. V tomto kurzu budete implementovat řazení a filtrování způsobem, který způsobí, že zpracování, aby prováděla databáze – to znamená, že v úložišti.

Přidání možnosti třídění, musíte přidat nové metody pro úložiště rozhraní a třídy úložiště i jako třída obchodní logiku. V *ISchoolRepository.cs* přidejte nový `GetDepartments` metodu, která přebírá `sortExpression` parametr, který se použije k seřazení seznamu oddělení, která je vrácena:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression` Parametr bude určovat sloupec řazení a směr řazení.

Přidejte kód pro novou metodu pro *SchoolRepository.cs* souboru:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Změnit existující konstruktor bez parametrů `GetDepartments` metoda zavolat novou metodu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

V testovacím projektu přidejte následující novou metodu pro *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Pokud se chystáte vytvořit všechny testy jednotek, které závisí tato metoda vrátí seřazený seznam, je třeba seřadit seznam před jeho vrácením. Můžete nebude vytvářet testy tímto způsobem. v tomto kurzu, metoda může vrátit pouze netříděný seznam oddělení.

V *SchoolBL.cs* přidejte následující novou metodu do třídy obchodní logiky:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Tento kód předá parametr řazení metodu úložiště.

Spustit *Departments.aspx* stránky.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Nyní můžete kliknout na záhlaví libovolného sloupce pro řazení podle sloupce. Pokud sloupec je už seřazený, kliknutím na záhlaví obrátí směr řazení.

## <a name="adding-a-search-box"></a>Přidání vyhledávacího pole

V této části budete přidání textového pole hledání, propojení tak, `ObjectDataSource` řídit pomocí parametru ovládacího prvku a přidejte metodu do třídy obchodní logiky chcete podporovat filtrování.

Otevřít *Departments.aspx* stránku a přidejte následující kód mezi názvem a prvním `ObjectDataSource` ovládacího prvku:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

V `ObjectDataSource` ovládací prvek s názvem `DepartmentsObjectDataSource`, postupujte takto:

- Přidat `SelectParameters` – element pro parametr s názvem `nameSearchString` , který získá hodnota zadaná v `SearchTextBox` ovládacího prvku.
- Změnit `SelectMethod` atribut hodnotu `GetDepartmentsByName`. (Vytvoříte tato metoda později.)

Zápis `ObjectDataSource` ovládací prvek teď vypadá podobně jako v následujícím příkladu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

V *ISchoolRepository.cs*, přidejte `GetDepartmentsByName` metodu, která přebírá obě `sortExpression` a `nameSearchString` parametry:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

V *SchoolRepository.cs*, přidejte následující novou metodu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Tento kód používá `Where` pro výběr položek, které obsahují hledaný řetězec. Pokud hledaný řetězec je prázdný, budou vybrány všechny záznamy. Všimněte si, že při zadání metody volá společně v jednom příkazu takto (`Include`, pak `OrderBy`, pak `Where`), `Where` metoda musí být vždy poslední.

Změnit existující `GetDepartments` metodu, která přebírá `sortExpression` parametr zavolat novou metodu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

V *MockSchoolRepository.cs* v testovacím projektu přidejte následující novou metodu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

V *SchoolBL.cs*, přidejte následující novou metodu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Spustit *Departments.aspx* stránku a zadejte hledaný řetězec, abyste měli jistotu, že funguje logiku výběru. Do textového pole ponechte prázdné a opakujte vyhledávání, abyste měli jistotu, že se vrátí všechny záznamy.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Přidání sloupce podrobností pro každý řádek mřížky

V dalším kroku budete chtít zobrazit všechny kurzy pro každé oddělení zobrazí v pravém buňky v mřížce. K tomuto účelu použijete vnořený `GridView` ovládacího prvku a databind tak data z `Courses` vlastnost navigace `Department` entity.

Otevřít *Departments.aspx* a do značky `GridView` řídit, zadejte obslužnou rutinu pro `RowDataBound` událostí. Značky pro počáteční značka ovládacího prvku teď vypadá podobně jako v následujícím příkladu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Přidat nový `TemplateField` elementu po `Administrator` pole šablony:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Tento kód vytvoří vnořený `GridView` ovládací prvek, který zobrazuje číslo kurzu a název seznamu kurzů. Neurčuje zdroj dat, protože databind, je nutné ho v kódu v `RowDataBound` obslužné rutiny.

Otevřít *Departments.aspx.cs* a přidejte následující obslužnou rutinu `RowDataBound` události:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Tento kód získá `Department` entity z argumenty události, převede `Courses` navigační vlastnost pro `List` kolekce a databinds vnořeného `GridView` do kolekce.

Otevřít *SchoolRepository.cs* souboru a zadejte předběžné načítání pro `Courses` navigační vlastnost voláním `Include` metoda v objektu dotazu, který vytvoříte v `GetDepartmentsByName` metody. `return` Výroky `GetDepartmentsByName` metoda teď vypadá podobně jako v následujícím příkladu.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Spuštění stránky. Kromě řazení a filtrování funkce, která jste přidali dříve ovládací prvek GridView nyní zobrazuje podrobnosti o vnořené kurzu pro každé oddělení.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Dokončení tohoto postupu Úvod do scénáře řazení, filtrování a hlavní podrobnosti. V dalším kurzu zobrazí se vám způsob zpracování souběžnosti.

> [!div class="step-by-step"]
> [Předchozí](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [další](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
