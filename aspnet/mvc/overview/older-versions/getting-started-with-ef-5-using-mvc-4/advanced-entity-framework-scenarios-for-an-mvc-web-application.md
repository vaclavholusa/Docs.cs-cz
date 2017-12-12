---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: "Pokročilé scénáře Entity Framework pro webovou aplikaci MVC (10 10) | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d58a745896b29317c1d1049e3bf1a5ec2e628820
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Pokročilé Entity Framework scénáře pro webové aplikace MVC (10 10)
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio 2012. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Kurz řady můžete začít od začátku nebo [stáhnout počáteční projekt pro tato kapitola](building-the-ef5-mvc4-chapter-downloads.md) a začněte zde.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte, [stáhnout dokončené kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a pokuste se znovu provést váš problém. Řešení problému můžete získat obecně tak, že porovnáte svůj kód Dokončený kód. Pro některé běžné chyby a jak je vyřešit, najdete v části [chyby a řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V tomto kurzu předchozí implementována úložiště a jednotky pracovních vzorů. Tento kurz obsahuje následující témata:

- Provádění nezpracovaná dotazy SQL.
- Provádění dotazů bez sledování.
- Zkoumání dotazy odeslal do databáze.
- Práce s třídami proxy serveru.
- Zakazuje automatické zjišťování změny.
- Zakázání ověřování při ukládání změn.
- [Chyby a pracovní postupy](#errors)

Pro většinu těchto témat bude fungovat s stránek, které jste už vytvořili. Použití nezpracovaná SQL k hromadné aktualizace vytvoříte novou stránku, která aktualizuje počet kredity všech kurzů v databázi:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

A použít dotaz ne sledování přidáte nové logiku ověření pro oddělení upravit stránku:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Provádění dotazů nezpracovaná SQL

První rozhraní API sady Entity Framework kód obsahuje metody, které vám umožní předat příkazy SQL přímo do databáze. Máte následující možnosti:

- Použití `DbSet.SqlQuery` metoda pro dotazy, které vracejí typy entit. Vrácených objektů musí být na typ očekávaný `DbSet` objektu a jsou automaticky sledovány objektem kontext databáze není-li vypnout sledování. (Najdete v následující části `AsNoTracking` metoda.)
- Použití `Database.SqlQuery` metoda pro dotazy, které návratové typy, které nejsou entity. Vrácená data není sledována kontext databáze, i když používáte tuto metodu pro načtení typů entit.
- Použití [Database.ExecuteSqlCommand](https://msdn.microsoft.com/en-us/library/gg679456(v=vs.103).aspx) pro příkazy nejsou dotazem.

Jednou z výhod použití rozhraní Entity Framework je, že zabraňuje příkazů kódu příliš úzce na konkrétní metodu ukládání dat. Dělá to pomocí generování SQL dotazy a příkazy, které také s není pro zápis sami. Ale existují výjimečných scénáře, kdy budete muset spustit konkrétní dotazy SQL, které jste vytvořili ručně a tyto metody umožňují vám zpracovávat takové výjimky.

Jak platí vždy při spuštění příkazů SQL ve webové aplikaci, je nutné provést opatření k ochraně vaší lokality před útoky Injektáž SQL. Jeden způsob, jak to udělat, je použití parametrických dotazů a ujistěte se, že řetězců odeslána webová stránka se nedá interpretovat jako příkazy SQL. V tomto kurzu budete používat parametrizované dotazy při integraci uživatelský vstup do dotazu.

### <a name="calling-a-query-that-returns-entities"></a>Volání metody dotazu, který vrací entity

Předpokládejme, že chcete `GenericRepository` třída zajistit další filtrování a řazení flexibilitu bez nutnosti vytvoření odvozené třídy s další metody. Jeden způsob, jak dosáhnout, která by mohla být přidá metoda, která přijímá dotaz SQL. Můžete nastavit jakýkoli druh filtrování a řazení je chcete v kontroleru, například `Where` klauzuli, která závisí na spojení nebo poddotaz. V této části se zobrazí, jak implementovat tato metoda.

Vytvořte `GetWithRawSql` metoda přidáním následující kód do *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

V *CourseController.cs*, zavolejte metodu nové z `Details` metoda, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

V takovém případě může jste použili `GetByID` metoda, ale používáte `GetWithRawSql` metoda ověřit, jestli `GetWithRawSQL` metoda funguje.

Spustit stránce s podrobnostmi o ověření, že dotaz select funguje (vyberte **kurzu** kartu a potom **podrobnosti** pro jeden kurzu).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Volání metody dotazu, který vrátí jiné typy objektů

Dříve jste vytvořili mřížka student statistiky o stránky, která vám ukázal, počet studenty pro každé datum registrace. Kód, který to v *HomeController.cs* používá LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Předpokládejme, že chcete napsat kód, který načte tato data přímo v SQL, nikoli pomocí LINQ. Pokud chcete provést, že budete muset spustit dotaz, který vrátí něco jiného než entity objektů, což znamená, že musíte použít `Database.SqlQuery` metoda.

V *HomeController.cs*, nahraďte příkaz LINQ v `About` metoda následujícím kódem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

O stránku spustíte. Zobrazuje stejná data, která předtím.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Volání příkazu Update jazyka

Předpokládejme, že správci Contoso univerzity chcete být schopni provést hromadně změny v databázi, jako je například změna číslo kredity u každé kurzu. Pokud univerzity má velký počet kurzy, je neefektivní je načtou všechny jako entity a měnit je jednotlivě. V této části budete implementovat na webové stránce, která umožňuje uživateli zadat faktor, podle kterého chcete změnit číslo kredity u všech kurzů a budete proveďte požadovanou změnu spuštěním SQL `UPDATE` příkaz. Webová stránka bude vypadat jako na následujícím obrázku:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

V tomto kurzu předchozí použít obecné úložiště číst a aktualizovat `Course` entity v `Course` řadiče. Pro tuto operaci hromadné aktualizace musíte vytvořit nová metoda úložiště, který není v úložišti Obecné. K tomu, vytvoříte vyhrazená `CourseRepository` třídu odvozenou od `GenericRepository` třídy.

V *DAL* složku vytvořit *CourseRepository.cs* a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

V *UnitOfWork.cs*, změnit `Course` typu úložiště z `GenericRepository<Course>` na`CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

V *CourseContoller.cs*, přidejte `UpdateCourseCredits` metoda:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Tato metoda se použije pro obě `HttpGet` a `HttpPost`. Když `HttpGet` `UpdateCourseCredits` metoda spustí, `multiplier` proměnná bude mít hodnotu null a zobrazení zobrazí prázdné textové pole a tlačítko pro odeslání, jak je vidět na předchozím obrázku.

Když **aktualizace** po kliknutí na tlačítko a `HttpPost` metoda spustí `multiplier` budou mít hodnoty zadané v textovém poli. Kód pak zavolá úložiště `UpdateCourseCredits` vliv metoda, která vrátí počet řádků, a tato hodnota je uložena v `ViewBag` objektu. Při zobrazení obdrží počet ovlivněných řádků v `ViewBag` objektu, se zobrazí toto číslo místo textového pole a tlačítko, odeslat, jak je znázorněno na následujícím obrázku:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Vytvoření zobrazení v *Views\Course* složku pro stránku kredity během aktualizace:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

V *Views\Course\UpdateCourseCredits.cshtml*, nahraďte kód šablony s následujícím kódem:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Spustit `UpdateCourseCredits` metoda výběrem **kurzy** kartě následným přidáním "/ UpdateCourseCredits" na konec adresy URL v adresním řádku prohlížeče (například: `http://localhost:50205/Course/UpdateCourseCredits`). Zadejte číslo do textového pole:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Klikněte na tlačítko **aktualizace**. Zobrazí počet ovlivněných řádků:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Klikněte na tlačítko **zpět do seznamu** zobrazíte seznam kurzy revidovaný počet kredity.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Další informace o nezpracovanou dotazy SQL najdete v tématu [nezpracovaná dotazy SQL](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) na blog týmu rozhraní Entity Framework.

## <a name="no-tracking-queries"></a>Ne sledování dotazy

Když kontext databáze načte řádky databáze, vytvoří objekty entity, které představují je ve výchozím nastavení se uchovává informace o jestli entity v paměti jsou synchronizované s co je v databázi. Data v paměti funguje jako mezipaměť a používá se při aktualizaci entity. Toto použití mezipaměti se často nepotřebné ve webové aplikaci protože kontextu instance jsou obvykle krátkodobou (nový jeden se vytvoří a uvolnění pro každý požadavek) a objektem context, který čte entitu je obvykle zveřejněn. před použitím dané entity znovu.

Můžete určit, zda kontext sleduje objekty entity pro dotaz s použitím `AsNoTracking` metoda. Typické scénáře, ve kterých můžete chtít udělat, patří:

- Dotaz načte velkého objemu dat, která vypnutí sledování může výrazně zvýšit výkon.
- Chcete připojit entity aktualizujte ji, ale jste dříve získali stejné entity pro jiný účel. Vzhledem k tomu, že entita je již sledován kontext databáze, nemůžete připojit entity, který chcete změnit. Je možné zabránit a používat `AsNoTracking` možnost s dřívější dotazu.

V této části budete implementovat obchodní logiku, která ukazuje, druhý z těchto scénářů. Konkrétně budete vynutit obchodní pravidlo s upozorněním, že lektorem nemůže být správce více než jeden oddělení.

V *DepartmentController.cs*, přidat nové metody, kterou můžete volat z `Edit` a `Create` metody a ujistěte se, že žádné dvě oddělení mají stejné správce:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Přidejte kód v `try` blokování z `HttpPost` `Edit` metoda k volání této nové metody, pokud nejsou žádné chyby ověření. `try` Bloku teď vypadá jako v následujícím příkladu:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Spusťte oddělení upravit stránku a zkuste změnit správce oddělení lektorem, který je již správce jinému oddělení. Můžete získat očekávané chybová zpráva:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Teď spustit oddělení upravit stránku znovu a tato změna čas **nároky** velikost. Když kliknete na tlačítko **Uložit**, se zobrazí chybová stránka:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Chybová zpráva o výjimce "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" tomu dojít z důvodu následující posloupnost událostí:

- `Edit` Volání metod `ValidateOneAdministratorAssignmentPerInstructor` metodu, která načte všechny oddělení, které mají Jan Novak jako jejich správce. Anglickou oddělení čtení, která způsobí. Protože se jedná o oddělení upravovaný, žádná chybová zpráva. V důsledku této operace čtení ale anglické oddělení entita, která byla načtena z databáze je nyní sledován kontext databáze.
- `Edit` Metoda pokusí se nastavit `Modified` příznak na anglickém oddělení entity vytvořené vazač modelu MVC, ale který nezdaří, protože kontext je již sledování entity pro angličtinu oddělení.

Jedno řešení tohoto problému je zabránit sledování v paměti oddělení entity načíst pomocí dotazu ověření kontextu. Neexistuje žádné nevýhodou na to, protože nebude aktualizuje tuto entitu nebo čtení znovu tak, že by těžit z něj uložená v mezipaměti.

V *DepartmentController.cs*v `ValidateOneAdministratorAssignmentPerInstructor` metoda, zadejte žádné sledování, jak je znázorněno v následujícím:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Opakujte pokus o úpravu **nároky** množství oddělení. Tuto chvíli je operace úspěšná, a vrátí webu podle očekávání na oddělení indexovou stránku, zobrazuje hodnotu revidovaný rozpočet.

## <a name="examining-queries-sent-to-the-database"></a>Zkoumání dotazy odeslal do databáze

V některých případech je vhodné se moci zobrazit skutečné dotazy SQL, které se odesílají do databáze. K tomuto účelu můžete prozkoumat proměnné dotazu v ladicím programu nebo volání dotazu `ToString` metoda. Pokud chcete vyzkoušet tento limit, budete podívejte se na jednoduchý dotaz a podívejte se na co se stane k němu při přidávání možnosti takové eager načítání, filtrování a řazení.

V *řadiče nebo CourseController*, nahraďte `Index` metoda následujícím kódem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Nyní nastavte zarážky *GenericRepository.cs* na `return query.ToList();` a `return orderBy(query).ToList();` prohlášení o `Get` metoda. Spusťte projekt v režimu ladění a vyberte stránku kurzu Index. Pokud kód dosáhne zarážky, podívejte se `query` proměnné. Zobrazí dotaz, který je odeslána do systému SQL Server. Je jednoduchý `Select` příkaz:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Dotazy může být příliš dlouhý pro zobrazení v ladění systému windows v sadě Visual Studio. Chcete-li zobrazit celý dotaz, můžete zkopírovat hodnotu proměnné a vložte ho do textového editoru:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Nyní přidáte rozevíracího seznamu na stránku kurzu indexu tak, aby uživatelé můžete filtrovat pro konkrétní oddělení. Kurzy budete řazení podle názvu, a specifikujete, přes načítání pro `Department` navigační vlastnost. V *CourseController.cs*, nahraďte `Index` metoda následujícím kódem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

Metoda přijímá vybranou hodnotu rozevíracím seznamu v `SelectedDepartment` parametr. Pokud není nic vybráno, bude mít tento parametr hodnotu null.

A `SelectList` kolekce obsahující všechna oddělení byla předána do zobrazení pro rozevíracího seznamu. Parametry předaný `SelectList` konstruktor zadejte pole Název hodnoty, názvu pole text a vybranou položku.

Pro `Get` metodu `Course` úložiště, určuje kód výraz filtru, řazení a načítání pro nápovědy eager `Department` navigační vlastnost. Výraz filtru vždy vrátí `true` Pokud není nic vybráno v rozevíracím seznamu (tedy `SelectedDepartment` má hodnotu null).

V *Views\Course\Index.cshtml*, bezprostředně před otevření `table` značky, přidejte následující kód k vytvoření rozevíracího seznamu a tlačítko odeslání:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Se zarážkami spustit v `GenericRepository` spuštění kurzu indexovou stránku třídy. Pokračujte přes první dva časy, že kód dotkne zarážku, tak, aby stránka se zobrazí v prohlížeči. Vyberte oddělení z rozevíracího seznamu a klikněte na **filtru**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

První zarážky této doby bude pro dotaz oddělení pro rozevíracího seznamu. Přeskočit a zobrazit `query` proměnné příštím kód dosáhne zarážky Chcete-li zobrazit, co `Course` dotazu teď vypadá jako. Zobrazí se přibližně takto:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Uvidíte, že dotaz je teď `JOIN` dotaz, který načte `Department` data spolu s `Course` data a že obsahuje `WHERE` klauzule.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Práce s třídami Proxy

Rozhraní Entity Framework vytvoří instancí entit (například při spuštění dotazu), často vytvoří je jako instance dynamicky generovaném odvozený typ, který funguje jako proxy pro entitu. Tento proxy server přepíše některé virtuální vlastnosti entity, která má vložit háky pro provádění akcí automaticky při přístupu k vlastnosti. Například tento mechanismus se používá pro podporu opožděného načítání relací.

Ve většině případů nemusíte mít na paměti toto použití proxy, ale existují výjimky:

- V některých případech můžete chtít zabránit ve vytváření instancí proxy rozhraní Entity Framework. Například serializaci instancí proxy serveru může být efektivnější než serializaci instancí proxy serveru.
- Když vytváříte instance třídy entity pomocí `new` operátor, neobdržíte její instance. To znamená, že funkce není dostupná, jako je opožděného načítání a automatické sledování změn. Obvykle je to v pořádku; obecně nepotřebujete opožděného načítání, protože vytváříte nové entity, který není v databázi, a obecně nepotřebujete sledování, pokud jste explicitní označení entity jako změn `Added`. Ale pokud potřebujete opožděného načítání a potřebujete sledování změn, můžete vytvořit nové instance entity s proxy pomocí `Create` metodu `DbSet` – třída.
- Můžete chtít získat skutečný typ entity z typu proxy serveru. Můžete použít `GetObjectType` metodu `ObjectContext` třídy pro získání skutečný typ entity instance typu proxy serveru.

Další informace najdete v tématu [práce s proxy](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) na blog týmu rozhraní Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Zakázání automatické zjišťování změny

Rozhraní Entity Framework Určuje, jak došlo ke změně entity (a proto aktualizací, které potřebují k odeslání do databáze) tak, že porovnáte aktuální hodnoty entity s původními hodnotami. Původní hodnoty se uloží při entita byla dotazována nebo připojen. Některé metody, které způsobí změnu automatické zjišťování jsou následující:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Pokud sledujete velký počet entit a jednu z těchto metod zavoláte mnohokrát ve smyčce, můžete dosáhnout výrazné vylepšení výkonu dočasně vypnout pomocí zjišťování automatické změny [AutoDetectChangesEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) vlastnost. Další informace najdete v tématu [automaticky detekovat změny](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Zakázání ověřování při ukládání změny

Při volání `SaveChanges` metoda, ve výchozím nastavení rozhraní Entity Framework ověří data v všechny vlastnosti všech změněných entit před aktualizací databáze. Pokud jste aktualizovali velký počet entit a jste již zkontrolujete, data, tento pracovní není nutný, a můžete dokonce vytvářet proces ukládání změn trvat méně času můžete dočasně vypnout ověření. Můžete to, že pomocí [ValidateOnSaveEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) vlastnost. Další informace najdete v tématu [ověření](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Souhrn

Dokončení této série kurzů na používající rozhraní Entity Framework v aplikaci ASP.NET MVC. Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k dat ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

Další informace o tom, jak nasadit webovou aplikaci, až když jste sestavili najdete v tématu [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/en-us/library/bb386521.aspx) v knihovně MSDN.

Informace o dalších témat souvisejících s MVC, jako je například ověřování a autorizaci, najdete v článku [doporučené prostředky aplikace MVC](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Potvrzování

- Tní Dykstra napsali původní verze tohoto kurzu a je senior programovací zapisovač na webovou platformu společnosti Microsoft a nástrojů týmu obsahu.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) společně vytvořené v tomto kurzu a nebyla většinu práce aktualizace pro EF 5 a MVC 4. Rick je senior programovací zapisovač pro Microsoft zaměřením na Azure a MVC.
- [Rowan Lukeš](http://www.romiller.com) a ostatní členové týmu rozhraní Entity Framework asistované s recenze kódu a pomohl ladění mnohé problémy s migrací, které vznikly při byly aktualizujeme kurz pro EF 5.

## <a name="vb"></a>JAZYKA VISUAL BASIC

Při tomto kurzu bylo původně vytvořeno, jsme k dispozici jak C# a VB verzích dokončené stažení projektu. Pomocí této aktualizace poskytujeme projekt C# ke stažení pro každou kapitolu, aby bylo snazší začít pracovat kdekoli v řadě, ale z důvodu omezení čas a dalších priority není to pro VB. Pokud sestavení projektu jazyka Visual Basic, pomocí tyto kurzy a by chtěl který sdílet s ostatními, dejte nám vědět.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Chyby a řešení

### <a name="cannot-createshadow-copy"></a>Nelze vytvořit nebo stínové kopie

Chybová zpráva:

*Nelze vytvořit nebo stínové kopie 'DotNetOpenAuth.OpenId' Tento soubor již existuje.*

Řešení:

Počkejte několik sekund a aktualizujte stránku.

### <a name="update-database-not-recognized"></a>Update-Database nebyl rozpoznán

Chybová zpráva:

*Termín 'Update-Database' nebyl rozpoznán jako název rutiny, funkce, soubor skriptu nebo spustitelného programu. Zkontrolujte, zda název, nebo pokud byl zahrnut cestu, ověřte, zda je cesta správná a zkuste to znovu.* (Z  *`Update-Database`*  v pomocí PMC.)

Řešení:

Ukončete aplikaci Visual Studio. Otevřete projekt a zkuste to znovu.

### <a name="validation-failed"></a>Ověření se nezdařilo

Chybová zpráva:

*Ověření se nezdařilo pro jednu nebo více entit. Naleznete ve vlastnosti 'EntityValidationErrors' Další podrobnosti.* (Z  *`Update-Database`*  v pomocí PMC.)

Řešení:

Jednou z příčin tohoto problému je chyby ověření při `Seed` metoda spustí. V tématu [první financování a ladění Entity Framework (EF) databází](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) tipy k ladění `Seed` metoda.

### <a name="http-50019-error"></a>HTTP 500.19 chyby

Chybová zpráva:

*Chyba protokolu HTTP 500.19 – vnitřní chyba serveru k požadované stránce nelze přistupovat, protože související konfigurační data pro stránku nejsou platná.*

Řešení:

Jedním ze způsobů, může se tato chyba je z s více kopií řešení, každý z nich používá stejné číslo portu. Obvykle můžete tento problém vyřešit ukončení všech instancí sady Visual Studio a poté restartování pracujete v projektu. Pokud to nefunguje, zkuste změnit číslo portu. Klikněte pravým tlačítkem na soubor projektu a pak klikněte na položku Vlastnosti. Vyberte **webové** kartě a poté změňte číslo portu v **adresa Url projektu** textové pole.

### <a name="error-locating-sql-server-instance"></a>Chyba vyhledáním instance systému SQL Server

Chybová zpráva:

*Při navazování připojení k systému SQL Server došlo k chybě související se sítí nebo konkrétní instancí. Server nebyl nalezen nebo není přístupný. Ověřte, zda je název instance správný a zda systém SQL Server je nakonfigurován k povolení vzdálených připojení. (Zprostředkovatel: rozhraní sítě systému SQL, chyba: 26 - chyba vyhledání Server nebo instanci zadáno)*

Řešení:

Zkontrolujte připojovací řetězec. Pokud jste odstranili ručně databáze, změňte název databáze v řetězci konstrukce.

>[!div class="step-by-step"]
[Předchozí](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
[další](building-the-ef5-mvc4-chapter-downloads.md)
