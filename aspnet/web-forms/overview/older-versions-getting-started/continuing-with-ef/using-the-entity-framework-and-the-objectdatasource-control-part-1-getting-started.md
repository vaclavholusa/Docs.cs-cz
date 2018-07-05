---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Použití rozhraní Entity Framework 4.0 a ovládací prvek ObjectDataSource, 1. část: Začínáme | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila Začínáme s Entity Framework série kurzů. Pokud yo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: ba3b65dededeca3534b9273bfd3ae48429711fce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369748"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Použití rozhraní Entity Framework 4.0 a ovládací prvek ObjectDataSource, 1. část: Začínáme
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila [Začínáme s Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série kurzů. Pokud nebyla dokončena v předchozích kurzech, jako výchozí bod pro účely tohoto kurzu můžete [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili. Můžete také [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , který vytvoří kompletní série kurzů.
> 
> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Framework 4.0 a Visual Studio 2010. Ukázková aplikace je web pro fiktivní společnosti Contoso University. Zahrnuje funkce, jako student přijetí, kurz vytvoření a přiřazení instruktorem.
> 
> Tento kurz ukazuje příklady v jazyce C#. [Ukázky ke stažení](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) obsahuje kód v jazyce C# a Visual Basic.
> 
> ## <a name="database-first"></a>První databáze
> 
> Existují tři způsoby, jak můžete pracovat s daty v Entity Framework: *Database First*, *první Model*, a *Code First*. Tento kurz je určen pro první databázi. Informace o rozdílech mezi těmito pracovními postupy a pokyny o tom, jak vybrat tu nejlepší pro váš scénář, naleznete v tématu [Entity Framework vývojářské pracovní postupy](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>webové formuláře
> 
> Podobně jako řady Začínáme v této sérii kurzů používá model webových formulářů ASP.NET a předpokládá, že víte, jak pracovat s webovými formuláři ASP.NET v sadě Visual Studio. Pokud ne, najdete v článku [Začínáme s webovými formuláři ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Pokud budete chtít pracovat s rozhraním ASP.NET MVC framework, přečtěte si téma [Začínáme s Entity Framework pomocí technologie ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Verze softwaru
> 
> | **Uvedené v tomto kurzu** | **Funguje taky s** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web. Tento kurz nebyl byly testovány pomocí novější verze sady Visual Studio. Existují mnoho rozdíly v nabídkách, dialogová okna a šablony. |
> | .NET 4 | Rozhraní .NET 4.5 je zpětně kompatibilní s .NET 4, ale tento kurz nebyl prošel testováním s využitím .NET 4.5. |
> | Entity Framework 4 | Tento kurz nebyl byly testovány pomocí novější verze rozhraní Entity Framework. Od verze Entity Framework 5, ve výchozím nastavení používá EF `DbContext API` , která byla zavedena v systému EF 4.1. Ovládací prvek EntityDataSource byla navržena pro použití `ObjectContext` rozhraní API. Informace o tom, jak používat EntityDataSource ovládacím prvkem `DbContext` rozhraní API, najdete v článku [tento příspěvek na blogu](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Dotazy
> 
> Pokud máte nějaké otázky, které přímo nesouvisejí, najdete v tomto kurzu, můžete je publikovat [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework a LINQ to Entities fórum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), nebo [ StackOverflow.com](http://stackoverflow.com/).


`EntityDataSource` Ovládací prvek umožňuje velmi rychle vytvářet aplikace, ale obvykle vyžaduje značné množství obchodní logika a logika přístupu k datům v udržovat vaše *.aspx* stránky. Pokud očekáváte, že vaše aplikace se jejich složitost v a tak, aby vyžadovala průběžnou údržbu, další dobu vývoje investovat ještě před zahájením Chcete-li vytvořit *n vrstvá* nebo *vrstveným* struktury aplikace je to jednodušší údržbu. Chcete-li implementaci této architektury, oddělte prezentační vrstvy z vrstvy obchodní logiky (BLL) a vrstva přístupu k datům (DAL). Jedním ze způsobů implementace tato struktura je použít `ObjectDataSource` místo ovládacího prvku `EntityDataSource` ovládacího prvku. Při použití `ObjectDataSource` ovládacího prvku, implementovat vlastní kód přístup k datům a pak ho v vyvolat *.aspx* stránek pomocí ovládacího prvku, který má mnoho stejných funkcí jako ostatní ovládací prvky zdroje dat. Díky tomu můžete kombinovat výhody metodiky n vrstvá výhody použití ovládacího prvku webového formuláře pro přístup k datům.

`ObjectDataSource` Řízení poskytuje větší flexibilitu a jinými způsoby. Protože můžete napsat vlastní kód přístup k datům, je jednodušší víc než jenom číst, vložit, aktualizovat nebo odstranit určitý typ entity, které jsou úlohy, která `EntityDataSource` ovládací prvek je určena k provedení. Můžete například provádět protokolování pokaždé, když se aktualizuje entitu, archivace dat pokaždé, když se odstraní entitu, nebo automaticky kontrola a aktualizace souvisejících dat, podle potřeby při vkládání řádků s hodnotou cizího klíče.

## <a name="business-logic-and-repository-classes"></a>Obchodní logika a třídy úložiště

`ObjectDataSource` Ovládací prvek funguje vyvoláním třídu, která vytvoříte. Třída obsahuje metody, které načítají a aktualizují data, a zadejte názvy těchto metod `ObjectDataSource` ovládacího prvku v kódu. Při vykreslování nebo postback zpracování `ObjectDataSource` volá metody, které jste zadali.

Kromě základních operací CRUD, třídy, kterou vytvoříte pro použití s `ObjectDataSource` ovládací prvek může být nutné k provedení obchodní logiku při `ObjectDataSource` načte nebo aktualizuje data. Při aktualizaci oddělení může být například potřeba ověřte, že žádné jiné oddělení mají stejné správce, protože jedna osoba nemůže být správce více než jednom oddělení.

V některých `ObjectDataSource` dokumentaci, jako [přehledu třídy ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), ovládací prvek volá třída označovány jako *obchodní objekt* , který obsahuje obchodní logika a logika přístupu k datům . V tomto kurzu vytvoříte samostatné třídy pro obchodní logika a logika přístupu k datům. Třída, která zapouzdří logiku přístup k datům se nazývá *úložiště*. Třída obchodní logiky zahrnuje obchodní logiku metody a metody pro přístup k datům, ale volání metod přístupu k datům úložiště k provádění úloh přístup k datům.

Můžete také vytvořit abstraktní vrstvu mezi vaším knihoven BLL a DAL, která usnadňuje automatizované jednotky testování BLL. Tento abstraktní vrstvu se implementuje vytvořením rozhraní a vytvoříte instanci úložiště ve třídě obchodní logiku pomocí rozhraní. Díky tomu je možné, abychom vám poskytli obchodní logiku třídy s odkazem na libovolný objekt, který implementuje rozhraní úložiště. Pro běžné operace poskytují objekt úložiště, který funguje s Entity Framework. Pro účely testování, poskytují objekt úložiště, který pracuje s daty uloženými v tak, aby můžete snadno upravit, jako jsou proměnné třídy definovaná jako kolekce.

Následující obrázek ukazuje rozdíl mezi Třída obchodní logiku, která zahrnuje přístup k datům logiku bez úložiště a jednu, která používá úložiště.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Začnete vytvořením webové stránky, ve kterém `ObjectDataSource` ovládací prvek vázán přímo do úložiště, protože provádí jenom základní přístup k datům úlohy. V dalším kurzu vytvoříte třídu obchodní logiky pomocí logiky ověřování a vytvoření vazby `ObjectDataSource` ovládací prvek do třídy místo třídu úložiště. Můžete také vytvořit testy jednotek pro logiku ověřování. V třetím kurzu této série přidáte řazení a filtrování funkce, které aplikace.

Na stránkách v tomto kurzu vytvoříte pracovat `Departments` sady entit datového modelu, který jste vytvořili v [Začínáme série kurzů](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Aktualizace databáze a datového modelu

V tomto kurzu začnete tím, že dvě změny do databáze, které vyžadují odpovídající změny do datového modelu, který jste vytvořili v [Začínáme s Entity Framework a webovými formuláři](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) kurzy. Jedním z těchto kurzů provedené změny v Návrháři ručně pro synchronizaci datového modelu se databázi po změně databáze. V tomto kurzu budete používat návrháři **aktualizaci modelové databáze** nástroj automaticky aktualizovat datový model.

### <a name="adding-a-relationship-to-the-database"></a>Přidání relace do databáze

V sadě Visual Studio, otevřete Contoso University webovou aplikaci, kterou jste vytvořili v [Začínáme s Entity Framework a webovými formuláři](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série kurzů a pak otevřete `SchoolDiagram` databázového diagramu.

Když se podíváte na `Department` tabulky v databázovém diagramu se zobrazí, že na něm `Administrator` sloupce. Tento sloupec je cizí klíč `Person` tabulky, ale žádné relace cizího klíče je definován v databázi. Budete muset vytvořit relaci a aktualizovat datový model, aby rozhraní Entity Framework můžete automaticky zpracovává tuto relaci.

V databázovém diagramu, klikněte pravým tlačítkem `Department` tabulce a vybrat **vztahy**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

V **vztahy cizího klíče** klepněte **přidat**, klikněte na tři tečky u **specifikace tabulek a sloupců**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

V **tabulky a sloupce** dialogové okno pole, nastavit primární klíč tabulky a pole `Person` a `PersonID`a nastavte tabulka cizího klíče a pole `Department` a `Administrator`. (Když toto provedete, se změní název relace z `FK_Department_Department` k `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Klikněte na tlačítko **OK** v **tabulky a sloupce** klikněte **Zavřít** v **vztahy cizího klíče** pole a uložení změn. Pokud budete vyzváni, zda chcete uložit `Person` a `Department` tabulky, klikněte na tlačítko **Ano**.

> [!NOTE]
> Pokud jste odstranili `Person` řádky, které odpovídají data, která je již `Administrator` sloupce, nebude možné uložit změny. V takovém případě použijte editor tabulek v **Průzkumníka serveru** k Ujistěte se, že `Administrator` hodnota v každé `Department` řádek obsahuje ID záznamu, který skutečně existuje v `Person` tabulky.
> 
> Po uložení změn, nebude moct odstranit řádek z `Person` tabulky, pokud to je oddělení správce. V produkční aplikace je při odstranění brání omezení databáze, nebo zadáte kaskádové odstranění by zadat určité chybové zprávě. Příklad toho, jak určit kaskádové odstranění, naleznete v tématu [rozhraní Entity Framework a ASP.NET – získání spustit část 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Přidání zobrazení do databáze

Na novém *Departments.aspx* stránce, která vytvoříte, chcete poskytnout rozevíracího seznamu instruktorů, s názvy ve formátu "nejprve naposledy" tak, aby uživatelé mohli vybrat správci oddělení. Aby bylo snazší to udělat, vytvoříte zobrazení v databázi. Zobrazení bude obsahovat pouze data vyžadovaná rozevíracího seznamu: úplný název (ve správném formátu) a klíč záznamu.

V **Průzkumníka serveru**, rozbalte *School.mdf*, klikněte pravým tlačítkem myši **zobrazení** a pak zvolte položku **přidat nové zobrazení**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Klikněte na tlačítko **Zavřít** při **přidat tabulku** dialogové okno se zobrazí a vložte následující příkaz SQL do podokna SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Uložení pohledu jako `vInstructorName`.

### <a name="updating-the-data-model"></a>Aktualizace datového modelu

V *DAL* složku, otevřete *SchoolModel.edmx* souboru, klikněte pravým tlačítkem na návrhové ploše a vyberte **aktualizace modelů z databáze**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

V **zvolte vaše databázové objekty** dialogové okno, vyberte **přidat** kartě a vyberte zobrazení, které jste právě vytvořili.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Klikněte na tlačítko **Dokončit**.

V návrháři, uvidíte, že nástroj vytvořili `vInstructorName` entity a nové přidružení mezi `Department` a `Person` entity.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> V **výstup** a **seznam chyb** windows, může se zobrazit upozornění oznamující, že nástroj automaticky vytvoří primární klíč pro nový `vInstructorName` zobrazení. Toto je očekávané chování.


Když budete odkazovat na nové `vInstructorName` entity v kódu, které nechcete použít vytváření databáze z předpony malé "v" k němu. Proto se přejmenovat entity a sadu entit v modelu.

Otevřít **Model prohlížeče**. Zobrazí `vInstructorName` uveden jako typ entity a zobrazení.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

V části **nazvaného SchoolModel** (ne **SchoolModel.Store**), klikněte pravým tlačítkem na **vInstructorName** a vyberte **vlastnosti**. V **vlastnosti** okno Změnit **název** vlastnost "InstructorName" a změnit **název sady entit** vlastnost "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Uložit a zavřít datový model a pak znovu sestavit projekt.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Pomocí třídy úložiště a ovládacího prvku ObjectDataSource

Vytvořte nový soubor třídy v *DAL* složky, pojmenujte ho *SchoolRepository.cs*a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Tento kód poskytuje jedinou `GetDepartments` metodu, která vrátí všechny entity v `Departments` sady entit. Protože víte, že vám budou přistupovat k `Person` navigační vlastnost pro každý řádek vrácených, můžete zadat nemůžou dočkat, až načítání pro danou vlastnost pomocí `Include` metody. Třída také implementuje `IDisposable` rozhraní k zajištění, že připojení k databázi je uvolněna při uvolnění objektu.

> [!NOTE]
> Běžnou praxí je pro vytvoření třídy úložiště pro každý typ entity. V tomto kurzu se používá jednu třídu úložiště pro několik typů entit. Další informace o modelu úložiště, najdete v příspěvcích ve [blog týmu rozhraní Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) a [Julie Lerman blogu](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


`GetDepartments` Vrátí metoda `IEnumerable` objekt spíše než výjimku `IQueryable` objektu, aby se zajistilo, že vrácená kolekce je použitelná, i když je uvolněn samotného objektu úložiště. `IQueryable` Objekt může způsobit, že přístup k databázi vždy, když se přistupuje, ale objekt úložiště může být uvolněn dobou, ovládací prvek databound pokusí vykreslit data. Mohli byste například vrátit jiný typ kolekce, například `IList` místo objektu `IEnumerable` objektu. Ale vrácení `IEnumerable` objekt zajistí, že můžete provádět úlohy zpracování typické jen pro čtení seznamu jako `foreach` smyčky a dotazů LINQ, ale nelze přidat nebo odebrat položky v kolekci, což může znamenat, že tyto změny budou ukládají do databáze.

Vytvoření *Departments.aspx* stránka, která používá *Site.Master* stránku předlohy a přidejte následující kód v `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Tento kód vytvoří `ObjectDataSource` ovládací prvek, který používá třídu úložiště, který jste právě vytvořili, a `GridView` ovládací prvek pro zobrazení údajů. `GridView` Ovládací prvek určuje **upravit** a **odstranit** příkazy, ale nepřidali kódu, který je zatím podporuje.

Použití několika sloupců `DynamicField` ovládací prvky, takže můžete využít výhod funkcí pro formátování i ověřování automatické dat. Pro tyto práci, budete muset volat `EnableDynamicData` metoda ve `Page_Init` obslužné rutiny události. (`DynamicControl` nepoužívají se v ovládacích prvcích `Administrator` pole, proto není kompatibilní s navigační vlastnosti.)

`Vertical-Align="Top"` Atributy bude důležité později při přidání sloupce, který má vnořený `GridView` ovládací prvek mřížky.

Otevřít *Departments.aspx.cs* soubor a přidejte následující `using` – příkaz:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Pak přidejte následující obslužnou rutinu pro na stránce `Init` události:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

V *DAL* složku, vytvořte nový soubor třídy *Department.cs* a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Tento kód přidá metadata do datového modelu. Určuje, že `Budget` vlastnost `Department` entity ve skutečnosti představuje měnu, i když je jeho datový typ `Decimal`, a určuje, že hodnota musí být mezi 0 a 1,000,000.00 $. Také určuje, že `StartDate` vlastnost by měla být ve formátu datum ve formátu mm/dd/rrrr.

Spustit *Departments.aspx* stránky.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Všimněte si, že i když jste nezadali řetězec formátu *Departments.aspx* kódu stránky pro **rozpočtu** nebo **datum zahájení** výchozí sloupce, měny a data formátování byl použit k nim pomocí `DynamicField` ovládací prvky, pomocí metadat, který jste zadali v *Department.cs* souboru.

## <a name="adding-insert-and-delete-functionality"></a>Přidání Insert a Delete funkce

Otevřít *SchoolRepository.cs*, přidejte následující kód k vytvoření `Insert` – metoda a `Delete` metoda. Tento kód také obsahuje metodu s názvem `GenerateDepartmentID` , která vypočítá další hodnotu klíče záznamu k dispozici pro použití `Insert` metody. Toto je požadované, protože databáze není nakonfigurovaná k výpočtu automaticky pro `Department` tabulky.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Attach – metoda

`DeleteDepartment` Volání metod `Attach` metodou znovu vytvořit odkaz, který je udržován v kontextu objektu správce stavu objektů mezi entitou v paměti a databází řádek představuje. Toto musí předcházet volání metod `SaveChanges` metody.

Termín *kontextu objektu* odkazuje na třídu Entity Framework, která je odvozena z `ObjectContext` třídu, která používáte pro přístup k sad entit a entit. V kódu pro tento projekt, je název třídy `SchoolEntities`, a její instanci je vždy pojmenováno `context`. Objekt kontextu *správce stavu objektu* je třída, která je odvozena z `ObjectStateManager` třídy. Objekt contact používá správce stavu objektu ukládat objekty entity a ke sledování, zda každý z nich je synchronizovaný s jeho odpovídajícím řádku tabulky nebo řádků v databázi.

Při čtení entity kontextu objektu ukládá ho do Správce stavu objektu a sleduje, zda tohoto vyjádření objektu je synchronizovaný s databází. Například pokud změníte hodnotu vlastnosti, je nastaven příznak označující, že vlastnost, kterou jste změnili už nejsou synchronizované s databází. Potom při volání `SaveChanges` metody objektu kontextu ví, co dělat, protože správce stavu objektu ví přesně Čím se liší mezi aktuální stav entity a stav databáze v databázi.

Ale tento proces obvykle nefunguje ve webové aplikaci, protože tato instance kontextu objektu, která načte entity, spolu s všechno, co v jeho objekt state manager a je uvolněn po vykreslení stránky. Instance kontextu objektu, který se musí použít změny je nový, která je vytvořena instance pro zpracování zpětného volání. V případě třídy `DeleteDepartment` metody `ObjectDataSource` ovládací prvek znovu vytvoří původní verzi entity pro vás z hodnoty v zobrazení stavu, ale to opětovném vytváření `Department` entita neexistuje v objektu správce stavu. Pokud jste volali `DeleteObject` metody v této entitě znovu vytvořena volání by selhat, protože kontext objektu neví, zda entita je synchronizovaný s databází. Však voláním `Attach` metoda znovu naváže stejné sledování mezi znovu vytvořit entitu a hodnotami v databázi, která byla původně provádí automaticky při entity byl načten v dřívější instance kontextu objektu.

Existují situace, když nechcete, aby objekt kontextu ke sledování entit v správce stavu objektu a příznaky tak, aby toho můžete nastavit. Příklady jsou uvedeny v dalších kurzech v této sérii.

### <a name="the-savechanges-method"></a>Metoda SaveChanges

Tato třída jednoduché úložiště znázorňuje základní principy k provádění operací CRUD. V tomto příkladu `SaveChanges` metoda je volána ihned po jednotlivých aktualizací. V produkčním prostředí aplikace můžete chtít volat `SaveChanges` metoda ze samostatné metodě získáte větší kontrolu nad při aktualizaci databáze. (Na konci v dalším kurzu najdete odkaz na dokument white paper, který se zabývá jednotek práce vzor, který je jedním z přístupů k koordinace související aktualizace.) Všimněte si také, že v tomto příkladu `DeleteDepartment` způsob neobsahuje kód pro zpracování konfliktů souběžnosti, přidá se kód k tomu v pozdějších kurzech v této sérii.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Načítání názvů instruktorem a vyberte při vkládání

Uživatelé musí být možné vybrat správce ze seznamu školitelů v rozevíracím seznamu při vytváření nového oddělení. Proto přidejte následující kód, který *SchoolRepository.cs* vytvoření metody k získání seznamu instruktorů pomocí zobrazení, které jste vytvořili dříve:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Vytvoření stránky pro vkládání oddělení

Vytvoření *DepartmentsAdd.aspx* stránka, která používá *Site.Master* stránku a přidejte následující kód v `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Tento kód vytvoří dva `ObjectDataSource` ovládací prvky, jeden pro vkládání nových `Department` entity a jeden pro načítání názvů kurzů vedených pro `DropDownList` ovládací prvek, který se používá pro výběr správci oddělení. Vytvoří značku `DetailsView` ovládací prvek pro zadání nového oddělení a určuje obslužné rutiny pro ovládací prvek `ItemInserting` události tak, že můžete nastavit `Administrator` hodnoty cizího klíče. Na konci je `ValidationSummary` ovládací prvek pro zobrazení chybové zprávy.

Otevřít *DepartmentsAdd.aspx.cs* a přidejte následující `using` – příkaz:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Přidejte následující proměnné třídy a metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init` Metoda aktivovat funkci Dynamická Data. Obslužné rutiny pro `DropDownList` ovládacího prvku `Init` uloží odkaz na ovládací prvek a obslužnou rutinu pro událost `DetailsView` ovládacího prvku `Inserting` událostí používá tento odkaz zobrazíte `PersonID` hodnotu vybrané instruktorem a aktualizace `Administrator` vlastnosti cizího klíče `Department` entity.

Spuštění stránky, přidejte informace o nové oddělení a klikněte **vložit** odkaz.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Zadejte hodnoty pro nové jiného oddělení. Zadejte číslo větší než 1,000,000.00 v **rozpočtu** pole a karty na další položku. V poli se zobrazí hvězdička, a pokud nad ním podržíte ukazatel myši, zobrazí se chybová zpráva, kterou jste zadali v metadatech pro toto pole.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Klikněte na tlačítko **vložit**, a zobrazí chybová zpráva, zobrazí `ValidationSummary` ovládacího prvku v dolní části stránky.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

V dalším kroku zavřete prohlížeč a otevře *Departments.aspx* stránky. Přidat možnost delete *Departments.aspx* stránce přidáním `DeleteMethod` atribut `ObjectDataSource` ovládacího prvku a `DataKeyNames` atribut `GridView` ovládacího prvku. Počáteční značky pro tyto ovládací prvky bude nyní vypadat podobně jako v následujícím příkladu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Spuštění stránky.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Odstranit oddělení, které jste přidali, když jste spustili *DepartmentsAdd.aspx* stránky.

## <a name="adding-update-functionality"></a>Přidání funkce aktualizace

Otevřít *SchoolRepository.cs* a přidejte následující `Update` metody:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Po kliknutí na **aktualizace** v *Departments.aspx* stránky, `ObjectDataSource` ovládací prvek vytvoří dva `Department` entity k předání do `UpdateDepartment` metody. Jeden obsahuje původní hodnoty, které jsou uložené v zobrazení stavu a druhý obsahuje nové hodnoty, které byly zadány v `GridView` ovládacího prvku. Kód v `UpdateDepartment` metoda předává `Department` entita, která má původní hodnoty, které mají `Attach` metodu pro vytvoření sledování mezi entitou a co je v databázi. Poté předá kód `Department` entita, která má nové hodnoty, které mají `ApplyCurrentValues` metody. Objekt kontextu porovná staré a nové hodnoty. Pokud nová hodnota se liší od původní hodnoty, kontext objektu se změní hodnota vlastnosti. `SaveChanges` Metoda pak aktualizuje pouze změněné sloupce v databázi. (Ale pokud funkci aktualizace pro tuto entitu byly mapovány na uložené procedury, celý řádek se aktualizují bez ohledu na to, které byly změněny sloupců.)

Otevřít *Departments.aspx* a přidejte následující atributy, které se `DepartmentsObjectDataSource` ovládacího prvku:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Tato staré hodnoty způsobí, že bude uložená v zobrazení stavu tak, že může být porovnána s novými hodnotami v `Update` metody.
- `OldValuesParameterFormatString="orig{0}"`   
 Tento příkaz informuje ovládací prvek, který je název původní hodnoty parametru `origDepartment` .

Značky pro úvodní značce prvku `ObjectDataSource` ovládací prvek teď vypadá podobně jako v následujícím příkladu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Přidat `OnRowUpdating="DepartmentsGridView_RowUpdating"` atribut `GridView` ovládacího prvku. Použijete toto nastavení `Administrator` hodnotu vlastnosti na základě řádku uživatel vybere v rozevíracím seznamu. `GridView` Počáteční značce teď vypadá podobně jako v následujícím příkladu:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Přidat `EditItemTemplate` pro ovládací prvek `Administrator` sloupec, který se `GridView` řídit bezprostředně po `ItemTemplate` ovládací prvek pro tento sloupec:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

To `EditItemTemplate` ovládací prvek je podobný `InsertItemTemplate` v ovládacím prvku *DepartmentsAdd.aspx* stránky. Rozdíl je, že počáteční hodnota ovládacího prvku se nastavuje pomocí `SelectedValue` atribut.

Před `GridView` řídit, přidejte `ValidationSummary` ovládací prvek, jako jste to udělali *DepartmentsAdd.aspx* stránky.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Otevřít *Departments.aspx.cs* a hned po deklaraci částečné třídy přidejte následující kód k vytvoření soukromého pole tak, aby odkazovaly `DropDownList` ovládacího prvku:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Pak přidejte obslužné rutiny pro `DropDownList` ovládacího prvku `Init` událostí a `GridView` ovládacího prvku `RowUpdating` události:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Obslužná rutina pro `Init` události uloží odkaz na `DropDownList` ovládací prvek v poli třídy. Obslužná rutina pro `RowUpdating` událostí používá odkaz k získání hodnoty zadané uživatelem a použít ji k `Administrator` vlastnost `Department` entity.

Použití *DepartmentsAdd.aspx* stránce Přidat jiného oddělení, a spusťte *Departments.aspx* stránky a klikněte na tlačítko **upravit** na řádek, který jste přidali.

> [!NOTE]
> Nebude moci upravit řádky, které jste nepřidali (to znamená, které již byly v databázi), protože neplatná data v databázi. Správci pro řádky, které byly vytvořeny s databází jsou pro studenty. Pokud se pokusíte upravit jeden z nich, zobrazí se chybová stránka, která hlásí chybu, jako jsou `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Pokud zadáte neplatný **rozpočtu** částka a potom klikněte na tlačítko **aktualizace**, uvidíte stejnou hvězdičku a chybová zpráva, která jste viděli v *Departments.aspx* stránky.

Změňte hodnotu pole nebo vyberte jiný a klikněte na tlačítko **aktualizace**. Zobrazí se tato změna.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Dokončení tohoto postupu Úvod k používání `ObjectDataSource` ovládací prvek pro základní CRUD (vytváření, čtení, aktualizace nebo odstranění) operací s Entity Framework. Sestavíte jednoduchou n vrstvou aplikaci, ale vrstvy obchodní logiky je stále úzce spojeny s vrstvy přístupu k datům, což ztěžuje automatizované testování částí. V následujícím kurzu uvidíte, jak implementovat vzor úložiště usnadňuje testování částí.

> [!div class="step-by-step"]
> [Next](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
