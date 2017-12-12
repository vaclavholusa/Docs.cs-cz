---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: "Pomocí rozhraní Entity Framework 4.0 a ovládacího prvku ObjectDataSource, část 3: řazení a filtrování | Microsoft Docs"
author: tdykstra
description: "Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený Začínáme s řadou kurz Entity Framework 4.0. I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 4accd3381a66bde1f87f0dc7dd95beeb54fcc6a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Pomocí rozhraní Entity Framework 4.0 a ovládacího prvku ObjectDataSource, část 3: řazení a filtrování
====================
podle [tní Dykstra](https://github.com/tdykstra)

> Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený [Začínáme s Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) kurz řady. Pokud nebyla dokončena starší kurzy, jako výchozí bod pro účely tohoto kurzu můžete [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili. Můžete také [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) vytvořené dokončení kurzu řady. Pokud máte dotazy týkající se kurzy, můžete je do příspěvku [ASP.NET Entity Framework fórum](https://forums.asp.net/1227.aspx).


V tomto kurzu předchozí implementována použitému vzoru vícevrstvé webové aplikace, který používá rozhraní Entity Framework a `ObjectDataSource` ovládacího prvku. Tento kurz ukazuje, jak provést řazení a filtrování a zpracování stránek podrobností. Bude potřeba přidat následující vylepšení *Departments.aspx* stránky:

- Textové pole, aby uživatelé mohli vybrat oddělení podle názvu.
- Seznam kurzů pro každé oddělení, které se zobrazí v mřížce.
- Možnost můžete seřadit kliknutím na záhlaví sloupce.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Přidání možnost řazení GridView sloupce

Otevřete *Departments.aspx* stránky a přidejte `SortParameterName="sortExpression"` atribut `ObjectDataSource` ovládací prvek s názvem `DepartmentsObjectDataSource`. (Později vytvoříte `GetDepartments` metoda, která přebírá parametr s názvem `sortExpression`.) Značky pro počáteční značka ovládacího prvku nyní podobá následujícímu příkladu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Přidat `AllowSorting="true"` atribut otevírací značce `GridView` ovládacího prvku. Značky pro počáteční značka ovládacího prvku nyní podobá následujícímu příkladu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

V *Departments.aspx.cs*, nastavit výchozí pořadí řazení, že zavoláte `GridView` ovládacího prvku `Sort` metoda z `Page_Load` metoda:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Ve třídě obchodní logiky nebo třídu úložiště můžete přidat kód, který seřadí nebo filtry. Pokud se ve třídě obchodní logiky, řazení a filtrování pracovních bude provedeno po načtení dat z databáze, protože třída obchodní logiky ve spolupráci s `IEnumerable` objekt vrácený úložiště. Pokud přidáte řazení a filtrování kód ve třídě, úložiště a provést před výrazu LINQ nebo dotaz na objekt byl převeden na `IEnumerable` objektu příkazech bude předána do databáze pro zpracování, které obvykle je efektivnější. V tomto kurzu budete implementovat řazení a filtrování způsobem, který způsobí, že zpracování musí udělat databáze – to znamená, že v úložišti.

Přidání možnosti třídění, musíte přidat nové metody pro úložiště rozhraní a třídy úložiště i o třídě obchodní logiku. V *ISchoolRepository.cs* soubor, přidejte nový `GetDepartments` metody, která přijímá `sortExpression` parametr, který se použije k řazení seznamu oddělení, která je vrácena:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression` Parametr bude určovat tento sloupec seřadit na a směr řazení.

Přidejte kód nové metodu *SchoolRepository.cs* souboru:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Změnit existující bez parametrů `GetDepartments` metoda k volání nové metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

V k testovacímu projektu, přidejte následující metodu do nové *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Pokud se chystáte vytvořit všechny testy jednotek, které závisí na tato metoda vrátí seřazený seznam, musíte před vrácením seřaďte seznam. Můžete nebude vytvářet testy jako je například v tomto kurzu, metoda může vrátit pouze neseřazené seznam oddělení.

V *SchoolBL.cs* soubor, přidejte následující metodu nové na obchodní logiku třídu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Tento kód předá parametr řazení metodu úložiště.

Spustit *Departments.aspx* stránky.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Nyní můžete kliknout na záhlaví libovolného sloupce řadit podle sloupce. Pokud je už seřazený sloupec, kliknutím na záhlaví obrátí směr řazení.

## <a name="adding-a-search-box"></a>Přidání vyhledávací pole

V této části budete přidat textového pole pro vyhledávání, propojovat je se `ObjectDataSource` řízení pomocí parametru řízení a přidání metody do třídy obchodní logiku pro podporu filtrování.

Otevřete *Departments.aspx* stránky a přidejte následující kód mezi záhlaví a první `ObjectDataSource` ovládacího prvku:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

V `ObjectDataSource` ovládací prvek s názvem `DepartmentsObjectDataSource`, postupujte takto:

- Přidat `SelectParameters` element pro parametr s názvem `nameSearchString` hodnota zadaná v, který získá `SearchTextBox` ovládacího prvku.
- Změna `SelectMethod` atribut hodnotu `GetDepartmentsByName`. (Vytvoříte tato metoda později.)

Značku `ObjectDataSource` řízení nyní podobá následujícímu příkladu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

V *ISchoolRepository.cs*, přidejte `GetDepartmentsByName` metody, která přijímá obě `sortExpression` a `nameSearchString` parametry:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

V *SchoolRepository.cs*, přidejte následující metodu nové:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Tento kód používá `Where` metoda vyberte položky, které obsahují řetězec pro hledání. Pokud je řetězec prázdný, budou vybrány všechny záznamy. Všimněte si, že když zadáte metoda volá společně v jednom příkazu takto (`Include`, pak `OrderBy`, pak `Where`), `Where` metoda musí být vždy poslední.

Změnit existující `GetDepartments` metody, která přijímá `sortExpression` parametr volat metodu nové:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

V *MockSchoolRepository.cs* v k testovacímu projektu, přidejte následující metodu nové:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

V *SchoolBL.cs*, přidejte následující metodu nové:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Spustit *Departments.aspx* stránky a zadejte hledaný řetězec pro Ujistěte se, že funguje logiku pro výběr. Ponechte textové pole prázdné a zkuste hledání, ujistěte se, že jsou vráceny všechny záznamy.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Přidat sloupec podrobnosti pro každý řádek tabulky

Dále budete chtít zobrazit všechny kurzy pro každé oddělení zobrazí v pravém buňky v mřížce. K tomuto účelu použijete vnořený `GridView` řízení a databind jeho data z `Courses` vlastnost navigace `Department` entity.

Otevřete *Departments.aspx* a do značky `GridView` řídit, zadejte obslužnou rutinu pro `RowDataBound` událostí. Značky pro počáteční značka ovládacího prvku nyní podobá následujícímu příkladu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Přidejte nový `TemplateField` element po `Administrator` pole šablony:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Tento kód vytvoří vnořený `GridView` ovládací prvek, který se zobrazuje číslo kurzu a název seznam kurzů. Neurčuje zdroje dat, protože budete databind ho v kódu v `RowDataBound` obslužné rutiny.

Otevřete *Departments.aspx.cs* a přidejte následující obslužnou rutinu pro `RowDataBound` událostí:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Tento kód získá `Department` entita z argumenty událostí převede `Courses` navigační vlastnost pro `List` kolekce a databinds vnořeného `GridView` do kolekce.

Otevřete *SchoolRepository.cs* souboru a zadejte přes načítání pro `Courses` navigační vlastnost voláním `Include` metoda v objektu dotazu, který vytvoříte v `GetDepartmentsByName` metoda. `return` Příkaz v `GetDepartmentsByName` metoda nyní podobá následujícímu příkladu.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Spuštění stránky. Kromě řazení a filtrování funkci, která jste přidali dříve prvek GridView nyní zobrazuje podrobnosti o vnořené kurzu pro každé oddělení.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Tím dokončíte Úvod do scénáře řazení, filtrování a podrobností. V dalším kurzu se zobrazí, jak bude zpracováván souběžnosti.

>[!div class="step-by-step"]
[Předchozí](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[další](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
