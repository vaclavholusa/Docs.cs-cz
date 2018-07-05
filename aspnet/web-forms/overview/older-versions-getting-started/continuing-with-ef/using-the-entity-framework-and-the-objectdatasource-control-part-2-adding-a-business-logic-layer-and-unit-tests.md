---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Použití rozhraní Entity Framework 4.0 a ovládací prvek ObjectDataSource, 2. část: přidání vrstvy obchodní logiky a testy jednotek | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila Začínáme s Entity Framework 4.0 série kurzů. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 02f0b86203eb879ca618655b8956f22dc67858cd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394239"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Použití rozhraní Entity Framework 4.0 a ovládací prvek ObjectDataSource, 2. část: přidání vrstvy obchodní logiky a testy jednotek
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila [Začínáme s Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série kurzů. Pokud nebyla dokončena v předchozích kurzech, jako výchozí bod pro účely tohoto kurzu můžete [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili. Můžete také [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , který vytvoří kompletní série kurzů. Pokud máte dotazy týkající se těchto kurzů, můžete je publikovat [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


V předchozím kurzu jste vytvořili vícevrstvou webovou aplikaci pomocí rozhraní Entity Framework a `ObjectDataSource` ovládacího prvku. Tento kurz ukazuje, jak přidat obchodní logiky a zajistit přitom ochranu vrstvy obchodní logiky (BLL) a přístup k datům layer (DAL) samostatné a ukazuje, jak vytvořit automatizované testy jednotky pro BLL.

V tomto kurzu budete provádět následující úlohy:

- Vytvoření rozhraní úložiště, který deklaruje metody přístupu k datům, které potřebujete.
- Implementujte rozhraní úložiště v třídě úložiště.
- Vytvořte třídu obchodní logiky, který zavolá třídu úložiště k provádění funkcí přístup k datům.
- Připojení `ObjectDataSource` ovládací prvek do třídy obchodní logiku místo pro třídu úložiště.
- Vytvořte projekt testování částí a třídy úložiště, který používá kolekce v paměti pro své datové úložiště.
- Vytvoření testování částí pro obchodní logiku, kterou chcete přidat do třídy obchodní logiku, pak spusťte test a prohlédněte si nezdaří.
- Implementovat obchodní logiku v třídě obchodní logiku, a poté znovu spusťte test jednotky a podívat, jak to předat.

Budete pracovat *Departments.aspx* a *DepartmentsAdd.aspx* stránky, které jste vytvořili v předchozím kurzu.

## <a name="creating-a-repository-interface"></a>Vytvoření rozhraní pro úložiště

Zobrazí za přibližně tak, že vytvoříte rozhraní úložiště.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

V *DAL* složku, vytvořte nový soubor třídy, pojmenujte ho *ISchoolRepository.cs*a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

Rozhraní definuje jednu metodu pro každou CRUD (vytváření, čtení, aktualizace nebo odstranění) metody, které jste vytvořili v třídě úložiště.

V `SchoolRepository` třídy v *SchoolRepository.cs*, označení, že tato třída implementuje `ISchoolRepository` rozhraní:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Vytvoření třídy obchodní logiky

V dalším kroku vytvoříte třídu obchodní logiku. To provedete tak, aby můžete přidat obchodní logiky, která se spustí `ObjectDataSource` řídit, přestože to není, ještě. Prozatím novou třídu obchodní logiku pouze provede stejné operace CRUD, které nemá úložiště.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Vytvořte novou složku s názvem *BLL*. (V reálné aplikaci byste vrstvy obchodní logiky obvykle implementován jako knihovna tříd – samostatný projekt, ale pro zjednodušení tento kurz se zachová BLL třídy ve složce projektu.)

V *BLL* složku, vytvořte nový soubor třídy, pojmenujte ho *SchoolBL.cs*a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Tento kód vytvoří stejné metody CRUD, které jste viděli dříve v třídě úložiště, ale ne přístup přímo metody rozhraní Entity Framework, volá úložiště metody třídy.

Proměnné třídy, která obsahuje odkaz na třídu úložiště je definována jako typ rozhraní a kód, který vytvoří instanci třídy úložiště je obsažený v dva konstruktory. Konstruktor bez parametrů použije `ObjectDataSource` ovládacího prvku. Vytvoří instanci `SchoolRepository` třídu, která jste vytvořili dříve. Další konstruktor umožňuje jakýkoli kód, který vytvoří instanci třídy obchodní logiky a zajistěte tak předání libovolný objekt, který implementuje rozhraní úložiště.

CRUD metody, které volají třídu úložiště a dva konstruktory umožňují použít třídu obchodní logiky pomocí libovolné úložiště back endovým datům zvolíte. Třída obchodní logiku nemusí vědět jak třídu, která je volání přenese data. (To se často nazývá *trvalost neznalosti*.) To usnadňuje testování částí, protože třída obchodní logiku se můžete připojit k úložišti implementace, která používá něco jako jednoduchá jako v paměti `List` kolekce a ukládat data.

> [!NOTE]
> Objekty entity jsou technicky vzato stále není ignorujících, protože při vytváření instance ze tříd, které dědí z rozhraní Entity Framework `EntityObject` třídy. Pro dokončení trvalost neznalosti, můžete použít *prostý starší objekty CLR*, nebo *POCOs*, místo objekty, které dědí `EntityObject` třídy. Použití POCOs je nad rámec tohoto kurzu. Další informace najdete v tématu [testovatelnost a Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) na webu MSDN.)


Teď se můžete připojit `ObjectDataSource` ovládacích prvků pro obchodní logiku místo na třídě k úložišti a ověřte, že vše funguje stejně jako dříve.

V *Departments.aspx* a *DepartmentsAdd.aspx*, změňte všechny výskyty `TypeName="ContosoUniversity.DAL.SchoolRepository"` k `TypeName="ContosoUniversity.BLL.SchoolBL`". (Existují čtyři instance ve všech).

Spustit *Departments.aspx* a *DepartmentsAdd.aspx* stránky k ověření, že jsou i nadále fungovat jako před nástupem prostředí.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Vytvoření projektu testování částí a implementace úložiště

Přidat nový projekt do řešení pomocí **testovací projekt** šablony a pojmenujte ho `ContosoUniversity.Tests`.

V testovacím projektu přidejte odkaz na `System.Data.Entity` a přidejte odkaz na projekt `ContosoUniversity` projektu.

Nyní můžete vytvořit třídu úložiště, které budete používat s testy jednotek. Úložiště dat pro toto úložiště bude v rámci třídy.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

V testovacím projektu vytvořte nový soubor třídy, pojmenujte ho *MockSchoolRepository.cs*a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Tato třída úložiště má stejné metody CRUD jako ten, který má přístup k rozhraní Entity Framework přímo, ale při práci s `List` kolekce v paměti, nikoli s databází. Díky tomu snadněji pro testovací třídu k nastavení a ověření testů jednotek pro třídu obchodní logiku.

## <a name="creating-unit-tests"></a>Vytváření testů jednotek

**Testování** šablona projektu pro vás vytvořili třídu testu jednotek se zakázaným inzerováním a dalším úkolem je upravit tak, že přidáte metody jednotkového testu k němu pro obchodní logiku, kterou chcete přidat do třídy obchodní logiku této třídy.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Na univerzitě Contoso jakékoli jednotlivých kurzů vedených lze pouze správce jednoho oddělení a je třeba přidat obchodní logiku a vynuťte si toto pravidlo. Začněte přidáním testů a spuštění testů, zobrazíte jejich selhání. Potom přidejte kód a znovu spusťte testy, byl očekáván zobrazíte jejich předávání.

Otevřít *UnitTest1.cs* a přidejte `using` příkazy pro obchodní logika a přístup k datům vrstvy, které jste vytvořili v projektu ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Nahradit `TestMethod1` metoda pomocí následujících metod:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL` Metoda vytvoří instanci třídy úložiště, který jste vytvořili pro projekt, který pak předá do nové instance třídy obchodní logiky testování jednotek. Metoda pak používá třídu obchodní logiku pro vložení tří oddělení, které můžete použít v testovacích metod.

Testovacích metod ověřte, že třída obchodní logiku vyvolá výjimku, pokud se někdo pokusí vložit nové oddělení stejné správce jako existující oddělení, nebo když se někdo pokusí aktualizovat oddělení správce tak, že ji nastavíte na ID osoby kdo je již správce jiného oddělení.

Zatím jste ještě nevytvořili třídy výjimek, takže tento kód nebude kompilovat. K jeho kompilace klikněte pravým tlačítkem na `DuplicateAdministratorException` a vyberte **generovat**a potom **třídy**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Tím se vytvoří třídu v projektu testu, které lze odstranit po vytvoření třídy výjimek v hlavním projektem. a implementovat obchodní logiku.

Spuštění projektu testů. Podle očekávání, selhání testů.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Přidání obchodní logiky, aby průchodu testu

V dalším kroku budete implementovat obchodní logiku, která znemožňuje nastavit jako správce oddělení někoho, kdo je již správce jiného oddělení. Budete vyvolat výjimku z vrstvy obchodní logiky a když uživatel upraví oddělení a klikne na tlačítko ji zachytit v prezentační vrstvě **aktualizace** po výběru uživatele, který je již správce. (Můžete také odebrat Instruktoři z rozevíracího seznamu, kteří už jsou administrators, před vykreslení stránky, ale účelem toho všeho je pro práci s vrstvy obchodní logiky.)

Začněte vytvořením třídy výjimek, který budete vyvolat, když se uživatel pokusí provést instruktorem správce více než jednom oddělení. V hlavním projektu vytvořte nový soubor třídy v *BLL* složky, pojmenujte ho *DuplicateAdministratorException.cs*a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Nyní odstranit dočasný *DuplicateAdministratorException.cs* soubor, který jste vytvořili v testovacím projektu. aby bylo možné zkompilovat.

V hlavním projektu, otevřete *SchoolBL.cs* a přidejte následující metodu, která obsahuje logiku ověřování. (Kód odkazuje na metodu, kterou vytvoříte později.)

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Budete volat tuto metodu při vkládání nebo aktualizaci `Department` entity za účelem ověření, zda jiného oddělení již má stejné správce.

Kód volá metodu pro vyhledávání v databázi `Department` entita, která má stejnou `Administrator` hodnota vlastnosti jako entity se přidají nebo aktualizují. Pokud ho najde, kód vyvolá výjimku. Žádné kontroly ověřování je vyžadována, pokud nemá žádné entity, které se přidají nebo aktualizují `Administrator` hodnotu a žádná výjimka je vyvolána, pokud metoda je volána v průběhu aktualizace a `Department` entity nalezených shod `Department` aktualizuje entitu.

Zavolat novou metodu z `Insert` a `Update` metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

V *ISchoolRepository.cs*, přidejte následující deklarace pro novou metodu přístupu k datům:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

V *SchoolRepository.cs*, přidejte následující `using` – příkaz:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

V *SchoolRepository.cs*, přidejte následující novou metodu přístupu k datům:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Tento kód načte `Department` entity, které mají zadanou správce. Pouze jeden oddělení by měl nalezen (pokud existuje). Ale protože je do databáze bez omezení, návratový typ je kolekce v případě, že nejsou nalezeny několika oddělení.

Ve výchozím nastavení, když do kontextu objektu načte entity z databáze, ji uchovává informace o jejich v jeho správce stavu objektu. `MergeOption.NoTracking` Parametr určuje, že nebude pro tento dotaz provést toto sledování. To je nezbytné, protože dotaz může vrátit přesné entity, který se pokoušíte aktualizovat, a pak jste nemohli připojit dané entitě. Pokud upravíte historie oddělení například *Departments.aspx* stránky a nechte beze změny správce, tento dotaz vrátí oddělení historie. Pokud `NoTracking` není nastaven, kontext objektu by už máte entity oddělení historie v jeho správce stavu objektu. Potom po připojení entity oddělení historie, která se znovu vytvoří ze zobrazení stavu kontextu objektu by vyvolat výjimku, která uvádí, že `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Jako alternativu k určení `MergeOption.NoTracking`, můžete vytvořit nový objekt kontextu pouze pro tento dotaz. Vzhledem k tomu, že nový kontext objektu bude mít své vlastní správce stavu objektu, nedojde ke konfliktu při volání `Attach` metody. Nový kontext objektu by sdílet metadata a databáze připojení původní objekt kontextu, takže snížení výkonu v důsledku tomto alternativním postupu bude minimální. Tento přístup je znázorněno zde, ale zavádí `NoTracking` možnost, která bude pro vás být užitečné v jiném kontextu. Tím `NoTracking` možnost je popsána dále v pozdějších kurzech v této sérii.)

V testovacím projektu přidejte novou metodu přístupu k datům na *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Tento kód používá k provedení stejné výběr dat LINQ, který `ContosoUniversity` používá technologii LINQ to Entities pro úložiště projektu.

Znovu spusťte testovací projekt. Tentokrát testy jsou úspěšné.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Zpracování výjimek v prvku ObjectDataSource

V `ContosoUniversity` projektu, spusťte *Departments.aspx* stránce a pokuste se změnit správce pro oddělení na uživatele, který je již správce jiného oddělení. (Mějte na paměti, že lze upravit pouze oddělení, které jste přidali během tohoto kurzu, protože databáze obsahuje předem pomocí neplatná data.) Zobrazí se následující chybová stránka serveru:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Nechcete, aby uživatelům zobrazit tento druh chybovou stránku, takže budete muset přidat kód pro zpracování chyb. Otevřít *Departments.aspx* a určit obslužnou rutinu pro `OnUpdated` událost `DepartmentsObjectDataSource`. `ObjectDataSource` Počáteční značce teď vypadá podobně jako v následujícím příkladu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

V *Departments.aspx.cs*, přidejte následující `using` – příkaz:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Přidejte následující obslužnou rutinu pro `Updated` události:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Pokud `ObjectDataSource` ovládací prvek zachytí výjimku při pokusu provést aktualizaci, předá výjimku v argumentu události (`e`) k této obslužné rutiny. Kód v obslužné rutině zkontroluje, jestli výjimka je výjimka duplicitní správce. Pokud se jedná, kód vytvoří ovládací prvek ověření, který obsahuje chybovou zprávu pro `ValidationSummary` ovládací prvek pro zobrazení.

Spuštění stránky a pokuste se opět někdo správce dvě oddělení. Tentokrát `ValidationSummary` ovládací prvek zobrazí chybovou zprávu.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Provádět podobné změny *DepartmentsAdd.aspx* stránky. V *DepartmentsAdd.aspx*, zadejte obslužnou rutinu pro `OnInserted` událost `DepartmentsObjectDataSource`. Výsledný kód bude vypadat podobně jako v následujícím příkladu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

V *DepartmentsAdd.aspx.cs*, přidejte stejné `using` – příkaz:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Přidejte následující obslužnou rutinu události:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Teď můžete otestovat *DepartmentsAdd.aspx.cs* stránku a ověřte, že správně zpracovává pokusí se jedna osoba správce více než jednom oddělení.

Dokončení tohoto postupu Úvod k implementaci modelu úložiště pro použití `ObjectDataSource` ovládací prvek s Entity Framework. Další informace o použitému vzoru úložišť a možnost testování, najdete v dokumentu MSDN White Paper [testovatelnost a Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).

V následujícím kurzu uvidíte, jak přidat řazení a filtrování funkce, které aplikace.

> [!div class="step-by-step"]
> [Předchozí](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [další](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
