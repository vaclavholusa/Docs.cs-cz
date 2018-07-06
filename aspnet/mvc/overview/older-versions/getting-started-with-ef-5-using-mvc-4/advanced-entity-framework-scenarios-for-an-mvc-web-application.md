---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Pokročilé scénáře pro Entity Framework pro webové aplikace MVC (10 z 10) | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje postup vytvoření aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a sady Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5a75d85140a40660314ab267fdd74a8058d791fc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832672"
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Scénáře pro pokročilé Entity Framework pro webové aplikace MVC (10 z 10)
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a Visual Studio 2012. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Série kurzů můžete začít od začátku nebo [stáhnout počáteční projekt pro tuto kapitolu](building-the-ef5-mvc4-chapter-downloads.md) a začněte tady.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte sami, [stáhnout dokončený kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a zkuste problém reprodukovat. Porovnáním kód Dokončený kód v obecně najdete řešení problému. Některé běžné chyby a jejich řešení najdete v tématu [chyby a náhradní řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V předchozím kurzu jste implementovali, úložiště a jednotky pracovních vzorů. Tento kurz se zabývá v následujících tématech:

- Provádění nezpracované dotazy SQL.
- Provádění dotazů bez sledování.
- Zkoumání dotazy se odesílají do databáze.
- Práce s třídami proxy serveru.
- Zakázání automatického zjišťování změn.
- Zakázání ověření při ukládání změn.
- [Chyby a pracovní postupy](#errors)

Pro většinu z těchto témat budete pracovat stránky, které jste už vytvořili. Použití nezpracovaných SQL hromadné aktualizace vytvoříte novou stránku, která aktualizuje počet kredity všechny kurzy v databázi:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

A použít dotaz bez sledování přidáte nový logiku ověřování k oddělení upravit stránku:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Provádí nezpracované dotazy SQL

Prvního rozhraní API sady Entity Framework kód obsahuje metody, které vám umožní předat příkazů SQL přímo do databáze. Máte následující možnosti:

- Použití `DbSet.SqlQuery` metodu pro dotazy vracející typy entit. Vrácených objektů musí být typu očekávání tím, `DbSet` objektu a automaticky sleduje provedené kontext databáze není-li vypnout sledování. (Viz následující část o `AsNoTracking` metoda.)
- Použití `Database.SqlQuery` metodu pro dotazy vracející typy, které nejsou entity. Vrácená data není sledována kontext databáze, i když používáte tuto metodu pro načtení typů entit.
- Použití [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) pro příkazy bez dotazů.

Jednou z výhod používající nástroj Entity Framework je, že se eliminuje propojí váš kód příliš úzce na konkrétní metodu ukládat data. Dělá to tak, že generování dotazy SQL a příkazy, které také díky které by bylo nutné napsat sami. Ale existují výjimečné situace, kdy je potřeba spustit konkrétní dotazy SQL, které ručně vytvoříte, a tyto metody umožňuje zpracovávat takové výjimky.

Jak platí vždy při spuštění příkazů SQL ve webové aplikaci, je nutné provést opatření k ochraně před útoky prostřednictvím injektáže SQL serveru. Jednou z možností, které se pomocí parametrizovaných dotazů se ujistěte, že řetězce odeslané na webové stránce se nedá interpretovat jako příkazy SQL. V tomto kurzu použijete parametrizovaných dotazů při integraci uživatelský vstup do dotazu.

### <a name="calling-a-query-that-returns-entities"></a>Volání dotazu, který vrací entity

Předpokládejme, že chcete, aby `GenericRepository` třídě poskytnout další filtrování a řazení flexibilitu bez nutnosti vytvoření odvozené třídy s další metody. Přidejte metodu, která přijme dotaz SQL je jeden způsob, jak toho dosáhnout. Můžete nastavit jakýkoli druh filtrováním nebo řazením je vhodné v kontroleru, například `Where` klauzuli, která závisí spojení ani poddotaz. V této části uvidíte, jak implementovat metody.

Vytvořte `GetWithRawSql` metoda přidáním následujícího kódu *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

V *CourseController.cs*, volejte metodu z nové `Details` způsob, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

V tomto případě jste mohli použít `GetByID` metody, ale používáte `GetWithRawSql` metodu k ověření, který `GetWithRawSQL` metoda funguje.

Spuštění stránky podrobností a ověřte, že funguje dotaz select (vybrat **kurzu** kartu a potom **podrobnosti** pro jeden kurz).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Volání dotazu, který vrátí další typy objektů

Dříve jste vytvořili mřížky student statistiky o stránky, které jsme si ukázali, počet studentů pro každé datum registrace. Kód, který to dělá v *HomeController.cs* používá technologii LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Předpokládejme, že chcete napsat kód, který načte tato data přímo v SQL a ne pomocí jazyka LINQ. Provedete tak, že budete muset spustit dotaz, který vrací jinou hodnotu než objekty entity, což znamená, že musíte použít `Database.SqlQuery` metody.

V *HomeController.cs*, nahraďte příkazem LINQ v `About` metodu s následujícím kódem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Na stránce o spuštění. Zobrazí se stejná data, která předtím.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Volání dotaz Update

Předpokládejme, že správce společnosti Contoso University chtějí mít možnost k provádění hromadných změn v databázi, jako je například změna číslo kredity pro každý kurz. Pokud univerzity má velký počet kurzů, bylo by neefektivní načíst vše jako entity a měnit je jednotlivě. V této části budete implementovat webovou stránku, která umožňuje uživateli zadat faktor, podle kterého chcete změnit počet kredity pro všechny kurzy a provede změny pomocí provádí SQL `UPDATE` příkazu. Webové stránky bude vypadat jako na následujícím obrázku:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

V předchozím kurzu jste použili obecného úložiště číst a aktualizovat `Course` entity v `Course` kontroleru. Pro tuto operaci hromadně aktualizace budete muset vytvořit nové úložiště metodu, která není v obecné úložiště. K tomuto účelu vytvoříte vyhrazené `CourseRepository` třídu odvozenou od `GenericRepository` třídy.

V *DAL* složku, vytvořte *CourseRepository.cs* a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

V *UnitOfWork.cs*, změnit `Course` typ úložiště z `GenericRepository<Course>` do `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

V *CourseContoller.cs*, přidejte `UpdateCourseCredits` metody:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Tato metoda se používá pro obě `HttpGet` a `HttpPost`. Když `HttpGet` `UpdateCourseCredits` metoda spuštění `multiplier` proměnná bude mít hodnotu null a zobrazení se zobrazí prázdné textové pole a tlačítko pro odeslání, jak je znázorněno na předchozím obrázku.

Když **aktualizace** po kliknutí na tlačítko a `HttpPost` metoda spuštění `multiplier` budou mít hodnoty zadané v textovém poli. Kód poté volá úložiště `UpdateCourseCredits` metodu, která vrátí počet ovlivněných řádků a tato hodnota je uložena v `ViewBag` objektu. Když obdrží počet ovlivněných řádků v zobrazení `ViewBag` objektu, zobrazí toto číslo místo do textového pole a tlačítko, odeslat, jak je znázorněno na následujícím obrázku:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Vytvoření zobrazení v *Views\Course* složky kredity kurzu aktualizace stránky:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

V *Views\Course\UpdateCourseCredits.cshtml*, nahraďte kód šablony následujícím kódem:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Spustit `UpdateCourseCredits` metodu tak, že vyberete **kurzy** kartu a následným přidáním "/ UpdateCourseCredits" na konci adresy URL v adresním řádku prohlížeče (například: `http://localhost:50205/Course/UpdateCourseCredits`). Zadejte číslo v textovém poli:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Klikněte na tlačítko **aktualizace**. Zobrazí počet ovlivněných řádků:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Klikněte na tlačítko **zpět do seznamu** zobrazíte seznam kurzů s revidované množství kreditu, které.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Další informace o nezpracované dotazy SQL najdete v tématu [nezpracované dotazy SQL](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) na blogu týmu Entity Framework.

## <a name="no-tracking-queries"></a>Sledování bez dotazy

Když kontext databáze načte řádky databáze, vytvoří objekty entity, které představují je ve výchozím nastavení se uchovává informace o zda entity v paměti jsou synchronizované s tím, co je v databázi. Data v paměti funguje jako mezipaměť a používá se při aktualizaci entity. Tento ukládání do mezipaměti je často zbytečné ve webové aplikaci, protože kontextu instance jsou krátkodobé a jednorázové obvykle (nový je vytvořen a uvolnění pro každý požadavek) a kontextu, která načte entity je obvykle uvolněn předtím, než je znovu použít danou entitu.

Můžete určit, zda kontext sleduje objekty entity pro dotaz s použitím `AsNoTracking` metody. Typické scénáře, ve kterých můžete chtít provést, patří:

- Dotaz načte velké objemy dat, která vypnutí sledování může výrazně zvýšit výkon.
- Chcete připojit entitu, aby bylo možné ji aktualizovat, ale jste dříve získali stejné entity k jinému účelu. Vzhledem k tomu, že entita již sledován správou kontext databáze, nelze připojit entitu, kterou chcete změnit. Jedním ze způsobů k tomu nedocházelo, je použít `AsNoTracking` možnost s předchozí dotaz.

V této části budete implementovat obchodní logiku, která znázorňuje druhý z těchto scénářů. Konkrétně budete vynucovat obchodní pravidlo s upozorněním, že instruktorem nemůže být správce více než jednom oddělení.

V *DepartmentController.cs*, přidejte novou metodu, kterou lze volat z `Edit` a `Create` metody, abyste měli jistotu, že žádné dvě oddělení mají stejné správce:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Přidejte kód `try` bloku `HttpPost` `Edit` metoda k volání této nové metody, pokud nejsou žádné chyby ověření. `try` Bloku teď vypadá jako v následujícím příkladu:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Spusťte oddělení upravit stránku a pokuste se změnit správce oddělení instruktorem, který je již správce jinému oddělení. Získáte očekávané chybová zpráva:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Nyní spusťte oddělení upravit stránku znovu a tuto změnu času **rozpočtu** částku. Po kliknutí na **Uložit**, se zobrazí chybová stránka:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Chybová zpráva výjimky je "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" k tomu dojít z důvodu následující posloupnost událostí:

- `Edit` Volání metod `ValidateOneAdministratorAssignmentPerInstructor` metodu, která načte všechna oddělení, které mají Jan Novak jako jejich správce. Který způsobí, že anglické oddělení čtení. Protože se jedná o oddělení, který právě upravujete, žádné chybě. V důsledku tuto operaci čtení ale angličtině oddělení entity, která byla načtena z databáze teď sledován správou kontext databáze.
- `Edit` Metoda se pokusí nastavit `Modified` příznak na Angličtina oddělení entity vytvořené vazač modelu MVC, ale který selže, protože kontext již sleduje entity pro anglickou oddělení.

Jedním řešením tohoto problému je zabránit sledování v paměti oddělení entity načíst pomocí dotazu ověření kontextu. Neexistuje žádné nevýhodou na to, protože nebude aktualizovat tuto entitu nebo čtení znovu tak, aby je výhodná je uložená v mezipaměti.

V *DepartmentController.cs*v `ValidateOneAdministratorAssignmentPerInstructor` metodě zadat bez sledování, jak je znázorněno v následujícím:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Opakovat pokus upravit **rozpočtu** množství oddělení. Tentokrát je operace úspěšná a webu vrátí podle očekávání oddělení indexovou stránku, hodnotu revidovaného rozpočtu.

## <a name="examining-queries-sent-to-the-database"></a>Zkoumání odeslání dotazů do databáze

Někdy je vhodné se může zobrazit skutečné dotazy SQL, které se odesílají do databáze. K tomuto účelu můžete prozkoumat proměnné dotazu v ladicím programu nebo volat v dotazu `ToString` metody. Pokud to pokud chcete vyzkoušet si, budete podívejte se na jednoduchý dotaz a podívejte se na co se stane do ní, jak přidat možnosti takové eager načítání, filtrování a řazení.

V *řadiče/CourseController*, nahraďte `Index` metodu s následujícím kódem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Nyní nastavte zarážku v *GenericRepository.cs* na `return query.ToList();` a `return orderBy(query).ToList();` prohlášení o `Get` metody. Spusťte projekt v režimu ladění a vyberte kurzu indexovou stránku. Pokud kód dosáhne zarážky, podívejte se `query` proměnné. Zobrazí se dotaz, který je odeslána do systému SQL Server. Je jednoduchý `Select` – příkaz:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Dotazy může být příliš dlouhý pro zobrazení v oknech ladění v sadě Visual Studio. Pokud chcete zobrazit celý dotaz, můžete zkopírovat hodnotu proměnné a jeho vložením do textového editoru:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Nyní přidáte rozevíracího seznamu na kurz indexovou stránku tak, aby uživatelé mohou filtrovat pro konkrétní oddělení. Kurzy vám seřadit podle názvu, a specifikujete, nemůžou dočkat, až načítání pro `Department` navigační vlastnost. V *CourseController.cs*, nahraďte `Index` metodu s následujícím kódem:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

Metoda přijímá vybranou hodnotu v rozevíracím seznamu v `SelectedDepartment` parametru. Pokud není nic vybráno, bude mít tento parametr hodnotu null.

A `SelectList` kolekce obsahující všechna oddělení je předána do zobrazení pro rozevíracího seznamu. Parametry předávané `SelectList` konstruktor zadejte název hodnoty pole, název textového pole a vybrané položky.

Pro `Get` metodu `Course` úložiště, kód určuje výraz filtru, pořadí řazení a eager načítání pro `Department` navigační vlastnost. Výraz filtru vždy vrátí `true` Pokud není proveden žádný výběr v rozevíracím seznamu (to znamená, `SelectedDepartment` má hodnotu null).

V *Views\Course\Index.cshtml*, bezprostředně před zahájením `table` značky, přidejte následující kód k vytvoření rozevíracího seznamu a tlačítka Odeslat:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Se zarážkami přesto bude nastavené `GenericRepository` spuštění kurzu indexu stránky třídy. Pokračujte až do doby první dva, že kód narazí na zarážku, tak, aby stránka se zobrazí v prohlížeči. Vyberte oddělení z rozevíracího seznamu a klikněte na tlačítko **filtr**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Tentokrát pro oddělení dotaz pro rozevírací seznam bude k první zarážce. Přeskočit a zobrazit `query` proměnné při příštím kód dosáhne zarážky Chcete-li zobrazit, co `Course` dotazu teď vypadá podobně jako. Zobrazí se vám něco jako toto:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Uvidíte, že dotaz je teď `JOIN` dotaz, který načte `Department` dat spolu s `Course` dat a že obsahuje `WHERE` klauzuli.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Práce s třídami Proxy

Když rozhraní Entity Framework vytváří instance entity (například při spuštění dotazu), často vytvoří jako instance dynamicky generované odvozeného typu, který funguje jako proxy pro entitu. Tento proxy server přepíše některé virtuální vlastnosti entity, chcete-li vložit háky pro provádění akcí automaticky při přístupu k vlastnosti. Tento mechanismus je například použít pro podporu opožděné načtení vztahů.

Ve většině případů není nutné mít na paměti toto použití proxy, ale existují výjimky:

- V některých případech můžete chtít zabránit ve vytváření instancí proxy Entity Framework. Například serializaci instancí – proxy server může být efektivnější než serializaci instancí proxy serveru.
- Při vytváření instance třídy entity pomocí `new` operátoru, nebudete mít instanci proxy serveru. To znamená, že se že vám funkce, jako je opožděné načtení a automatické sledování změn. Obvykle je to v pořádku; obvykle není nutné opožděné načtení, vzhledem k tomu, že vytváříte novou entitu, která není v databázi, a obvykle není nutné řešení change tracking, pokud jste explicitní označení entity jako `Added`. Ale pokud potřebujete opožděné načtení a potřebujete řešení change tracking, můžete vytvořit nové instance entity s použitím proxy servery `Create` metodu `DbSet` třídy.
- Můžete chtít získat typ skutečné entity z typ proxy serveru. Můžete použít `GetObjectType` metodu `ObjectContext` třídy pro získání skutečné entity typu instance typu proxy serveru.

Další informace najdete v tématu [práce s proxy](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) na blogu týmu Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Zakázání automatického zjišťování změn

Entity Framework Určuje, jak se entity změnil (a proto aktualizací musí být odeslán do databáze) srovnáním aktuální entity s původními hodnotami. Původní hodnoty jsou uloženy při entity se dotazovat nebo připojen. Některé metody, které způsobí automatickou změnu zjišťování jsou následující:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Pokud sledujete velké množství entit a volání jedné z těchto metod v mnoha případech ve smyčce, můžete dosáhnout výrazné zlepšení výkonu dočasně vypnout automatickou změnu pomocí zjišťování [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) vlastnost. Další informace najdete v tématu [automaticky Detekovaly změny](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Zakázání ověření při ukládání změny

Při volání `SaveChanges` metody, ve výchozím nastavení rozhraní Entity Framework ověří data ve všech vlastností veškeré entitám před aktualizací databáze. Pokud jste aktualizovali velké množství entit a jste už ověřili dat, není nutné tuto práci a můžete provést proces ukládání pak změny budou méně času tak, že dočasně vypnete ověřování. Můžete provést, že při použití [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) vlastnost. Další informace najdete v tématu [ověření](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Souhrn

Dokončení tohoto postupu Tato série kurzů týkající se použití rozhraní Entity Framework v aplikaci ASP.NET MVC. Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k Data technologie ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

Další informace o tom, jak nasadit webové aplikace po začlenění ho najdete v tématu [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx) v knihovně MSDN.

Informace o dalších témat souvisejících s MVC, jako je například ověřování a autorizaci, najdete v článku [doporučené prostředky aplikace MVC](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Potvrzení

- Petr Dykstra napsal původní verzi tohoto kurzu a je vedoucí pro programování zápis na webovou platformu společnosti Microsoft a týmu obsahu nástroje.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) spoluautorem v tomto kurzu a nebyla většinu práce aktualizuje pro EF 5 a MVC 4. Rick je vedoucí pro programování zápis pro společnost Microsoft, zaměřuje se na Azure a MVC.
- [Rowan Miller](http://www.romiller.com) a ostatní členové týmu, Entity Framework s asistencí s revizemi kódu a pomohly ladění mnoho problémů s migrace, které vznikly během kurzu jsme byly aktualizace pro EF 5.

## <a name="vb"></a>VB

Při tomto kurzu byla původně vytvořil, jsme poskytli jazyce C# a VB verzích aplikace project stahování dokončeno. V této aktualizaci jsme tím projektu jazyka C# ke stažení jednotlivých kapitol, aby bylo snazší Začínáme kdekoli v této sérii, ale z důvodu omezení času a jiné priority, jsme neprovedli, který pro Visual Basic Pokud se sestavení projektu jazyka Visual Basic pomocí těchto kurzů a by chtěl, sdílet s ostatními, dejte nám prosím vědět.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Chyby a řešení

### <a name="cannot-createshadow-copy"></a>Nelze vytvořit/stínové kopie

Chybová zpráva:

*Nelze vytvořit nebo stínovou kopii "DotNetOpenAuth.OpenId", který již existuje.*

Řešení:

Počkejte několik sekund a aktualizujte stránku.

### <a name="update-database-not-recognized"></a>Aktualizace databáze nebyl rozpoznán

Chybová zpráva:

*Termín 'Aktualizace databáze' nebyl rozpoznán jako název rutiny, funkce, soubor skriptu nebo spustitelného programu. Zkontrolujte, zda název, nebo pokud cesty byl zahrnut, ověřte správnost cesty a zkuste to znovu.* (Z *`Update-Database`* příkazu v konzole PMC.)

Řešení:

Ukončení sady Visual Studio. Znovu otevřít projekt a zkuste to znovu.

### <a name="validation-failed"></a>Ověření se nezdařilo

Chybová zpráva:

*Ověření se nezdařilo pro jednu nebo více entit. Viz vlastnost 'EntityValidationErrors' Další podrobnosti.* (Z *`Update-Database`* příkazu v konzole PMC.)

Řešení:

Jeden příčinou tohoto problému je chyby ověřování při `Seed` metoda spuštění. Naleznete v tématu [financování a ladění Entity Framework (EF) databází](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) tipy pro ladění `Seed` metody.

### <a name="http-50019-error"></a>HTTP Error 500.19 chyba

Chybová zpráva:

*Chyba protokolu HTTP 500.19 – vnitřní chyba serveru požadovanou stránku nelze získat přístup, protože související konfigurační data pro stránky je neplatná.*

Řešení:

Jedním ze způsobů, které se zobrazí tato chyba je nemusíte několik kopií řešení, každý z nich používat stejné číslo portu. Obvykle můžete tento problém vyřešit ukončením všechny instance sady Visual Studio a poté restartuje vaše práce na projektu. Pokud to nefunguje, zkuste změnit číslo portu. Klikněte pravým tlačítkem na soubor projektu a pak klikněte na vlastnosti. Vyberte **webové** kartu a potom změňte číslo portu **adresa Url projektu** textového pole.

### <a name="error-locating-sql-server-instance"></a>Chyba vyhledáním instanci systému SQL Server

Chybová zpráva:

*Při navazování připojení k serveru SQL Server došlo k chybě související se sítí nebo s instancí. Server nebyl nalezen nebo nebyl přístupný. Ověřte, zda je název instance správný a, SQL Server je nakonfigurován pro povolit vzdálená připojení. (poskytovatel: rozhraní sítě systému SQL, chyba: 26 - Chyba vyhledávání serveru či Instance zadáno)*

Řešení:

Zkontrolujte připojovací řetězec. Pokud jste ručně odstranit databázi, změňte název databáze v řetězci konstrukce.

> [!div class="step-by-step"]
> [Předchozí](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [další](building-the-ef5-mvc4-chapter-downloads.md)
