---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: "Implementace úložiště a jednotky pracovních vzorů v aplikaci ASP.NET MVC (9, 10) | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c920dc8defe18b6f27d122c2cd1a6c6ffdaad608
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementace úložiště a jednotky pracovních vzorů v aplikaci ASP.NET MVC (9, 10)
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio 2012. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Kurz řady můžete začít od začátku nebo [stáhnout počáteční projekt pro tato kapitola](building-the-ef5-mvc4-chapter-downloads.md) a začněte zde.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte, [stáhnout dokončené kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a pokuste se znovu provést váš problém. Řešení problému můžete získat obecně tak, že porovnáte svůj kód Dokončený kód. Pro některé běžné chyby a jak je vyřešit, najdete v části [chyby a řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V předchozích kurzu používá dědičnosti ke snížení redundantní kód `Student` a `Instructor` tříd entit. V tomto kurzu se zobrazí některé způsoby použití úložiště a jednotky pracovních vzorů pro operace CRUD. Jako v předchozích kurzu budete v tato změnit způsob kódu pracuje s stránky jste již vytvořili nikoli vytvářet nové stránky.

## <a name="the-repository-and-unit-of-work-patterns"></a>Úložiště a jednotky pracovních vzorů

Úložiště a jednotky pracovních vzorů jsou určeny k vytvoření vrstvy abstrakce mezi vrstva přístupu k datům a vrstvu obchodní logiky aplikace. Implementace tyto vzory pomůžou izolovat aplikace od změny v úložišti dat a mohou usnadnit automatizované testování částí nebo testy řízený vývoj (TDD).

V tomto kurzu budete implementovat třídu úložiště ke každému typu entity. Pro `Student` vytvoříte úložiště rozhraní a třídy úložiště typu entity. Pokud instanci můžete vytvořit úložiště do kontroleru, použijete rozhraní tak, aby řadičem bude přijímat odkaz na libovolný objekt, který implementuje rozhraní úložiště. Pokud je řadič běží pod webový server, obdrží úložiště, který funguje s rozhraní Entity Framework. Pokud je řadič běží pod třídu test jednotky, obdrží úložiště, který funguje s data uložená způsobem, který můžete snadno upravit pro testování, jako je například kolekci v paměti.

Později v tomto kurzu budete používat více úložiště a jednotky pracovních třídu pro `Course` a `Department` typy entit, které do `Course` řadiče. Třída pracovní jednotky koordinuje pracovní více úložišť vytvořením třídy kontextu jedné databáze sdílejí všechny z nich. Pokud chcete být schopni provést automatizované testování částí, by vytvořit a používat rozhraní pro tyto třídy stejným způsobem, jako jste to udělali pro `Student` úložiště. K zjednodušení tento kurz, budete však vytvořit a použít tyto třídy bez rozhraní.

Následující obrázek ukazuje jeden ze způsobů conceptualize vztahy mezi kontroleru a kontextu třídy ve srovnání s nepoužíváte úložiště nebo jednotka práci vzor vůbec.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Tento kurz série nebude vytvářet testy částí. Úvod do TDD s aplikace MVC, která používá vzor úložiště, najdete v části [návod: použití TDD s architekturou ASP.NET MVC](https://msdn.microsoft.com/en-us/library/ff847525.aspx). Další informace o vzoru úložiště najdete v následujících zdrojích informací:

- [Vzor úložiště](https://msdn.microsoft.com/en-us/library/ff649690.aspx) na webu MSDN.
- [Úložiště a jednotky pracovních vzorů pomocí Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) na blog týmu rozhraní Entity Framework.
- [Agilní Entity Framework 4 úložiště](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) řadu příspěvcích na blogu Julie Lerman.
- [Vytváření účtu v aplikaci na první pohled HTML5 nebo jQuery](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) na blogu Dana Wahlin.

> [!NOTE]
> Implementace úložiště a jednotky pracovních vzorů mnoha způsoby. Třídy úložiště můžete použít s nebo bez jednotku pracovní třídy. Můžete implementovat jednu úložiště pro všechny typy entit a jeden pro každý typ. Pokud budete implementovat jednu pro každý typ, můžete použít samostatné třídy, obecná základní třída a odvozené třídy, nebo abstraktní základní třída a odvozené třídy. Můžete zahrnout obchodní logiku v úložišti nebo omezit na data přístup logiku. Můžete také vytvořit abstraktní vrstvu do vaší třídy kontextu databáze pomocí [IDbSet](https://msdn.microsoft.com/en-us/library/gg679233(v=vs.103).aspx) rozhraní existuje místo [DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=vs.103).aspx) typy pro sady entit. Přístup k implementaci abstraktní vrstvu uvedené v tomto kurzu je jednou z možností je třeba zvážit, není doporučení pro všechny scénáře a různá prostředí.


## <a name="creating-the-student-repository-class"></a>Vytvoření třídy úložiště studenty

V *DAL* složky, vytvořte soubor třídy s názvem *IStudentRepository.cs* a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Tento kód deklaruje typickou sadu CRUD metody, včetně způsobů čtení – ten, který vrátí všechny `Student` entity a ten, který vyhledá jedné `Student` entitu podle ID.

V *DAL* složky, vytvořte soubor třídy s názvem *StudentRepository.cs* souboru. Nahraďte stávající kód následující kód, který implementuje `IStudentRepository` rozhraní:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Kontext databáze je definována v proměnné třídy a konstruktoru očekává volající objekt předávat instance kontextu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Může vytvořit instanci nový kontext v úložišti, ale pak pokud jste použili více úložišť v jednom řadiči, každý by binárními samostatný kontext. Později budete používat v několika úložiště `Course` řadiče a uvidíte, jak jednotku pracovní třídy můžete zajistit, že všechny úložiště používají stejný kontext.

Implementuje úložiště [IDisposable](https://msdn.microsoft.com/en-us/library/system.idisposable.aspx) a zruší kontext databáze, protože jste viděli dříve v kontroleru a její metody CRUD volat v kontextu databáze v stejným způsobem, který jste předtím viděli.

## <a name="change-the-student-controller-to-use-the-repository"></a>Změna řadiče Student používat úložiště

V *StudentController.cs*, nahraďte kód aktuálně ve třídě, s následujícím kódem. Změny se zvýrazněnou.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Řadičem teď deklaruje proměnné třídy pro objekt, který implementuje `IStudentRepository` rozhraní namísto třídy kontextu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Výchozí konstruktor (bez parametrů) vytvoří novou instanci kontextu, a volitelné konstruktor umožňuje volajícímu předat instance kontextu.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Pokud jste používali *vkládání závislostí*, nebo DI, nebude musíte výchozí konstruktor protože DI softwaru by zajistit, aby objekt správné úložiště by vždy.)

V metodách CRUD úložiště se nyní označuje jako místo kontextu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

A `Dispose` metoda teď uvolní místo kontext úložiště:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Spuštění tohoto webu a klikněte na **studenty** kartě.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

Na stránce vypadá a funguje stejně jako předtím, než jste změnili kódu pro použití úložiště a dalších stránek Student také fungovat stejně. Existuje však důležitý rozdíl ve způsobu, jakým `Index` metoda řadiče nemá filtrování a řazení. Původní verze této metody obsahovala následující kód:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Aktualizovaný `Index` metoda obsahuje následující kód:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Došlo ke změně pouze zvýrazněný kód.

V původní verzi kódu `students` je zadán jako `IQueryable` objektu. Dotaz není odeslal do databáze, dokud je převeden do kolekce pomocí metody, jako třeba `ToList`, který nedojde, dokud zobrazení indexu přistupuje k student modelu. `Where` Stane metoda ve výše uvedeném původní kódu `WHERE` klauzule v dotazu SQL, která je odeslána do databáze. Naopak to znamená, že jenom vybrané entity jsou vráceny v databázi. Ale v důsledku změna `context.Students` k `studentRepository.GetStudents()`, `students` proměnná po tento příkaz `IEnumerable` kolekci, která zahrnuje všechny studenty v databázi. Konečný výsledek použití `Where` metoda je stejný, ale teď práci v paměti na webovém serveru a ne serverem databáze. Pro dotazy, které vrací velké objemy dat může to být neefektivní.

> [!TIP] 
> 
> **IQueryable vs. Rozhraní IEnumerable**
> 
> Po implementaci úložiště jak je vidět tady, i v případě, že zadáte v něco **vyhledávání** pole dotazu na SQL Server vrátí všechny řádky studenty, protože neobsahuje kritéria vyhledávání:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Tento dotaz vrátí všechny student dat, protože úložiště provést dotaz, aniž by věděly, o kritériích vyhledávání. Proces řazení, použití kritéria hledání a výběrem podmnožiny dat pro stránkování (v tomto případě zobrazuje jenom 3 řádky) se provádí v paměti novější při `ToPagedList` metoda je volána na `IEnumerable` kolekce.
> 
> V předchozí verzi kód (před implementací úložišti), dotaz neposílají se do databáze až po instalaci kritéria hledání, když `ToPagedList` se volá na `IQueryable` objektu.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Když je volána ToPagedList na `IQueryable` objektu dotaz odeslána do systému SQL Server určuje řetězec pro hledání a v důsledku se vrátí pouze řádky, které splňují kritéria hledání a žádné filtrování je třeba provést v paměti.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (Následující kurz vysvětluje, jak prozkoumat dotazů odesílaných do systému SQL Server.)


V následující části ukazuje, jak implementovat úložiště metody, které vám umožní určit, že tento pracovní by mělo být provedeno v databázi.

Nyní jste vytvořili abstraktní vrstvu mezi kontrolerem a kontext databáze Entity Framework. Pokud se chystáte provést automatizované testování částí pomocí této aplikace, můžete vytvořit třídu alternativní úložiště v projektu testů jednotek, který implementuje `IStudentRepository` *.* Namísto volání kontextu k čtení a zápis dat, může tato třída imitované úložiště upravit kolekce v paměti účely otestování funkce řadiče.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementovat obecné úložiště a jednotky pracovních třídy

Vytvoření třídy úložiště pro každý typ entity může vést k velkému redundantní kód, a to může způsobit částečné aktualizace. Předpokládejme například, že budete muset aktualizovat dva typy různých entit v rámci stejné transakci. Pokud každá používá instanci kontextu samostatné databáze, jeden může proběhnout úspěšně a dalších může selhat. Jedním ze způsobů, chcete-li minimalizovat redundantní kód je použít obecné úložiště a jeden způsob, jak zajistit, aby všechny úložiště používají stejný kontext databáze (a tedy koordinovat všechny aktualizace), je použít jednotku pracovní třídy.

V této části kurzu vytvoříte `GenericRepository` – třída a `UnitOfWork` třídy a použít je v `Course` řadiče pro přístup k i `Department` a `Course` sady entit. Jak je popsáno výše, k zjednodušení tuto část kurzu, nejsou vytváření rozhraní pro tyto třídy. Pokud se chystáte použít je k usnadnění TDD, by obvykle jejich implementací s rozhraními stejně jako jste to udělali, ale `Student` úložiště.

### <a name="create-a-generic-repository"></a>Vytvořit obecné úložiště

V *DAL* složku vytvořit *GenericRepository.cs* a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Třída proměnné jsou deklarovány pro kontext databáze a sadě entit, které pro vytvoření instance úložiště:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Konstruktor přijímá instanci kontext databáze a inicializuje nastavená proměnná entity:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get` Metoda na základě výrazy lambda umožní volací kód, který zadejte podmínku filtrování a sloupec pro řazení výsledků podle a parametr řetězce umožňují volající Zadejte čárkami oddělený seznam navigačních vlastností pro přes načítání:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Kód `Expression<Func<TEntity, bool>> filter` znamená volající zajistí výrazu lambda na základě `TEntity` typ a tento výraz bude vrátit logickou hodnotu. Například, pokud úložiště je vytvořena instance pro `Student` typ entity, může zadat kód v metodě volání `student => student.LastName == "Smith` &quot; pro `filter` parametr.

Kód `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` také znamená volající zajistí výrazu lambda. Ale v takovém případě je vstupní výraz `IQueryable` objekt pro `TEntity` typu. Výraz vrátí seřazený verze aplikace, která `IQueryable` objektu. Například, pokud úložiště je vytvořena instance pro `Student` typ entity, může zadat kód v metodě volání `q => q.OrderBy(s => s.LastName)` pro `orderBy` parametr.

Kód `Get` metoda vytvoří `IQueryable` objektu a poté použije výraz filtru, pokud existuje:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Dále platí výrazy eager načítání po analýze seznamu odděleného čárkami:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Nakonec se vztahuje `orderBy` výrazu, existuje-li a vrátí výsledky; v opačném případě vrátí výsledky z neuspořádaný dotazu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Při volání `Get` metodu, můžete to udělat filtrování a řazení v `IEnumerable` kolekce vráceném metodou místo parametry pro tyto funkce. Ale řazení a filtrování pracovních pak provádějí v paměti na webovém serveru. Pomocí těchto parametrů, je zajistit, že práci databázi spíše než webový server. Další možností je vytvořit odvozených tříd pro konkrétní entitu typy a přidejte specializované `Get` metody, jako například `GetStudentsInNameOrder` nebo `GetStudentsByName`. Ale komplexní aplikace, to může způsobit velké množství takových odvozené třídy a specializované metody, které může být více práce k údržbě.

Kód `GetByID`, `Insert`, a `Update` metody je podobná viděli v úložišti neobecnou. (Neposkytují parametrem přes načítání v `GetByID` podpis, protože nemůžete udělat přes načítání s `Find` metoda.)

Pro jsou k dispozici dva přetížení `Delete` metoda:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Jeden z těchto umožňuje předat v právě ID entity, která má být odstraněn, a má entity instance. Jak už jste viděli [zpracování souběžnosti](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) kurz pro souběžnosti zpracování budete potřebovat `Delete` metody, která přijímá instanci entity, která obsahuje původní hodnotu vlastnosti sledování.

Tato obecná úložiště bude zpracovávat požadavky na typické CRUD. Pokud typ konkrétní entity má zvláštní požadavky, například složitější filtrování nebo řazení, můžete vytvořit odvozené třídy, která má další metody pro tento typ.

## <a name="creating-the-unit-of-work-class"></a>Vytváření třída pracovní jednotky

Třída pracovní jednotky slouží jediný účel: k Ujistěte se, že při použití více úložiště, sdílejí kontextu jedné databáze. Tímto způsobem, a to po dokončení jednotka práce, můžete volat `SaveChanges` metody u této instance kontextu a mít jistotu, že všechny související změny budou koordinované. Všechno, co je třída potřeb `Save` metod a vlastností pro každý úložiště. Každou vlastnost úložiště vrátí instance úložiště, které byly vytvořeny pomocí stejné instanci kontextu databáze jako ostatní instance úložiště.

V *DAL* složky, vytvořte soubor třídy s názvem *UnitOfWork.cs* a nahraďte kód šablony s následujícím kódem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Kód vytvoří proměnné třídy pro kontext databáze a každý úložiště. Pro `context` proměnné, nový kontext je vytvořena instance:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Každou vlastnost úložiště ověří, zda úložiště již existuje. V opačném případě se vytvoří úložiště, předávání v kontextu instance. V důsledku toho všechny úložiště sdílet stejné instanci kontextu.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save` Volání metod `SaveChanges` na kontext databáze.

Každá třída, která vytvoří kontext databáze v proměnné třídy, jako `UnitOfWork` třída implementuje `IDisposable` a uvolní kontextu.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Změna řadičem během používání třídy UnitOfWork a úložiště

Nahraďte kód v aktuálně máte *CourseController.cs* následujícím kódem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Přidá proměnnou třídy pro tento kód `UnitOfWork` třídy. (Pokud používáte rozhraní tady, nebude inicializovat proměnnou tady; místo toho by implementovat vzor dva konstruktory, stejně jako jste to udělali `Student` úložiště.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

Ve zbývající části třídy, jsou všechny odkazy na kontext databáze nahrazuje odkazy na příslušné úložiště pomocí `UnitOfWork` vlastnosti pro přístup k úložišti. `Dispose` Metoda spravované `UnitOfWork` instance.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Spuštění tohoto webu a klikněte na **kurzy** kartě.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

Na stránce vypadá a funguje stejně jako předtím změny a dalších stránek kurzu také fungovat stejně.

## <a name="summary"></a>Souhrn

Nyní jste implementovali úložišti a jednotky pracovních vzorů. Lambda – výrazy mají použít jako parametry metody v obecné úložiště. Další informace o tom, jak používat tyto výrazy se `IQueryable` objektu, najdete v části [IQueryable(T) rozhraní (System.Linq)](https://msdn.microsoft.com/en-us/library/bb351562.aspx) v knihovně MSDN. V dalším kurzu se dozvíte, jak bude zpracováván některé pokročilé scénáře.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k dat ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Předchozí](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[další](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
