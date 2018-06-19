---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Pomocí rozhraní Entity Framework 4.0 a ovládacího prvku ObjectDataSource, část 1: Začínáme | Microsoft Docs'
author: tdykstra
description: Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený Začínáme s řadou kurz Entity Framework. Pokud je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 6584767418c898913777b3b1549a816679c8430d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892079"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Pomocí rozhraní Entity Framework 4.0 a ovládacího prvku ObjectDataSource, část 1: Začínáme
====================
Podle [tní Dykstra](https://github.com/tdykstra)

> Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený [Začínáme s Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) kurz řady. Pokud nebyla dokončena starší kurzy, jako výchozí bod pro účely tohoto kurzu můžete [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili. Můžete také [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) vytvořené dokončení kurzu řady.
> 
> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET pomocí sady Visual Studio 2010 a Entity Framework 4.0. Ukázkové aplikace je web pro fiktivní vysoké školy Contoso. Obsahuje funkce, jako je jejich příchodu student, postupu vytvoření a přiřazení lektorem.
> 
> Tento kurz ukazuje příklady v jazyce C#. [Ke stažení ukázkové](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) obsahuje kód v jak C# a Visual Basic.
> 
> ## <a name="database-first"></a>První databáze
> 
> Existují tři způsoby, můžete pracovat s daty v Entity Framework: *Database First*, *Model First*, a *Code First*. Tento kurz je určen pro první databáze. Informace o rozdílech mezi tyto pracovní postupy a pokyny o tom, jak zvolit tu nejvhodnější pro váš scénář najdete v tématu [Entity Framework vývoj pracovních](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>webové formuláře
> 
> Tento kurz řady jako řady Začínáme, používá model webových formulářů ASP.NET a předpokládá, že víte, jak pracovat s webovými formuláři ASP.NET v sadě Visual Studio. Pokud ne, najdete v části [Začínáme s webovými formuláři ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Pokud dáváte přednost práci s rozhraní ASP.NET MVC, přečtěte si téma [Začínáme s rozhraní Entity Framework používat rozhraní ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Verze softwaru
> 
> | **Zobrazí v tomto kurzu** | **Taky spolupracuje se službou** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express pro Web. Tento kurz nebyl testován s novější verzí sady Visual Studio. Existuje mnoho rozdílů v nabídce Možnosti, dialogová okna a šablony. |
> | .NET 4 | Rozhraní .NET 4.5 je zpětně kompatibilní s .NET 4, ale tento kurz nebyl testován s .NET 4.5. |
> | Rozhraní Entity Framework 4 | Tento kurz nebyl testován pomocí novější verze Entity Framework. Od verze Entity Framework 5, EF používá ve výchozím nastavení `DbContext API` která byla zavedená s EF 4.1. Ovládací prvek EntityDataSource byla navržená tak, aby použít `ObjectContext` rozhraní API. Informace o tom, jak používat EntityDataSource řízení s `DbContext` rozhraní API, najdete v části [tomto příspěvku na blogu](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Otázky
> 
> Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [ASP.NET Entity Framework fórum](https://forums.asp.net/1227.aspx), [Entity Framework a technologie LINQ to Entities fórum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), nebo [ StackOverflow.com](http://stackoverflow.com/).


`EntityDataSource` Řízení umožňuje velmi rychle vytvořit aplikaci, ale obvykle vyžaduje, abyste zachovat významné množství obchodní logika a logika přístup k datům v vaše *.aspx* stránky. Pokud očekáváte, vaše aplikace Čím složitost a vyžadují následné údržbě, vývoj déle investovat předem před vytvořením *n vrstvá* nebo *vrstvený* strukturu aplikace To je více udržovatelný. K implementaci této architektury, oddělit prezentační vrstvu z vrstvu obchodní logiky (BLL) a vrstva přístupu k datům (DAL). Jeden způsob, jak implementovat tato struktura je použití `ObjectDataSource` řízení místo `EntityDataSource` ovládacího prvku. Při použití `ObjectDataSource` ovládací prvek, implementovat vlastní kód pro přístup k datům a pak vyvolání v *.aspx* stránky používající ovládací prvek, který má mnoho stejných funkcí jako další ovládací prvky datových zdrojů. Díky tomu můžete kombinovat výhod přístup n vrstvá s výhody použití ovládacího prvku webového formuláře pro přístup k datům.

`ObjectDataSource` Řízení vám dává větší flexibilitu také jiné způsoby. Protože můžete napsat vlastní kód přístup k datům, je snazší více než jen pro čtení, vložit, aktualizovat nebo odstranit určitý typ entity, které jsou úlohy, `EntityDataSource` ovládací prvek je určen k provedení. Můžete například provést protokolování pokaždé, když se aktualizuje entitu, archivaci dat pokaždé, když se odstraní entitu, nebo automaticky kontrola a aktualizace související data, podle potřeby při vkládání řádek s hodnotou cizího klíče.

## <a name="business-logic-and-repository-classes"></a>Obchodní logika a třídy úložiště

`ObjectDataSource` Řízení funguje vyvoláním třídu, která vytvoříte. Třída obsahuje metody, které načíst a aktualizovat data, a zadejte názvy těchto metod, která `ObjectDataSource` ovládací prvek v kódu. Při vykreslování nebo zpětné odesílání `ObjectDataSource` volá metody, které jste určili.

Kromě základních operací CRUD, třídu, která vytvoříte pro použití s `ObjectDataSource` řízení může být nutné provést obchodní logiky při `ObjectDataSource` načte nebo aktualizuje data. Při aktualizaci oddělení může například muset ověřte, jestli žádné jiných oddělení mají stejného správce, protože jedna osoba nemůže být správcem více než jeden oddělení.

V některých `ObjectDataSource` dokumentace, například [přehledu třídy ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), ovládacího prvku zavolá třídu označuje jako *obchodní objekt* obchodní logika a logika přístup k datům, který obsahuje . V tomto kurzu vytvoříte samostatné třídy pro obchodní logika a logika přístup k datům. Je volána třída, který zapouzdřuje logiku přístup k datům *úložiště*. Obchodní logiky třída obsahuje metody obchodní logiky a metody přístupu k datům, ale metody přístupu k datům volání úložiště k provádění úloh přístup k datům.

Můžete také vytvořit abstraktní vrstvu mezi BLL a DAL, která usnadňuje automatizované jednotky testování BLL. Tato abstraktní vrstvu je implementováno modulem vytváření rozhraní a pomocí rozhraní při doložit úložiště ve třídě obchodní logiky. Díky tomu je možné zadat třídu obchodní logiky s odkazem na libovolný objekt, který implementuje rozhraní úložiště. Pro běžné operace by poskytnete objekt úložiště, který funguje s platformou Entity Framework. Pro testování, poskytnete objekt úložiště, který funguje s daty uloženými v tak, že můžete snadno upravit, jako je například třída proměnných definovaná jako kolekce.

Následující obrázek ukazuje rozdíl mezi třídu obchodní logiky, která obsahuje logiku pro přístup k datům bez úložiště a ten, který používá úložiště.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Nejdříve začneme vytvořením webové stránky, ve kterém `ObjectDataSource` ovládací prvek vázán přímo do úložiště, protože vykonává jenom základní přístup k datům úlohy. V dalším kurzu vytvoříte třídu obchodní logiky s logikou ověřování a vytvořit vazbu `ObjectDataSource` řízení třídy místo třídu úložiště. Pokud vytvoříte testy částí pro logiku ověření. V třetím kurzu této série přidá řazení a filtrování funkce do aplikace.

Na stránkách v tomto kurzu vytvoříte pracovat `Departments` sady entit datového modelu, který jste vytvořili v [Začínáme kurz řady](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Aktualizace databáze a datový Model

V tomto kurzu zahájíte tím, že dvě změny do databáze, které vyžadují odpovídající změny do datového modelu, který jste vytvořili v [Začínáme s Entity Framework a webových formulářů](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) kurzy. V jednom z těchto kurzech provedli jste změny v Návrháři ručně k synchronizaci datového modelu s databází po změně databáze. V tomto kurzu budete používat návrháři **aktualizovat Model z databáze** nástroj automaticky aktualizovat data modelu.

### <a name="adding-a-relationship-to-the-database"></a>Přidání vztahu k databázi

V sadě Visual Studio otevřete webovou aplikaci Contoso univerzity, kterou jste vytvořili v [Začínáme s Entity Framework a webových formulářů](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) kurz řady a potom otevřete `SchoolDiagram` diagram databáze.

Pokud si prohlédnete `Department` tabulky v diagramu databáze, uvidíte, že má `Administrator` sloupce. Tento sloupec je cizí klíč k `Person` tabulky, ale žádné relace cizího klíče je definována v databázi. Budete muset vytvořit relaci a aktualizovat data modelu tak, aby rozhraní Entity Framework můžete automaticky zpracovávat tento vztah.

V diagramu databáze, klikněte pravým tlačítkem myši `Department` tabulky a vyberte **vztahy**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

V **cizí klíč relace** klikněte na **přidat**, pak klikněte na tlačítko se třemi tečkami pro **tabulek a sloupců specifikace**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

V **tabulky a sloupce** dialogové okno pole, nastavte primární klíč tabulky a pole na `Person` a `PersonID`a nastavte tabulky cizího klíče a do pole `Department` a `Administrator`. (Když to uděláte, název relace se změní z `FK_Department_Department` k `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Klikněte na tlačítko **OK** v **tabulky a sloupce** pole, klikněte na tlačítko **Zavřít** v **cizí klíč relace** pole a uložte změny. Zobrazení dotazu, pokud chcete uložit `Person` a `Department` tabulky, klikněte na tlačítko **Ano**.

> [!NOTE]
> Pokud jste odstranili `Person` řádky, které odpovídají data, která je již v `Administrator` sloupce, nebude možné uložit změny. V takovém případě pomocí editoru tabulky v **Průzkumníka serveru** k Ujistěte se, že `Administrator` hodnotu v každé `Department` řádek obsahuje ID záznam, který skutečně existuje v `Person` tabulky.
> 
> Po uložení změn, nebudete moct odstranit řádek z `Person` tabulky, pokud to je správce a oddělení. V případě produkční aplikace by poskytují konkrétní chybová zpráva při odstranění brání omezení databáze, nebo zadáte kaskádové odstranění. Příklad určení kaskádové odstranění, naleznete v části [Entity Framework a ASP.NET – získávání spuštění část 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Přidání zobrazení do databáze

V novém *Departments.aspx* stránky, která bude vytvářet, chcete poskytnout rozevírací seznam vyučující, s názvy ve formátu "nejprve poslední" tak, aby uživatelé mohou vybrat správci oddělení. Aby bylo snazší k tomu, vytvoříte zobrazení v databázi. Zobrazení bude obsahovat pouze data, vyžaduje rozevíracího seznamu: úplný název (ve správném formátu) a záznam klíče.

V **Průzkumníka serveru**, rozbalte položku *School.mdf*, klikněte pravým tlačítkem myši **zobrazení** složky a vyberte **přidat nové zobrazení**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Klikněte na tlačítko **Zavřít** při **přidat tabulku** se zobrazí dialogové okno a vložte následující příkaz SQL do podokna SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Uložte zobrazení jako `vInstructorName`.

### <a name="updating-the-data-model"></a>Aktualizace modelu dat

V *DAL* složku, otevřete *SchoolModel.edmx* souboru, klikněte pravým tlačítkem na návrhovou plochu a vyberte **aktualizovat Model z databáze**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

V **zvolte si databázové objekty** dialogové okno, vyberte **přidat** a vyberte zobrazení, kterou jste právě vytvořili.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Klikněte na tlačítko **Dokončit**.

V návrháři, uvidíte vytvořený nástroje `vInstructorName` entitu a nového přidružení mezi `Department` a `Person` entity.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> V **výstup** a **seznam chyb** windows, může se zobrazit upozornění, že nástroj automaticky vytvořit primární klíč pro nový `vInstructorName` zobrazení. Toto je očekávané chování.


Když odkazujete na nové `vInstructorName` entity v kódu, nechcete použít databázi prefixu malá "v" k němu. Proto bude Přejmenujte entitu a sadu entit v modelu.

Otevřete **modelu prohlížeče**. Zobrazí `vInstructorName` uveden jako typ entity a zobrazení.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

V části **nazvaného SchoolModel** (ne **SchoolModel.Store**), klikněte pravým tlačítkem na **vInstructorName** a vyberte **vlastnosti**. V **vlastnosti** změňte **název** vlastnost "InstructorName" a změňte **nastavit název Entity** vlastnost "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Uložte a zavřete datový model a projekt znovu sestavte.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Pomocí třídy úložiště a ovládacího prvku ObjectDataSource

Vytvořte nový soubor – třída v *DAL* složky, pojmenujte ji *SchoolRepository.cs*a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Tento kód obsahuje jeden `GetDepartments` metoda, která vrátí všechny entity v `Departments` sady entit. Protože víte, že vám budou získávat přístup k `Person` navigační vlastnost pro každý řádek vrácených, můžete zadat přes načítání pro danou vlastnost pomocí `Include` metoda. Třída také implementuje `IDisposable` rozhraní k zajištění toho, že připojení k databázi je vydán při uvolnění objektu.

> [!NOTE]
> Běžnou praxí je pro vytvoření třídy úložiště ke každému typu entity. V tomto kurzu se používá jednu třídu úložiště pro víc typů entit. Další informace o vzoru úložiště, najdete v příspěvcích ve [blog týmu rozhraní Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) a [Julie Lerman blog](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


`GetDepartments` Metoda vrátí `IEnumerable` objekt místo `IQueryable` objektu, aby se zajistilo, že je vrácená kolekce použitelné i po uvolnění samotného objektu úložiště. `IQueryable` Objekt může způsobit přístup k databázi, vždy, když je přístupný, avšak objekt úložiště může zlikvidován podle času pokusí vykreslení dat u ovládacího prvku. Může vrátit jiný typ kolekce, například `IList` objektu místo `IEnumerable` objektu. Však návrat `IEnumerable` objekt zajistí, že můžete provádět úlohy zpracování typické seznamu jen pro čtení, jako `foreach` smyčky a dotazů LINQ, ale nelze přidat nebo odebrat položky v kolekci, což může znamenat, že by se tyto změny trvale uložit do databáze.

Vytvoření *Departments.aspx* stránky, která používá *Site.Master* hlavní stránky a přidejte následující kód v `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Tento kód vytvoří `ObjectDataSource` ovládací prvek, který používá třídu úložiště, který jste právě vytvořili, a `GridView` ovládací prvek zobrazí data. `GridView` Řízení Určuje **upravit** a **odstranit** příkazy, ale nepřidali kódu, který je ještě podporuje.

Pomocí několika sloupců `DynamicField` ovládací prvky tak, aby můžete využít výhod funkce automatického datového formátování a ověřování. Pro tyto pro práci, budete muset volat `EnableDynamicData` metoda v `Page_Init` obslužné rutiny události. (`DynamicControl` ovládací prvky nejsou použity v `Administrator` pole proto není kompatibilní s navigační vlastnosti.)

`Vertical-Align="Top"` Atributy bude důležité později při přidání sloupce, který obsahuje vnořený `GridView` řízení k mřížce.

Otevřete *Departments.aspx.cs* souboru a přidejte následující `using` příkaz:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Pak přidejte následující obslužnou rutinu pro danou stránku `Init` událostí:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

V *DAL* složky, vytvořte nový soubor třídy s názvem *Department.cs* a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Tento kód přidá metadata do datového modelu. Určuje, že `Budget` vlastnost `Department` entity ve skutečnosti představuje měnu, i když je jeho datový typ `Decimal`, a určuje, že hodnota musí být mezi 0 a 1,000,000.00 $. Také určuje, že `StartDate` vlastnost musí být formátována jako datum ve formátu mm/dd/rrrr.

Spustit *Departments.aspx* stránky.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Všimněte si, že i když jste nezadali řetězec formátu v *Departments.aspx* stránky kód pro **nároky** nebo **počáteční datum** sloupců, výchozí měny a data formátování použil k nim pomocí `DynamicField` ovládací prvky, pomocí metadata, která jste zadali v *Department.cs* souboru.

## <a name="adding-insert-and-delete-functionality"></a>Přidání Insert a Delete funkce

Otevřete *SchoolRepository.cs*, přidejte následující kód, abyste mohli vytvořit `Insert` metoda a `Delete` metoda. Tento kód také obsahuje metodu s názvem `GenerateDepartmentID` , vypočítá další hodnotu klíče záznamu k dispozici pro použití `Insert` metoda. Toto je nutné kvůli databáze není nakonfigurován pro toto automaticky vypočítat `Department` tabulky.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Attach – metoda

`DeleteDepartment` Volání metod `Attach` metoda opětné odkaz, který se udržuje v správce stavu objektů do kontextu objektu mezi entity v paměti a databází řádek představuje. Toto musí předcházet volání metod `SaveChanges` metoda.

Termín *kontext objekt* odkazuje na třídu Entity Framework, která je odvozena z `ObjectContext` třídu, která používáte pro přístup k sad entit a entity. V kódu pro tento projekt je třída s názvem `SchoolEntities`, a její instanci je vždy s názvem `context`. Objekt kontextu *správce stavu objektu* je třída, která je odvozena z `ObjectStateManager` třídy. Kontakt objekt používá správce stavu objekt pro uložení objektů entity a k udržování přehledu o tom, jestli je každé z nich synchronizované se svým odpovídající řádek tabulky nebo řádků v databázi.

Při čtení entitu do kontextu objektu ukládá v objektu správce stavu a uchovává informace o tom, jestli je synchronizována s databází této reprezentace objektu. Například pokud změníte hodnotu vlastnosti, je nastaven příznak indikující, že vlastnost, kterou jste změnili již není synchronizována s databází. Potom při volání `SaveChanges` metoda, kontext objektu ví, co dělat v databázi, protože správce stavu objektu zná přesně co je aktuální stav entity a stav databáze liší.

Však tento proces obvykle nefunguje ve webové aplikaci, protože instance kontextu objektu, který čte entity, spolu se nacházejí v jeho stav object manager. je zrušen po vykreslení stránky. Instance kontextu objektu, který se musí použít změny je novou, která je vytvořena instance pro zpracování zpětného volání. U `DeleteDepartment` metody `ObjectDataSource` řízení znovu vytvoří původní verze entity pro vás z hodnot v zobrazení stavu, ale to znovu vytvořit `Department` entita neexistuje v objektu správce stavu. Pokud jste volali metodu `DeleteObject` metodu na tuto entitu znovu vytvořena, volání by nezdaří, protože kontext objektu nebude vědět, zda tato entita je synchronizována s databází. Avšak volání `Attach` metoda znovu naváže stejné sledování mezi znovu vytvořenou entitu a hodnotami v databázi, která byla původně provést automaticky, pokud byl načten entity v instanci objektu kontextu starší.

Existují čas, kdy nechcete, aby kontext objektu sledovat entity v správce stavu objektu a můžete nastavit příznaky tak, aby zabránil jeho učinit. V dalších kurzech této série jsou uvedeny příklady tohoto objektu.

### <a name="the-savechanges-method"></a>Metoda SaveChanges

Tato třída jednoduché úložiště ukazuje základní principy k provádění operací CRUD. V tomto příkladu `SaveChanges` metoda je volána hned po jednotlivých aktualizací. V případě produkční aplikace můžete chtít volání `SaveChanges` metoda ze samostatné metodě, čímž získáte větší kontrolu nad při aktualizaci databáze. (Na konci v dalším kurzu najdete odkaz na dokument white paper, který popisuje jednotka práci vzor, který je jeden ze způsobů koordinace související aktualizace.) Všimněte si také, v příkladu `DeleteDepartment` metoda nezahrnuje kód pro zpracování konfliktů souběžnosti; přidá se kód k tomu novější kurzu této série.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Načítání názvů lektorem vyberte při vkládání

Uživatelé musí být schopni seznamu vyučující v rozevíracím seznamu vyberte správce při vytváření nové oddělení. Proto přidejte následující kód, který *SchoolRepository.cs* vytvořit metodu pro načtení seznamu vyučující pomocí zobrazení, které jste vytvořili dříve:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Vytvoření stránky pro vkládání oddělení

Vytvoření *DepartmentsAdd.aspx* stránky, která používá *Site.Master* stránky a přidejte následující kód v `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Tento kód vytvoří dvě `ObjectDataSource` prvky, jeden pro vkládání nových `Department` entity a jeden pro načítání lektorem názvy `DropDownList` ovládací prvek, který se používá pro výběr správci oddělení. Vytvoří značku `DetailsView` řízení pro zadání nové oddělení a určuje obslužné rutiny pro ovládací prvek `ItemInserting` událostí tak, aby můžete nastavit `Administrator` hodnoty cizího klíče. Na konci je `ValidationSummary` ovládací prvek zobrazí chybové zprávy.

Otevřete *DepartmentsAdd.aspx.cs* a přidejte následující `using` příkaz:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Přidejte následující proměnné třídy a metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init` Metoda umožňuje funkce Dynamická Data. Obslužná rutina pro `DropDownList` ovládacího prvku `Init` události uloží odkaz na ovládací prvek a obslužnou rutinu pro `DetailsView` ovládacího prvku `Inserting` událostí používá tento odkaz k získání `PersonID` hodnotu vybrané lektorem a aktualizace `Administrator` vlastností cizího klíče z `Department` entity.

Spuštění stránky, přidejte informace o nové oddělení a klikněte **vložit** odkaz.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Zadejte hodnoty pro jiné nové oddělení. Zadejte číslo větší než 1,000,000.00 v **nároky** pole a karty na další pole. V poli se zobrazí hvězdičku a pokud nad ním podržíte ukazatel myši, zobrazí se chybová zpráva, která jste zadali v metadatech pro toto pole.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Klikněte na tlačítko **vložit**, a zobrazí se chybová zpráva zobrazí `ValidationSummary` ovládací prvek v dolní části stránky.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Potom zavřete prohlížeč a otevřete *Departments.aspx* stránky. Přidat schopnost odstraňování *Departments.aspx* tak, že přidáte `DeleteMethod` atribut `ObjectDataSource` řízení a `DataKeyNames` atribut `GridView` ovládacího prvku. Otevírání značky pro tyto ovládací prvky bude nyní vypadat podobně jako v následujícím příkladu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Spuštění stránky.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Odstranit oddělení, které jste přidali, když jste spustili *DepartmentsAdd.aspx* stránky.

## <a name="adding-update-functionality"></a>Přidání funkce aktualizace

Otevřete *SchoolRepository.cs* a přidejte následující `Update` metoda:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Když kliknete na tlačítko **aktualizace** v *Departments.aspx* stránky, `ObjectDataSource` řízení vytvoří dvě `Department` entity, které mají být předány `UpdateDepartment` metoda. Jeden obsahuje původní hodnoty, které byly uloženy v zobrazení stavu a dalších obsahuje nové hodnoty, které byly zadány v `GridView` ovládacího prvku. Kód `UpdateDepartment` metoda předává `Department` entita, která má původní hodnoty, které mají `Attach` za účelem vytvoření sledování mezi entitou a co je v databázi. Poté předá kód `Department` entita, která má nové hodnoty, které mají `ApplyCurrentValues` metoda. Kontext objekt porovnává staré a nové hodnoty. Pokud nová hodnota se liší od původní hodnota, kontext objektu se změní hodnota vlastnosti. `SaveChanges` Metoda pak aktualizuje jenom změněné sloupce v databázi. (Ale pokud funkce aktualizace pro tuto entitu byly mapovány na uložené procedury, celý řádek by aktualizovat bez ohledu na to, které byly změněny sloupce.)

Otevřete *Departments.aspx* souboru a přidejte následující atributy, které se `DepartmentsObjectDataSource` ovládacího prvku:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Tento příčiny původní hodnoty byly uloženy v zobrazení stavu tak, že může být porovnána s novými hodnotami v `Update` metoda.
- `OldValuesParameterFormatString="orig{0}"`   
 Tento příkaz informuje ovládací prvek, který je název původní hodnoty parametru `origDepartment` .

Značky pro počáteční značky `ObjectDataSource` řízení nyní podobá následujícímu příkladu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Přidat `OnRowUpdating="DepartmentsGridView_RowUpdating"` atribut `GridView` ovládacího prvku. Použijete toto nastavení `Administrator` hodnotu vlastnosti na základě řádku uživatel vybere v rozevíracím seznamu. `GridView` Počáteční značce nyní podobá následujícímu příkladu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Přidat `EditItemTemplate` řízení pro `Administrator` sloupec, který se `GridView` řídit, okamžitě po `ItemTemplate` řízení pro tento sloupec:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

To `EditItemTemplate` ovládací prvek je podobný `InsertItemTemplate` řídit ve *DepartmentsAdd.aspx* stránky. Rozdílem je, že počáteční hodnota ovládacího prvku se nastavuje pomocí `SelectedValue` atribut.

Před `GridView` řídit, přidejte `ValidationSummary` řízení stejně jako v *DepartmentsAdd.aspx* stránky.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Otevřete *Departments.aspx.cs* a okamžitě po deklaraci částečné třídy, přidejte následující kód k vytvoření soukromé pole tak, aby odkazovaly `DropDownList` ovládacího prvku:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Pak přidejte obslužné rutiny pro `DropDownList` ovládacího prvku `Init` událostí a `GridView` ovládacího prvku `RowUpdating` událostí:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Obslužná rutina pro `Init` události uloží odkaz na `DropDownList` ovládací prvek v poli třídy. Obslužná rutina pro `RowUpdating` událostí používá odkaz k získání hodnoty zadané uživatelem a použijte ho k `Administrator` vlastnost `Department` entity.

Použití *DepartmentsAdd.aspx* stránky přidat nové oddělení, a poté spusťte *Departments.aspx* a klikněte na tlačítko **upravit** na řádek, který jste přidali.

> [!NOTE]
> Nebudete moci upravit řádky, které jste nepřidali (to znamená, které již byly v databázi), protože neplatná data v databázi. Správci řádků, které byly vytvořeny s databází jsou studenty. Pokud se pokusíte upravit jeden z nich, obdržíte chybovou stránku, který nahlásí chybu jako `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Pokud zadáte neplatný **nároky** částka a pak klikněte na **aktualizace**, zobrazí stejné hvězdička a chybová zpráva, kterou jste viděli v *Departments.aspx* stránky.

Změnit hodnotu pole nebo vyberte jiný správce a klikněte na tlačítko **aktualizace**. Zobrazí se tato změna.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Tím dokončíte Úvod k použití `ObjectDataSource` řízení pro základní CRUD (vytvořit, číst, aktualizovat, odstraňovat) operací s rozhraní Entity Framework. Když jste sestavili jednoduchou aplikaci n vrstvá, ale vrstvu obchodní logiky je stále úzce párované do vrstvy přístup k datům, což ztěžuje automatizované testování částí. V následujícím kurzu uvidíte, jak implementovat použitému vzoru usnadňuje testování částí.

> [!div class="step-by-step"]
> [Next](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
