---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Pokročilé scénáře Entity Framework 6 pro aplikaci MVC 5 Web (12 12) | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 50987b30a49173605e7aeb8eb65ff1fe5d5e5753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877447"
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>Pokročilé Entity Framework 6 scénáře pro aplikaci MVC 5 Web (12 12)
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stáhnout PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio 2013. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


V tomto kurzu předchozí implementována tabulky na hierarchii dědičnosti. Tento kurz zahrnuje zavádí několik témat, které jsou vhodné pro zajímat, když přejdete mimo se základy vývoje webové aplikace ASP.NET, které používají Entity Framework Code First. Podrobné pokyny vás provedou kód a pomocí sady Visual Studio pro následující témata:

- [Provádění nezpracovaná dotazy SQL](#rawsql)
- [Provádění dotazů bez sledování](#notracking)
- [Zkoumání SQL odeslal do databáze](#sql)

Kurz představuje několik témat s stručné úvodní informace a odkazy na prostředky pro další informace:

- [Úložiště a jednotky pracovních vzorů](#repo)
- [Třídy proxy](#proxies)
- [Detekce automatických změn](#changedetection)
- [Automatické ověření](#validation)
- [EF tools pro Visual Studio](#tools)
- [Entity Framework zdrojového kódu](#source)

Tento kurz obsahuje následující oddíly:

- [Shrnutí](#summary)
- [Potvrzování](#acknowledgments)
- [Poznámka o jazyka Visual Basic](#vb)
- [Běžné chyby a řešení či alternativní řešení pro ně](#errors)

Pro většinu těchto témat bude fungovat s stránek, které jste už vytvořili. Použití nezpracovaná SQL k hromadné aktualizace vytvoříte novou stránku, která aktualizuje počet kredity všech kurzů v databázi:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Provádění dotazů nezpracovaná SQL

První rozhraní API sady Entity Framework kód obsahuje metody, které vám umožní předat příkazy SQL přímo do databáze. Máte následující možnosti:

- Použití [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) metoda pro dotazy, které vracejí typy entit. Vrácených objektů musí být na typ očekávaný `DbSet` objektu a jsou automaticky sledovány objektem kontext databáze není-li vypnout sledování. (Najdete v následující části [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) metoda.)
- Použití [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) metoda pro dotazy, které návratové typy, které nejsou entity. Vrácená data není sledována kontext databáze, i když používáte tuto metodu pro načtení typů entit.
- Použití [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) pro příkazy nejsou dotazem.

Jednou z výhod použití rozhraní Entity Framework je, že zabraňuje příkazů kódu příliš úzce na konkrétní metodu ukládání dat. Dělá to pomocí generování SQL dotazy a příkazy, které také s není pro zápis sami. Ale existují výjimečných scénáře, kdy budete muset spustit konkrétní dotazy SQL, které jste vytvořili ručně a tyto metody umožňují vám zpracovávat takové výjimky.

Jak platí vždy při spuštění příkazů SQL ve webové aplikaci, je nutné provést opatření k ochraně vaší lokality před útoky Injektáž SQL. Jeden způsob, jak to udělat, je použití parametrických dotazů a ujistěte se, že řetězců odeslána webová stránka se nedá interpretovat jako příkazy SQL. V tomto kurzu budete používat parametrizované dotazy při integraci uživatelský vstup do dotazu.

### <a name="calling-a-query-that-returns-entities"></a>Volání metody dotazu, který vrací entity

[DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) třída poskytuje metodu, která můžete použít k provedení dotazu, který vrací entity typu `TEntity`. Pokud chcete zobrazit, jak to funguje můžete budete změnit kód v `Details` metodu `Department` řadiče.

V *DepartmentController.cs*v `Details` metoda, nahraďte `db.Departments.FindAsync` volání metody s `db.Departments.SqlQuery` volání metody, jak je znázorněno v následující zvýrazněný kód:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Chcete-li ověřit, že nový kód funguje správně, vyberte **oddělení** kartu a potom **podrobnosti** pro jednu z oddělení.

![Podrobnosti o oddělení](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Volání metody dotazu, který vrátí jiné typy objektů

Dříve jste vytvořili mřížka student statistiky o stránky, která vám ukázal, počet studenty pro každé datum registrace. Kód, který to v *HomeController.cs* používá LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Předpokládejme, že chcete napsat kód, který načte tato data přímo v SQL, nikoli pomocí LINQ. Pokud chcete provést, že budete muset spustit dotaz, který vrátí něco jiného než entity objektů, což znamená, že musíte použít [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) metoda.

V *HomeController.cs*, nahraďte příkaz LINQ v `About` metoda s příkazem SQL, jak je znázorněno v následující zvýrazněný kód:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

O stránku spustíte. Zobrazuje stejná data, která předtím.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>Volání příkazu Update jazyka

Předpokládejme, že správci Contoso univerzity chcete být schopni provést hromadně změny v databázi, jako je například změna číslo kredity u každé kurzu. Pokud univerzity má velký počet kurzy, je neefektivní je načtou všechny jako entity a měnit je jednotlivě. V této části budete implementovat na webové stránce, která umožňuje uživateli zadat faktor, podle kterého chcete změnit číslo kredity u všech kurzů a budete proveďte požadovanou změnu spuštěním SQL `UPDATE` příkaz. Webová stránka bude vypadat jako na následujícím obrázku:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

V *CourseContoller.cs*, přidejte `UpdateCourseCredits` metody pro `HttpGet` a `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Pokud kontroler zpracovává `HttpGet` požadavku, nic nevrátí v `ViewBag.RowsAffected` proměnnou a zobrazení zobrazí prázdné textové pole a tlačítko pro odeslání, jak je vidět na předchozím obrázku.

Když **aktualizace** po kliknutí na tlačítko `HttpPost` metoda je volána, a `multiplier` je hodnota zadaná v textovém poli. Kód pak provede SQL, který aktualizuje kurzy a vrátí počet ovlivněných řádků zobrazení v `ViewBag.RowsAffected` proměnné. Po zobrazení získá hodnotu, v tom, že proměnné, zobrazí počet řádků aktualizovat místo textového pole a tlačítko, odeslat, jak je znázorněno na následujícím obrázku:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

V *CourseController.cs*, klikněte pravým tlačítkem na jednu z `UpdateCourseCredits` metody a pak klikněte na tlačítko **přidat zobrazení**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

V *Views\Course\UpdateCourseCredits.cshtml*, nahraďte kód šablony s následujícím kódem:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Spustit `UpdateCourseCredits` metoda výběrem **kurzy** kartě následným přidáním "/ UpdateCourseCredits" na konec adresy URL v adresním řádku prohlížeče (například: `http://localhost:50205/Course/UpdateCourseCredits`). Zadejte číslo do textového pole:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Klikněte na tlačítko **aktualizace**. Zobrazí počet ovlivněných řádků:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Klikněte na tlačítko **zpět do seznamu** zobrazíte seznam kurzy revidovaný počet kredity.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Další informace o nezpracovanou dotazy SQL najdete v tématu [nezpracovaná dotazy SQL](https://msdn.microsoft.com/data/jj592907) na webu MSDN.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>Ne sledování dotazy

Když kontext databáze načítá řádky tabulky, vytvoří objekty entity, které představují je ve výchozím nastavení se uchovává informace o jestli entity v paměti jsou synchronizované s co je v databázi. Data v paměti funguje jako mezipaměť a používá se při aktualizaci entity. Toto použití mezipaměti se často nepotřebné ve webové aplikaci protože kontextu instance jsou obvykle krátkodobou (nový jeden se vytvoří a uvolnění pro každý požadavek) a objektem context, který čte entitu je obvykle zveřejněn. před použitím dané entity znovu.

Sledování objekty entity v paměti můžete zakázat pomocí [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) metoda. Typické scénáře, ve kterých můžete chtít udělat, patří:

- Dotaz načte velkého objemu dat, která vypnutí sledování může výrazně zvýšit výkon.
- Chcete připojit entity aktualizujte ji, ale jste dříve získali stejné entity pro jiný účel. Vzhledem k tomu, že entita je již sledován kontext databáze, nemůžete připojit entity, který chcete změnit. Jedním ze způsobů ke zpracování této situace je použít `AsNoTracking` možnost s dřívější dotazu.

Pro příklad, který ukazuje, jak používat [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) metodu, najdete v části [starší verze tohoto kurzu](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Tato verze kurzu nenastavil příznak změněné u entity vytvořené vazače modelu v metodě úpravy, nepotřebuje `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Zkoumání SQL odeslal do databáze

V některých případech je vhodné se moci zobrazit skutečné dotazy SQL, které se odesílají do databáze. V dřívějších kurzu jste viděli, jak to provést v kódu interceptoru; Nyní se zobrazí několik způsobů, jak provést bez nutnosti psaní kódu interceptoru. Pokud chcete vyzkoušet tento limit, budete podívejte se na jednoduchý dotaz a podívejte se na co se stane k němu při přidávání možnosti takové eager načítání, filtrování a řazení.

V *řadiče nebo CourseController*, nahraďte `Index` metoda s následujícím kódem, chcete-li dočasně pozastavit přes načítání:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Nyní nastavit zarážky `return` – příkaz (F9 umístěte kurzor na tuto linku). Stisknutím klávesy F5 spusťte projekt v režimu ladění a vyberte stránku kurzu Index. Pokud kód dosáhne zarážky, podívejte se `sql` proměnné. Zobrazí dotaz, který je odeslána do systému SQL Server. Je jednoduchý `Select` příkaz.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Klikněte na třídu lupy zobrazíte dotazu ve **Text vizualizér**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Nyní přidáte rozevíracího seznamu na kurzy indexovou stránku tak, aby uživatelé můžete filtrovat pro konkrétní oddělení. Kurzy budete řazení podle názvu, a specifikujete, přes načítání pro `Department` navigační vlastnost.

V *CourseController.cs*, nahraďte `Index` metoda následujícím kódem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Obnovení zarážce na `return` příkaz.

Metoda přijímá vybranou hodnotu rozevíracím seznamu v `SelectedDepartment` parametr. Pokud není nic vybráno, bude mít tento parametr hodnotu null.

A `SelectList` kolekce obsahující všechna oddělení byla předána do zobrazení pro rozevíracího seznamu. Parametry předaný `SelectList` konstruktor zadejte pole Název hodnoty, názvu pole text a vybranou položku.

Pro `Get` metodu `Course` úložiště, určuje kód výraz filtru, řazení a načítání pro nápovědy eager `Department` navigační vlastnost. Výraz filtru vždy vrátí `true` Pokud není nic vybráno v rozevíracím seznamu (tedy `SelectedDepartment` má hodnotu null).

V *Views\Course\Index.cshtml*, bezprostředně před otevření `table` značky, přidejte následující kód k vytvoření rozevíracího seznamu a tlačítko odeslání:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Se zarážkou nastaven, spustit během indexovou stránku. Pokračujte přes první časy, že kód dotkne zarážku, tak, aby stránka se zobrazí v prohlížeči. Vyberte oddělení z rozevíracího seznamu a klikněte na **filtru**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

První zarážky této doby bude pro dotaz oddělení pro rozevíracího seznamu. Přeskočit a zobrazit `query` proměnné příštím kód dosáhne zarážky Chcete-li zobrazit, co `Course` dotazu teď vypadá jako. Zobrazí se přibližně takto:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Uvidíte, že dotaz je teď `JOIN` dotaz, který načte `Department` data spolu s `Course` data a že obsahuje `WHERE` klauzule.

Odeberte `var sql = courses.ToString()` řádku.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Úložiště a jednotky pracovních vzorů

Celá řada vývojářů napsat kód pro implementaci úložiště a jednotky pracovních vzorů jako obálku kolem kód, který funguje s platformou Entity Framework. Tyto vzory jsou určeny k vytvoření vrstvy abstrakce mezi vrstva přístupu k datům a vrstvu obchodní logiky aplikace. Implementace tyto vzory pomůžou izolovat aplikace od změny v úložišti dat a mohou usnadnit automatizované testování částí nebo testy řízený vývoj (TDD). Ale zápis další kód implementovat tyto vzory není vždy je nejlepší pro aplikace, které používají EF, z několika důvodů:

- Vlastní třídy kontextu EF insulates kódu z daného obchodu data kódu.
- Třída kontextu EF může fungovat také třídu pracovní jednotky pro databázi aktualizace, který používáte pomocí EF.
- Funkce byla zavedená v Entity Framework 6 usnadňují implementaci TDD bez nutnosti psaní kódu úložiště.

Další informace o tom, jak implementovat úložiště a jednotky pracovních vzorů najdete v tématu [verze Entity Framework 5 tohoto kurzu řady](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Informace o způsoby, jak implementovat TDD na Entity Framework 6 najdete v následujících zdrojích informací:

- [Jak EF6 umožňuje Mocking DbSets snadněji](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Testování s mocking framework](https://msdn.microsoft.com/data/dn314429)
- [Testování s vlastními testovací hodnoty Double](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Třídy proxy

Rozhraní Entity Framework vytvoří instancí entit (například při spuštění dotazu), často vytvoří je jako instance dynamicky generovaném odvozený typ, který funguje jako proxy pro entitu. Například prohlédněte si následující dva obrázky ladicí program. V první bitovou kopii, uvidíte, že `student` proměnná je očekávané `Student` zadejte ihned po doložit entity. V druhé bitovou kopii Jakmile EF využita ke čtení entit student z databáze, zobrazí třídu proxy.

![Před třídu proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Po třídu proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Tato třída proxy serveru přepíše některé virtuální vlastnosti entity, která má vložit háky pro provádění akcí automaticky při přístupu k vlastnosti. Jednu funkci, tento mechanismus se používá pro je opožděného načítání.

Ve většině případů nemusíte mít na paměti toto použití proxy, ale existují výjimky:

- V některých případech můžete chtít zabránit ve vytváření instancí proxy rozhraní Entity Framework. Například pokud jste serializaci entity obecně chcete třídy objektů POCO, není třídy proxy serveru. Jedním ze způsobů se chcete vyhnout potížím serializace je serializovat objekty přenos dat (DTOs) namísto objekty entity, jak je znázorněno [pomocí webového rozhraní API s platformou Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) kurzu. Dalším způsobem je [zakázat vytvoření proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Když vytváříte instance třídy entity pomocí `new` operátor, neobdržíte její instance. To znamená, že funkce není dostupná, jako je opožděného načítání a automatické sledování změn. Obvykle je to v pořádku; obecně nepotřebujete opožděného načítání, protože vytváříte nové entity, který není v databázi, a obecně nepotřebujete sledování, pokud jste explicitní označení entity jako změn `Added`. Ale pokud potřebujete opožděného načítání a potřebujete sledování změn, můžete vytvořit nové instance entity s proxy pomocí [vytvořit](https://msdn.microsoft.com/library/gg679504.aspx) metodu `DbSet` – třída.
- Můžete chtít získat skutečný typ entity z typu proxy serveru. Můžete použít [Metoda GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) metodu `ObjectContext` třídy pro získání skutečný typ entity instance typu proxy serveru.

Další informace najdete v tématu [práce s proxy](https://msdn.microsoft.com/data/JJ592886.aspx) na webu MSDN.

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>Detekce automatických změn

Rozhraní Entity Framework Určuje, jak došlo ke změně entity (a proto aktualizací, které potřebují k odeslání do databáze) tak, že porovnáte aktuální hodnoty entity s původními hodnotami. Původní hodnoty jsou uloženy, když se entita dotaz nebo připojené. Některé metody, které způsobí změnu automatické zjišťování jsou následující:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Pokud sledujete velký počet entit a jednu z těchto metod zavoláte mnohokrát ve smyčce, můžete dosáhnout výrazné vylepšení výkonu dočasně vypnout pomocí zjišťování automatické změny [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) vlastnost. Další informace najdete v tématu [automaticky detekovat změny](https://msdn.microsoft.com/data/jj556205) na webu MSDN.

<a id="validation"></a>
## <a name="automatic-validation"></a>Automatické ověření

Při volání `SaveChanges` metoda, ve výchozím nastavení rozhraní Entity Framework ověří data v všechny vlastnosti všech změněných entit před aktualizací databáze. Pokud jste aktualizovali velký počet entit a jste již zkontrolujete, data, tento pracovní není nutný, a můžete dokonce vytvářet proces ukládání změn trvat méně času můžete dočasně vypnout ověření. Můžete to, že pomocí [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) vlastnost. Další informace najdete v tématu [ověření](https://msdn.microsoft.com/data/gg193959) na webu MSDN.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Výkonné nástroje Entity Framework

[Výkonné nástroje Entity Framework](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) je doplněk sady Visual Studio, která byla použita k vytvoření datového modelu diagramy zobrazí v těchto kurzech. Nástroje můžete provést taky další funkce, například jako generování tříd entit na základě pro tabulky v existující databázi, aby databáze můžete používat s Code First. Po instalaci nástrojů se zobrazí některé další možnosti v kontextové nabídky. Například když kliknete pravým tlačítkem na třídě kontextu v **Průzkumníku**, získáte možnost k vygenerování diagram. Pokud používáte Code First nelze změnit datový model v diagramu, ale můžete přesunout věcí snadněji pochopili.

![EF v kontextové nabídce](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![EF diagram](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Entity Framework zdrojového kódu

Zdrojový kód pro Entity Framework 6 je k dispozici na [Githubu](https://github.com/aspnet/EntityFramework6). Můžete soubor chyby, a můžete přispívat vlastní vylepšení EF zdrojovém kódu.

I když zdrojový kód je otevřená, rozhraní Entity Framework plně podporovat jako produkt společnosti Microsoft. Týmem Microsoft Entity Framework udržuje řízení, po kterou jsou přijímány příspěvky a otestuje všechny změny kódu k zajištění kvality jednotlivými verzemi.

<a id="summary"></a>
## <a name="summary"></a>Souhrn

Dokončení této série kurzů na používající rozhraní Entity Framework v aplikaci ASP.NET MVC. Další informace o tom, jak pracovat s daty pomocí rozhraní Entity Framework najdete v tématu [EF stránky dokumentace na webu MSDN](https://msdn.microsoft.com/data/ee712907) a [přístup k datům ASP.NET - doporučené prostředky](../../../../whitepapers/aspnet-data-access-content-map.md).

Další informace o tom, jak nasadit webovou aplikaci, až když jste sestavili najdete v tématu [nasazení webu ASP.NET - doporučené prostředky](../../../../whitepapers/aspnet-web-deployment-content-map.md) v knihovně MSDN.

Informace o dalších témat souvisejících s MVC, jako je například ověřování a autorizaci, najdete v článku [ASP.NET MVC – doporučené prostředky](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Potvrzování

- Tní Dykstra napsali původní verzi v tomto kurzu, společně vytvořené aktualizace EF 5 a napsali aktualizace EF 6. Tní je senior programovací zapisovač na webovou platformu společnosti Microsoft a nástrojů týmu obsahu.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) nebyla většinu práce aktualizace kurz pro EF 5 a MVC 4 a společně vytvořené aktualizace EF 6. Rick je senior programovací zapisovač pro Microsoft zaměřením na Azure a MVC.
- [Rowan Lukeš](http://www.romiller.com) a ostatní členové týmu rozhraní Entity Framework asistované s recenze kódu a pomohl ladění mnohé problémy s migrací, které vznikly při byly aktualizujeme kurz pro EF 5 a EF 6.

<a id="vb"></a>
## <a name="vb"></a>VB

Tento kurz bylo původně vytvořeno pro EF 4.1, jsme k dispozici jak C# a VB verzích dokončené stažení projektu. Z důvodu omezení čas a dalších priority jsme neučinili, pro tuto verzi. Pokud sestavení projektu jazyka Visual Basic, pomocí tyto kurzy a by chtěl který sdílet s ostatními, dejte nám vědět.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Běžné chyby a řešení či alternativní řešení pro ně

### <a name="cannot-createshadow-copy"></a>Nelze vytvořit nebo stínové kopie

Chybová zpráva:

> Nelze vytvořit nebo stínové kopie se&lt;filename&gt;, pokud daný soubor již existuje.


Řešení

Počkejte několik sekund a aktualizujte stránku.

### <a name="update-database-not-recognized"></a>Update-Database nebyl rozpoznán

Chybová zpráva (z `Update-Database` v pomocí PMC):

> Termín 'Update-Database' nebyl rozpoznán jako název rutiny, funkce, soubor skriptu nebo spustitelného programu. Zkontrolujte, zda název, nebo pokud byl zahrnut cestu, ověřte, zda je cesta správná a zkuste to znovu.


Řešení

Ukončete aplikaci Visual Studio. Otevřete projekt a zkuste to znovu.

### <a name="validation-failed"></a>Ověření se nezdařilo

Chybová zpráva (z `Update-Database` v pomocí PMC):

> Ověření se nezdařilo pro jednu nebo více entit. Naleznete ve vlastnosti 'EntityValidationErrors' Další podrobnosti.


Řešení

Jednou z příčin tohoto problému je chyby ověření při `Seed` metoda spustí. V tématu [první financování a ladění Entity Framework (EF) databází](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) tipy k ladění `Seed` metoda.

### <a name="http-50019-error"></a>HTTP 500.19 chyby

Chybová zpráva:

> HTTP Error 500.19 - Internal Server Error  
> K požadované stránce nelze přistupovat, protože související konfigurační data pro stránku nejsou platná.


Řešení

Jedním ze způsobů, může se tato chyba je z s více kopií řešení, každý z nich používá stejné číslo portu. Obvykle můžete tento problém vyřešit pomocí ukončení všech instancí sady Visual Studio a projekt, kterou právě pracujete na restartování. Pokud to nefunguje, zkuste změnit číslo portu. Klikněte pravým tlačítkem na soubor projektu a pak klikněte na položku Vlastnosti. Vyberte **webové** kartě a poté změňte číslo portu v **adresa Url projektu** textové pole.

### <a name="error-locating-sql-server-instance"></a>Chyba vyhledáním instance systému SQL Server

Chybová zpráva:

> Při navazování připojení k systému SQL Server došlo k chybě související se sítí nebo konkrétní instancí. Server nebyl nalezen nebo není přístupný. Ověřte, zda je název instance správný a zda systém SQL Server je nakonfigurován k povolení vzdálených připojení. (Zprostředkovatel: rozhraní sítě systému SQL, chyba: 26 - chyba vyhledání Server nebo instanci zadáno)


Řešení

Zkontrolujte připojovací řetězec. Pokud jste odstranili ručně databáze, změňte název databáze v řetězci konstrukce.

> [!div class="step-by-step"]
> [Předchozí](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
