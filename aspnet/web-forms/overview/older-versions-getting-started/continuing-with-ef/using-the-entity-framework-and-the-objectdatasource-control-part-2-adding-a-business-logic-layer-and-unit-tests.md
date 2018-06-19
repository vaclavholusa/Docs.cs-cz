---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Pomocí rozhraní Entity Framework 4.0 a ovládacího prvku ObjectDataSource, část 2: Přidání vrstvu obchodní logiky a testování částí | Microsoft Docs'
author: tdykstra
description: Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený Začínáme s řadou kurz Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: ecdfb2bdc546f55778ec4cc1f61aa66e129134ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888312"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Pomocí rozhraní Entity Framework 4.0 a ovládacího prvku ObjectDataSource, část 2: Přidání vrstvu obchodní logiky a testy jednotek
====================
Podle [tní Dykstra](https://github.com/tdykstra)

> Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený [Začínáme s Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) kurz řady. Pokud nebyla dokončena starší kurzy, jako výchozí bod pro účely tohoto kurzu můžete [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili. Můžete také [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) vytvořené dokončení kurzu řady. Pokud máte dotazy týkající se kurzy, můžete je do příspěvku [ASP.NET Entity Framework fórum](https://forums.asp.net/1227.aspx).


V předchozích kurzu jste vytvořili vícevrstvé webové aplikace používající rozhraní Entity Framework a `ObjectDataSource` ovládacího prvku. Tento kurz ukazuje, jak přidat obchodní logiky při oddělíte vrstvu obchodní logiky (BLL) a přístup k datům layer (DAL) a ukazuje, jak vytvářet testy částí automatizované pro BLL.

V tomto kurzu budete dokončit následující úlohy:

- Vytvořte rozhraní úložiště, který deklaruje metody přístupu k datům, které potřebujete.
- Implementujte rozhraní úložiště v třídě úložiště.
- Vytvořte třídu obchodní logiky, který zavolá třídu úložiště k provádění funkcí přístup k datům.
- Připojení `ObjectDataSource` řízení k třídě obchodní logiky místo třídu úložiště.
- Vytvoření projektu testování částí a třídu úložiště, která používá kolekce v paměti pro jeho data store.
- Vytvoření testů jednotek pro obchodní logiky, která chcete přidat do třídy obchodní logiky, pak spusťte test, abyste viděli, nezdaří.
- Implementovat obchodní logiku v třídě obchodní logiky, a poté znovu spusťte jednotka otestovat, abyste viděli, předat.

Budete pracovat *Departments.aspx* a *DepartmentsAdd.aspx* stránek, které jste vytvořili v předchozí kurzu.

## <a name="creating-a-repository-interface"></a>Vytváření rozhraní úložiště

Budete zahájíte vytváření rozhraní úložiště.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

V *DAL* složku, vytvořte nový soubor třídy a pojmenujte ji *ISchoolRepository.cs*a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

Rozhraní definuje jednu metodu pro každou z CRUD (vytvořit, číst, aktualizovat, odstraňovat) metody, které jste vytvořili v třídě úložiště.

V `SchoolRepository` třídy v *SchoolRepository.cs*, označuje, že tato třída implementuje `ISchoolRepository` rozhraní:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Vytvoření třídy obchodní logiky

V dalším kroku vytvoříte třídu obchodní logiky. To uděláte tak, aby můžete přidat obchodní logiky, která budou spuštěny `ObjectDataSource` řídit, přestože to není, ještě. Prozatím se nová třída obchodní logiky provádět pouze stejné operace CRUD, které nemá úložiště.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Vytvořte novou složku a pojmenujte ji *BLL*. (V reálné aplikaci byste vrstvu obchodní logiky obvykle implementovaný jako knihovny tříd – samostatný projekt – ale na zjednodušení tento kurz se zachová BLL třídy ve složce projektu.)

V *BLL* složku, vytvořte nový soubor třídy a pojmenujte ji *SchoolBL.cs*a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Tento kód vytvoří stejné CRUD metody, které jste viděli dříve v třídě úložiště, ale ne přístup metody rozhraní Entity Framework přímo, volá úložiště, metody třídy.

Třída proměnné, která obsahuje odkaz na třídu úložiště je definována jako typ rozhraní a kód, který vytvoří instanci třídy úložiště se nachází v dva konstruktory. Konstruktor bez parametrů použije `ObjectDataSource` ovládacího prvku. Vytvoří instanci `SchoolRepository` třídu, která jste vytvořili dříve. Další konstruktoru umožňuje ať kód, který vytvoří instanci třídy obchodní logiky, která má předat libovolný objekt, který implementuje rozhraní úložiště.

CRUD metody, které volají třídu úložiště a dva konstruktory umožňují použití třídy obchodní logiky s jakoukoli back-end data store zvolíte. Třída obchodní logiky nemusí zajímat, jak třídu, která je volání potrvají data. (To se často označuje jako *trvalost které*.) To usnadňuje testování, jednotky, protože třída obchodní logiky můžete připojit k implementace úložiště, který používá něco jako jednoduchý jako v paměti `List` kolekce k ukládání dat.

> [!NOTE]
> Technicky jsou objekty entity stále není trvalost – které ignorují, protože jste instanci ze třídy, které dědí od rozhraní Entity Framework `EntityObject` třídy. Pro dokončení trvalost které můžete použít *prostý staré objekty CLR*, nebo *POCOs*, místo objekty, které dědí od `EntityObject` třídy. Použití POCOs je nad rámec tohoto kurzu. Další informace najdete v tématu [testovatelnosti a Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) na webu MSDN.)


Teď se můžete připojit `ObjectDataSource` ovládacích prvků mají obchodní logiky třídy místo na úložišti a ověřte, že všechno funguje stejně jako předtím.

V *Departments.aspx* a *DepartmentsAdd.aspx*, změňte všechny výskyty `TypeName="ContosoUniversity.DAL.SchoolRepository"` k `TypeName="ContosoUniversity.BLL.SchoolBL`". (Ve všech existují čtyři instancí.)

Spustit *Departments.aspx* a *DepartmentsAdd.aspx* stránky k ověření, že budou i nadále fungovat stejně jako před.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Vytvoření projektu testování částí a implementace úložiště

Přidat nový projekt na řešení pomocí **testovacího projektu** šablony a pojmenujte ji `ContosoUniversity.Tests`.

V testu projektu přidejte odkaz na `System.Data.Entity` a přidejte odkaz na projekt `ContosoUniversity` projektu.

Nyní můžete vytvořit třídu úložiště, které budete používat s testy jednotek. Úložiště dat pro toto úložiště bude v rámci třídy.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

V testu projektu vytvořte nový soubor třídy, pojmenujte ji *MockSchoolRepository.cs*a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Tato třída úložiště má stejné metody CRUD jako ten, který má přístup k rozhraní Entity Framework přímo, ale pracují s `List` kolekce v paměti, nikoli s databází. To usnadňuje třídu testovací nastavit a ověřit testy částí pro třídu obchodní logiky.

## <a name="creating-unit-tests"></a>Testování částí

**Testování** šablona projektu pro můžete vytvořit třídu test jednotky se zakázaným inzerováním a svůj další úkol je tato třída upravit přidáním metody testů jednotek do ji pro obchodní logiky, která chcete přidat do třídy obchodní logiky.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Na vysoké školy Contoso všechny jednotlivé lektorem lze pouze správce jednoho oddělení a je nutné přidat obchodní logiku a vynuťte toto pravidlo. Začněte přidáním testy a spouštění testů neuvidíte nezdaří. Budete pak přidejte kód a znovu spusťte testy, byla očekávána zobrazíte jejich předávání.

Otevřete *UnitTest1.cs* souboru a přidejte `using` pro obchodní logiku a data access vrstvy, které jste vytvořili v projektu ContosoUniversity příkazy:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Nahraďte `TestMethod1` metoda pomocí následujících metod:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL` Metoda vytvoří instanci třídy úložiště, kterou jste vytvořili pro projekt, který se potom předá do nové instance třídy obchodní logiky testování jednotka. Metoda pak používá třídu obchodní logiky vložit tři oddělení, které můžete použít v testu metody.

Zkušební metody ověřte, že třídu obchodní logiky vyvolá výjimku, pokud se někdo pokusí vložit nový oddělení pomocí stejného správce jako existující oddělení, nebo pokud se někdo pokusí aktualizovat oddělení správce nastavit na ID osoby kdo je již správce jiného oddělení.

Ještě jste nevytvořili třídy výjimek, tak nebude zkompilovat tento kód. Aby mohl zkompilovat, klikněte pravým tlačítkem na `DuplicateAdministratorException` a vyberte **generování**a potom **třída**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Tím se vytvoří třídu v k testovacímu projektu, který můžete odstranit po vytvoření třídy výjimek v hlavní projekt. a implementovat obchodní logiku.

Spusťte projekt test. Podle očekávání, testy nezdaří.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Přidání obchodní logiku, aby průchodu testu

Dále budete implementovat obchodní logiku, aby nebylo možné nastavit jako správce oddělení někoho, kdo je již správce jiného oddělení. Budete výjimku z vrstvy obchodní logiky a když uživatel upravuje oddělení a klikne na tlačítko ji catch v prezentační vrstvě **aktualizace** po výběru uživatele, který je již správce. (Můžete také odebrat vyučující z rozevíracího seznamu, kteří jsou již správci před vykreslení stránky, ale tady se používá k práci s vrstvu obchodní logiky.)

Začněte vytvořením třídy výjimky, která budete výjimku při pokusu uživatele, aby lektorem správce více než jednom oddělení. V hlavním projektu vytvořte nový soubor – třída v *BLL* složky, pojmenujte ji *DuplicateAdministratorException.cs*a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Nyní odstranit dočasnou *DuplicateAdministratorException.cs* soubor, který jste vytvořili v k testovacímu projektu dříve aby mohl zkompilovat.

V hlavní projekt, otevřete *SchoolBL.cs* souboru a přidejte následující metodu, která obsahuje logiku ověření. (Kód odkazuje na metodu, která vytvoříte později).

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Budete volat tuto metodu, při vložení nebo aktualizace `Department` entity, aby bylo možné zkontrolujte, zda jiný oddělení již má stejné správce.

Kód volá metody na hledání databáze `Department` entity, která má stejný `Administrator` hodnotu vlastnosti jako entity se přidají nebo aktualizují. Pokud je takový nalezen, kód vyvolá výjimku. Kontrola žádné ověření je nutné, pokud entity se přidají nebo aktualizují neobsahuje žádné `Administrator` hodnota a žádná výjimka je vyvolána, pokud metoda je volána v průběhu aktualizace a `Department` entity nalezených shod `Department` aktualizaci entity.

Volejte metodu nové z `Insert` a `Update` metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

V *ISchoolRepository.cs*, přidejte následující deklarace pro nová metoda přístupu k datům:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

V *SchoolRepository.cs*, přidejte následující `using` příkaz:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

V *SchoolRepository.cs*, přidejte následující metodu nové přístup k datům:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Tento kód načte `Department` entity, které mají zadaný správce. Pouze jeden oddělení má nalezen (pokud existuje). Však žádné omezení je integrovaná do databáze, je návratový typ kolekce, v případě, že se nacházejí více oddělení.

Ve výchozím nastavení pokud kontext objektu entity načte z databáze, se uchovává informace o jejich v jeho správce stavu objektu. `MergeOption.NoTracking` Parametr určuje, že toto sledování nebudou provést pro tento dotaz. To je nezbytné, protože dotaz může vrátit přesné entity, který se pokoušíte aktualizovat, a potom se budou moci připojit dané entity. Například pokud chcete upravit do historie oddělení *Departments.aspx* stránky a nechte beze změny správce, tento dotaz vrátí oddělení historie. Pokud `NoTracking` není nastaven, kontext objektu by už máte entity oddělení historie v jeho správce stavu objektu. Pak když připojíte entity oddělení historie, který se znovu vytvoří ze zobrazení stavu, kontext objektu by způsobí výjimku, která uvádí, že `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Jako alternativu k určení `MergeOption.NoTracking`, můžete vytvořit nový objekt kontextu pouze pro tento dotaz. Vzhledem k tomu, že nový kontext objektu by měla mít vlastní správce stavu objektu, by žádné konflikt při volání `Attach` metoda. Nový objekt kontextu by sdílet metadata a připojení k databázi s původní objekt kontextu, takže snížení výkonu tohoto alternativní přístupu by být minimální. Přístup zobrazeny zde, ale zavádí `NoTracking` možnost, která budete užitečné v jiných kontextech. `NoTracking` Popsané možnost Další novější kurzu této série.)

V k testovacímu projektu, přidejte nová metoda přístupu k datům na *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Tento kód používá LINQ provést stejný výběr dat, `ContosoUniversity` projektu úložiště používá technologii LINQ to Entities pro.

K testovacímu projektu spusťte znovu. Tentokrát jsou testy úspěšné.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Zpracování výjimek ObjectDataSource

V `ContosoUniversity` projektu, spusťte *Departments.aspx* stránky a pokuste se změnit správce pro oddělení na uživatele, který je již správce jiného oddělení. (Nezapomeňte, že můžete upravit pouze oddělení, které jste přidali během tohoto kurzu, protože databáze je předem zaveden s neplatnými daty.) Zobrazí se následující chybová stránka serveru:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Nechcete, aby uživatelům zobrazit tento druh chybovou stránku, takže je nutné přidat kód pro ošetření chyb. Otevřete *Departments.aspx* a zadejte obslužnou rutinu pro `OnUpdated` události `DepartmentsObjectDataSource`. `ObjectDataSource` Počáteční značce nyní podobá následujícímu příkladu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

V *Departments.aspx.cs*, přidejte následující `using` příkaz:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Přidejte následující obslužnou rutinu pro `Updated` událostí:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Pokud `ObjectDataSource` řízení zachytí výjimku při pokusu o provedení aktualizace, pak předá výjimku v argumentu události (`e`) k této obslužné rutiny. Kód v obslužné rutině zkontroluje, pokud výjimka je výjimka duplicitní správce. Pokud se jedná, kód vytvoří validátoru ovládací prvek, který obsahuje chybovou zprávu pro `ValidationSummary` ovládacího prvku pro zobrazení.

Spuštění stránky a pokusí se někdo znovu správce dvě oddělení. Tato doba `ValidationSummary` ovládací prvek zobrazí chybovou zprávu.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Ujistěte se, podobně jako změny *DepartmentsAdd.aspx* stránky. V *DepartmentsAdd.aspx*, zadejte obslužnou rutinu pro `OnInserted` události `DepartmentsObjectDataSource`. Výsledný kód bude podobat následujícímu příkladu.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

V *DepartmentsAdd.aspx.cs*, přidání stejné `using` příkaz:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Přidejte následující obslužné rutiny události:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Nyní můžete otestovat *DepartmentsAdd.aspx.cs* a ověřte, že také správně zpracovává pokusy o nastavení jedna osoba správce více než jeden oddělení.

Tím dokončíte Úvod k implementaci použitému vzoru pro použití `ObjectDataSource` ovládacího prvku pomocí rozhraní Entity Framework. Další informace o použitému vzoru úložišť a testovatelnosti, najdete v dokumentu White Paper MSDN [testovatelnosti a Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).

V následujícím kurzu uvidíte, jak přidat řazení a filtrování funkce do aplikace.

> [!div class="step-by-step"]
> [Předchozí](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [další](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
