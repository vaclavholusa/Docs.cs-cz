---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementace úložiště a jednotky pracovních vzorů v aplikaci ASP.NET MVC (9, 10) | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje postup vytvoření aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a sady Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 20f82744582faddaffdc5b4785bf208ecf0955a7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756709"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementace úložiště a jednotky pracovních vzorů v aplikaci ASP.NET MVC (9, 10)
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a Visual Studio 2012. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Série kurzů můžete začít od začátku nebo [stáhnout počáteční projekt pro tuto kapitolu](building-the-ef5-mvc4-chapter-downloads.md) a začněte tady.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte sami, [stáhnout dokončený kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a zkuste problém reprodukovat. Porovnáním kód Dokončený kód v obecně najdete řešení problému. Některé běžné chyby a jejich řešení najdete v tématu [chyby a náhradní řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V předchozím kurzu použijete ke snížení redundantní kód v dědičnosti `Student` a `Instructor` tříd entit. V tomto kurzu uvidíte některé způsoby, jak používat úložiště a jednotky pracovních vzorů pro operace CRUD. Stejně jako v předchozím kurzu v tohohle změníte způsob, jak kód funguje se stránkami jste již vytvořili namísto vytváření nové stránky.

## <a name="the-repository-and-unit-of-work-patterns"></a>Úložiště a jednotky pracovních vzorů

Úložiště a jednotky pracovních vzorů slouží k vytvoření abstraktní vrstvu mezi vrstva přístupu k datům a vrstvu obchodní logiky aplikace. Implementaci těchto vzorců může pomoci izolovat aplikace před změnami v úložišti dat a mohou usnadnit automatizované testování částí nebo testy řízený vývoj (TDD).

V tomto kurzu budete implementovat třídy úložiště pro každý typ entity. Pro `Student` vytvoříte úložiště rozhraní a třídy úložiště typu entity. Když vytvoříte instanci úložiště v řadiči, použijete rozhraní tak, aby řadič bude přijímat odkaz na libovolný objekt, který implementuje rozhraní úložiště. Pokud kontroler běží pod webový server, obdrží úložiště, který funguje s Entity Framework. Pokud kontroler běží pod třídu testu jednotek, obdrží úložiště, který pracuje s daty uloženými v tak, aby můžete snadno upravit pro testování, jako je například kolekci v paměti.

Později v tomto kurzu budete používat více úložišť a jednotek práce třídy pro `Course` a `Department` entitu napíše `Course` kontroleru. Třída pracovní jednotky koordinuje práce z více úložišť vytvořením třídy kontextu izolované databáze sdílejí všechny z nich. Pokud chcete mít možnost provádět automatizované testování částí, by vytvořit a používat rozhraní pro tyto třídy stejně, jako jste to udělali pro `Student` úložiště. Pro zjednodušení tohoto kurzu, budete však vytvořit a použít tyto třídy bez rozhraní.

Následující obrázek ukazuje jeden ze způsobů pohodlným vztahy mezi kontroleru a kontextu třídy ve srovnání s bez použití úložiště nebo jednotky pracovní postup vůbec.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

V této řadě kurzů nebude vytvořit testy jednotek. Úvod do TDD s aplikace MVC, která používá vzor úložiště, najdete v části [návod: použití TDD s architekturou ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Další informace o modelu úložiště najdete v článku na následujících odkazech:

- [Vzor úložiště](https://msdn.microsoft.com/library/ff649690.aspx) na webové stránce MSDN.
- [Používání úložiště a jednotky pracovních vzorů s Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) na blogu týmu Entity Framework.
- [Agilní Entity Framework 4 úložiště](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) řadu příspěvků na blogu Julie Lerman.
- [Vytváření účtu v aplikaci HTML5 a jQuery první pohled](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) na blogu Dana Wahlin.

> [!NOTE]
> Existuje mnoho způsobů, jak implementovat úložiště a jednotky pracovních vzorů. Třídy úložiště můžete použít i bez jednotek práce třídy. Můžete implementovat jednoho úložiště pro všechny typy entit a jeden pro každý typ. Pokud se rozhodnete implementovat, jeden pro každý typ, můžete použít samostatné třídy, obecná základní třída a odvozené třídy, nebo abstraktní základní třída a odvozené třídy. Můžete zahrnout obchodní logiku v úložišti nebo omezit na logikou přístupu k datům. Můžete také sestavit vrstvu HAL do vaší třídy kontextu databáze s použitím [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) rozhraní existuje místo [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) typy pro sady entit. Přístupem k implementaci abstraktní vrstvu uvedené v tomto kurzu je jednou z možností je třeba zvážit, ne doporučení pro všechny scénáře a prostředí.


## <a name="creating-the-student-repository-class"></a>Vytvoření třídy úložiště studenta

V *DAL* složce vytvořte soubor třídy *IStudentRepository.cs* a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Tento kód deklaruje typickou sadu metod CRUD, včetně způsobů čtení – ten, který vrátí všechny `Student` entit a ten, který vyhledá jeden `Student` entitu podle ID.

V *DAL* složce vytvořte soubor třídy *StudentRepository.cs* souboru. Nahraďte stávající kód následujícím kódem, který implementuje `IStudentRepository` rozhraní:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Kontext databáze je definována v proměnné třídy a konstruktor očekává, že objektem neúčastnícím se volání a zajistěte tak předání instance kontextu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Může vytvořit instanci nový kontext v úložišti, ale pak pokud jste použili více úložišť v jednom řadiči, každý skončili byste s samostatný kontext. Později budete používat více úložišť v `Course` kontroleru a uvidíte, jak jednotek práce třídy můžete zajistit, že všechny úložiště používají stejný kontext.

Úložiště implementuje [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) a uvolní kontext databáze, protože jste viděli v kontroleru a její metody CRUD provádět volání do kontextu databáze stejným způsobem, který jste předtím viděli.

## <a name="change-the-student-controller-to-use-the-repository"></a>Změnit Student řadiče na použití úložiště

V *StudentController.cs*, nahraďte kód ve třídě aktuálně následujícím kódem. Změny jsou zvýrazněné.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Kontroler nyní deklaruje proměnnou třídy pro objekt, který implementuje `IStudentRepository` rozhraní namísto třídy kontextu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Výchozí (bezparametrový) konstruktor vytvoří novou instanci kontextu a volitelný konstruktor umožňuje volajícímu a zajistěte tak předání kontextu instance.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Pokud jste používali *injektáž závislostí*, nebo DI, nebude nutné výchozí konstruktor vzhledem k tomu, že DI software zajistí, že by vždy poskytnout objekt správné úložiště.)

V metodách CRUD se nyní nazývá úložiště namísto kontextu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

A `Dispose` metoda nyní uvolní úložiště namísto kontextu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Spuštění tohoto webu a klikněte na tlačítko **studenty** kartu.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

Stránka vypadá a funguje stejně jako předtím, než změníte kód, který použije úložiště a dalších stránek Student také fungovat stejně. Existuje ale podstatným rozdílem tak, jak `Index` metody kontroleru nepodporuje filtrování a řazení. Původní verze této metody obsaženy následující kód:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Aktualizovaný `Index` metoda obsahuje následující kód:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Pouze zvýrazněný kód byl změněn.

V původní verzi kódu `students` je zadán jako `IQueryable` objektu. Dotaz se odesílají do databáze, dokud je převeden do kolekce pomocí metody, jako `ToList`, který nedojde, dokud zobrazení indexu má přístup k modelu studentů. `Where` Stane metoda do původního kódu výše `WHERE` klauzule v dotazu SQL, který je odeslán do databáze. Který pak znamená, že pouze vybrané entity se vrátil v databázi. Ale v důsledku změny `context.Students` k `studentRepository.GetStudents()`, `students` proměnné po tomto příkazu `IEnumerable` kolekci, která zahrnuje všechny studenty v databázi. Konečný výsledek použití `Where` metodou je stejný, ale nyní práci v paměti na webovém serveru a ne databáze. Pro dotazy, které vracejí velké objemy dat může to být neefektivní.

> [!TIP]
> 
> **Položka IQueryable vs. IEnumerable**
> 
> Po implementaci úložiště jak je znázorněno zde, i v případě, že zadáte v něco **hledání** pole dotaz odeslaný do systému SQL Server vrátí všechny řádky studentů, protože neobsahuje kritéria vyhledávání:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Tento dotaz vrátí všechna data studentů, protože úložiště spuštění dotazu bez znalosti o kritériích vyhledávání. Proces řazení, použití kritéria hledání a vybrat podmnožinu dat pro stránkování (v tomto případě zobrazuje jenom 3 řádky) se provádí v paměti novější při `ToPagedList` metoda je volána na `IEnumerable` kolekce.
> 
> V předchozí verzi kódu (před implementací úložiště), dotaz neposílají se do databáze až po použití kritérií vyhledávání, když `ToPagedList` je volán na `IQueryable` objektu.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Když je volána ToPagedList v `IQueryable` dotaz odeslaný do systému SQL Server určuje hledaný řetězec a díky tomu jsou vráceny pouze řádky, které splňují kritéria hledání a bez filtrování je potřeba udělat v paměti objektů.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (Následující kurz vysvětluje, jak prozkoumat dotazy odeslané do systému SQL Server.)


Následující části ukazuje, jak implementovat metody úložiště, které vám umožní určit, že by měl tuto práci provádí databáze.

Právě jste vytvořili abstraktní vrstvu mezi kontrolerem a Entity Framework kontext databáze. Pokud se chystáte provádět automatizované testování částí pomocí této aplikace, můžete vytvořit třídu alternativní úložiště v projektu testů jednotek, která implementuje `IStudentRepository` *.* Namísto volání kontextu pro čtení a zápis dat, tato třída mock úložiště může manipulovat s kolekcí v paměti účely otestování funkce řadiče.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementace obecného úložiště a jednotek práce třídy

Vytvoření třídy úložiště pro každý typ entity může vést k velkému redundantní kód, a to může způsobit částečné aktualizace. Předpokládejme například, že budete muset aktualizovat dva typy různých entit v rámci stejné transakce. Pokud každá používá instance kontextu samostatné databáze, jeden může proběhnout úspěšně a druhý může selhat. Jedním ze způsobů, chcete-li minimalizovat redundantní kód je použití obecného úložiště a jeden způsob, jak zajistit, že všechna úložiště používají stejný kontext databáze (a tedy koordinovat všechny aktualizace), je použít jednotky práce třídy.

V této části kurzu vytvoříte `GenericRepository` třídy a `UnitOfWork` třídy a jejich použitím v `Course` kontroléru pro přístup k i `Department` a `Course` sady entit. Jak jsme vysvětlili výše, pro zjednodušení této části kurzu jste vytvoření rozhraní pro tyto třídy. Ale pokud se chystáte je použít k usnadnění TDD, by obvykle implementovat je pomocí rozhraní stejně jako jste to udělali `Student` úložiště.

### <a name="create-a-generic-repository"></a>Vytvoření obecného úložiště

V *DAL* složku, vytvořte *GenericRepository.cs* a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Třída proměnné jsou deklarovány pro kontext databáze a pro sadu entit, která úložiště je vytvořena instance pro:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Konstruktor přijímá instanci kontext databáze a inicializuje proměnnou sada entit:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get` Metoda povolit, aby volající kód zadejte podmínky filtru a ve sloupci pro řazení výsledků podle pomocí výrazů lambda a řetězcový parametr umožňuje volajícímu zadat čárkou oddělený seznam navigačních vlastností pro předběžné načítání:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Kód `Expression<Func<TEntity, bool>> filter` znamená, že volající poskytne výraz lambda podle `TEntity` typu a tento výraz vrátí hodnotu typu Boolean. Například, pokud úložiště je vytvořena instance pro `Student` typ entity, může kód ve volání metody zadejte `student => student.LastName == "Smith` &quot; pro `filter` parametru.

Kód `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` také znamená, že volající poskytne výraz lambda. V takovém případě je vstup do výrazu, ale `IQueryable` objekt pro `TEntity` typu. Výraz vrátí seřazený verzi, která `IQueryable` objektu. Například, pokud úložiště je vytvořena instance pro `Student` typ entity, může kód ve volání metody zadejte `q => q.OrderBy(s => s.LastName)` pro `orderBy` parametru.

Kód v `Get` metoda vytvoří `IQueryable` objekt a poté použije výraz filtru, pokud existuje:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Dále platí výrazy eager načítání po analýze čárkami oddělený seznam:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Nakonec se vztahuje `orderBy` výrazu, pokud existuje a vrátí výsledky; v opačném případě vrátí výsledky z Neseřazený dotazu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Při volání `Get` metody, proveďte filtrování a řazení `IEnumerable` kolekci vrácené metodou namísto zadávání parametrů pro tyto funkce. Ale řazení a filtrování pracovních pak provádějí v paměti na webovém serveru. S použitím těchto parametrů, zajistíte tím, že práce probíhá v databázi spíše než webový server. Další možností je vytvořit odvozené třídy pro konkrétní entitu typů a přidat specializované `Get` metody, jako například `GetStudentsInNameOrder` nebo `GetStudentsByName`. Nicméně komplexní aplikace, výsledkem může velký počet takových odvozené třídy a specializované metody, které by mohly být více práce udržovat.

Kód v `GetByID`, `Insert`, a `Update` je podobný co jste viděli v úložišti pro obecné metody. (Neposkytují parametrem předběžné načítání v `GetByID` podpis, protože nelze provést předběžné načítání s `Find` metoda.)

Jsou k dispozici dvě přetížení pro `Delete` metody:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Jeden z těchto umožňuje předáním pouze ID entity odstranit, a jeden má instanci entity. Jak už jste viděli [zpracování souběžnosti](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) výukový program, pro souběžnosti, budete potřebovat `Delete` metodu, která přebírá instanci entity, která obsahuje původní hodnotu vlastnosti sledování do.

Toto obecného úložiště bude zpracovávat požadavky na typické CRUD. Když má zvláštní požadavky, jako je například mnohem složitější filtrování a řazení, typ konkrétní entity můžete vytvořit odvozené třídy, která má další metody pro daný typ.

## <a name="creating-the-unit-of-work-class"></a>Vytváří se třída pracovní jednotky

Třída pracovní jednotky slouží jediný účel: k Ujistěte se, že při použití více úložišť, sdílejí kontext jedné databáze. Tímto způsobem, po dokončení určitou jednotku práce můžete volat `SaveChanges` metoda u dané instance kontextu a být jistí, že všechny související změny budou koordinované. Všechno, je třída potřebám `Save` metod a vlastností pro každé úložiště. Každá vlastnost úložiště vrátí instanci úložiště, která byla vytvořena pomocí stejné instance kontextu databáze jako další úložiště instancí.

V *DAL* složce vytvořte soubor třídy *UnitOfWork.cs* a nahraďte kód šablony následujícím kódem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Kód vytvoří proměnné třídy pro kontext databáze a každé úložiště. Pro `context` proměnné, nový kontext je vytvořena instance:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Každou vlastnost úložiště ověří, zda úložiště již existuje. V opačném případě se vytvoří úložiště předá v kontextu instance. Všechna úložiště v důsledku toho sdílejí stejné instance kontextu.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save` Volání metody `SaveChanges` na kontext databáze.

Každá třída, která vytvoří kontext databáze do proměnné třídy, jako jsou `UnitOfWork` implementuje třída `IDisposable` a uvolní kontextu.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Změna Kontroleru kurz použití třídy UnitOfWork a úložiště

Nahraďte kód v současné době máte *CourseController.cs* následujícím kódem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Tento kód přidá proměnnou třídy pro `UnitOfWork` třídy. (Pokud používáte rozhraní tady, by inicializovat proměnnou tady; místo toho, měli byste implementovat vzor dva konstruktory stejně, jako jste to udělali pro `Student` úložiště.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

Ve zbývající části třídy, jsou všechny odkazy na kontext databáze nahrazuje odkazy na příslušné úložiště pomocí `UnitOfWork` vlastnosti pro přístup k úložišti. `Dispose` Metoda zlikviduje `UnitOfWork` instance.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Spuštění tohoto webu a klikněte na tlačítko **kurzy** kartu.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

Stránka vypadá a funguje stejně jako předtím provedené změny a další kurz stránky taky fungují stejně.

## <a name="summary"></a>Souhrn

Nyní jste implementovali úložiště a jednotky pracovních vzorů. Výrazy lambda použitých jako parametry metod v obecného úložiště. Další informace o tom, jak používat tyto výrazy se `IQueryable` objektu, najdete v článku [IQueryable(T) rozhraní (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) v knihovně MSDN. V dalším kurzu se dozvíte, jak zpracovat některé pokročilé scénáře.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k Data technologie ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [další](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
