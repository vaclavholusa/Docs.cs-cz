---
title: Jádro ASP.NET MVC s EF Core - řazení, filtru, stránkování - 3 10
author: rick-anderson
description: V tomto kurzu přidáte třídění, filtrování a stránkování funkce na stránku pomocí ASP.NET Core a Entity Framework Core.
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 344e3a1806ff21d8ce335b2b407a8a93baf72c1b
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a>Jádro ASP.NET MVC s EF Core - řazení, filtru, stránkování - 3 10

Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).

V tomto kurzu předchozí implementována sadu webové stránky pro základní operace CRUD pro studenty entity. V tomto kurzu přidáte třídění, filtrování a funkce stránkování pro studenty indexovou stránku. Pokud vytvoříte stránky, který nemá jednoduchý seskupení.

Následující obrázek znázorňuje, co bude stránka vypadat po dokončení. Záhlaví sloupců jsou odkazy, které uživatel může kliknout na řadit podle sloupce. Kliknutím na záhlaví opakovaně sloupce Přepne mezi vzestupné a sestupné řazení.

![Studenti, kteří indexovou stránku](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Přidat sloupec řazení odkazy na indexovou stránku studenty

K přidání řazení Student indexovou stránku, změníte `Index` metoda studenty řadiče a přidejte do zobrazení indexu Student kód.

### <a name="add-sorting-functionality-to-the-index-method"></a>Přidání řazení funkce Index – metoda

V *StudentsController.cs*, nahraďte `Index` metoda následujícím kódem:

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

Tento kód přijme `sortOrder` parametr z řetězce dotazu v adrese URL. Hodnotu řetězce dotazu je poskytovaný ASP.NET MVC základní jako parametr pro metodu akce. Parametr bude řetězec, který je buď "Name" nebo "Datum", může volitelně následovat podtržítkem a řetězec "desc" Zadejte sestupném pořadí. Výchozí pořadí řazení je vzestupně.

Při prvním vyžádání stránky indexu není žádný řetězec dotazu. Studenti, kteří se zobrazí ve vzestupném pořadí podle příjmení, což výchozí nastavení podle v případě patří prostřednictvím `switch` příkaz. Když uživatel klikne na sloupec záhlaví hypertextový odkaz, odpovídající `sortOrder` v řetězci dotazu je zadána hodnota.

Dva `ViewData` prvky (NameSortParm a DateSortParm) se zobrazením používají ke konfiguraci hypertextové odkazy záhlaví sloupce s řetězcové hodnoty odpovídající dotazu.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

Jedná se o Ternární příkazy. První z nich určuje, že pokud `sortOrder` parametr má hodnotu null nebo prázdná, musí být NameSortParm nastavena na "name_desc"; jinak je potřeba ho nastavit na prázdný řetězec. Tyto dva příkazy zapnutí zobrazení nastavit sloupec hypertextové odkazy záhlaví následujícím způsobem:

|  Aktuální řazení  | Poslední název hypertextový odkaz | Datum hypertextový odkaz |
|:--------------------:|:-------------------:|:--------------:|
| Poslední název vzestupné  | descending          | ascending      |
| Poslední sestupném název | ascending           | ascending      |
| Datum vzestupné       | ascending           | descending     |
| Datum sestupném      | ascending           | ascending      |

Metoda používá k určení tento sloupec seřadit podle technologie LINQ to Entities. Kód vytvoří `IQueryable` proměnná před příkazem switch ho změní v příkazu přepínače a počet volání `ToListAsync` metoda po `switch` příkaz. Při vytváření a úprava `IQueryable` proměnné, bude odeslán žádný dotaz do databáze. Dotaz není provést, dokud je převést `IQueryable` objektu do kolekce voláním metody `ToListAsync`. Proto tento kód výsledkem jeden dotaz, který není provést, dokud `return View` příkaz.

Tento kód může získat podrobné s velkým počtem sloupců. [Poslední kurzu této série](advanced.md#dynamic-linq) ukazuje, jak napsat kód, který umožňuje předat název `OrderBy` sloupce v proměnné řetězce.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Přidejte hypertextové odkazy záhlaví sloupců do zobrazení indexu studenty

Nahraďte kód v *Views/Students/Index.cshtml*, s následujícím kódem přidat hypertextové odkazy záhlaví sloupce. Jsou vyznačené změněných řádků.

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

Tento kód používá informace v `ViewData` řetězce hodnoty vlastnosti, které chcete nastavit hypertextové odkazy s odpovídající dotazu.

Spuštění aplikace, vyberte **studenty** a klikněte **příjmení** a **datum registrace** záhlaví sloupce. Ověřte, že řazení funguje.

![Studenti, kteří indexovou stránku v pořadí název](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Přidat vyhledávací pole na studenty indexovou stránku

Pokud chcete přidat, filtrování studenty indexovou stránku, budete přidat textové pole a tlačítko pro odeslání do zobrazení a provádět odpovídající změny v `Index` metoda. Textové pole umožňují zadejte řetězec, který chcete vyhledat v název první a poslední název pole.

### <a name="add-filtering-functionality-to-the-index-method"></a>Přidat další filtrování funkce Index – metoda

V *StudentsController.cs*, nahraďte `Index` metoda následujícím kódem (změny se zvýrazněnou).

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Přidali jste `searchString` parametru `Index` metoda. Hodnota řetězce vyhledávání byl přijat z textové pole, které přidáte do zobrazení indexu. Jste také přidali do příkazu LINQ where klauzuli, která vybere pouze studenty, jejichž křestní jméno nebo příjmení obsahuje řetězec pro hledání. Příkaz, který přidá where klauzule se spustí pouze v případě, že je hodnota pro vyhledávání.

> [!NOTE]
> Tady jsou volání `Where` metodu `IQueryable` objekt a filtr se zpracují na serveru. V některých případech může být volání `Where` metoda jako metody rozšíření na kolekci v paměti. (Například Předpokládejme, že můžete změnit odkaz na `_context.Students` tak místo z EF `DbSet` odkazuje na metodu úložiště, která vrátí `IEnumerable` kolekce.) Výsledek by za normálních okolností stejné, ale v některých případech může být odlišné.
>
>Například implementace rozhraní .NET Framework `Contains` metoda provádí malá a velká písmena porovnání ve výchozím nastavení, ale v systému SQL Server je určena nastavení kolace instance systému SQL Server. Toto nastavení je výchozí pro velká a malá písmena. Může zavolat `ToUpper` metoda aby test explicitně velká a malá písmena: *kde (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*. Zajistí, že výsledky zůstane stejná, pokud změníte kód, abyste mohli používat úložiště, která vrátí hodnotu `IEnumerable` kolekce místo `IQueryable` objektu. (Při volání `Contains` metodu `IEnumerable` kolekce, získat implementace rozhraní .NET Framework;. při jeho volání na `IQueryable` objektu získat implementace zprostředkovatele databáze.) Je však snížení výkonu pro toto řešení. `ToUpper` Kódu by měla být umístěna funkci v klauzuli WHERE příkazu TSQL SELECT. Pro optimalizaci který by bránily pomocí indexu. Vzhledem k tomu, že SQL je nainstalován většinou velká a malá písmena, je vyhýbat se `ToUpper` kód, dokud nemigrujete k úložišti dat malá a velká písmena.

### <a name="add-a-search-box-to-the-student-index-view"></a>Vyhledávací pole, přidejte do zobrazení indexu studenty

V *Views/Student/Index.cshtml*, přidejte zvýrazněný kód bezprostředně před otevření před vytvořením popiskem, textové pole, značka tabulky a **vyhledávání** tlačítko.

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

Tento kód používá `<form>` [značky pomocná](xref:mvc/views/tag-helpers/intro) do textového pole pro vyhledávání a tlačítko Přidat. Ve výchozím nastavení `<form>` značky pomocná odesílat data formuláře s POST, což znamená, že jsou parametry předány v textu zprávy HTTP a není v adrese URL jako řetězce dotazu. Když zadáte GET protokolu HTTP, data formuláře je předán v adrese URL jako řetězce dotazu, který uživatelům umožňuje bookmark adresu URL. Doporučujeme W3C pokyny, které byste měli používat docházet při akci nevede k aktualizaci.

Spuštění aplikace, vyberte **studenty** kartě, zadejte vyhledávací řetězec a kliknutím na tlačítko Hledat ověřte, zda je funkční filtrování.

![Studenti, kteří indexovou stránku s filtrováním](sort-filter-page/_static/filtering.png)

Všimněte si, že adresa URL obsahuje řetězec pro hledání.

```html
http://localhost:5813/Students?SearchString=an
```

Pokud vytvoříte záložku této stránce, získáte při použití záložky filtrovaný seznam. Přidání `method="get"` k `form` značka je v tom, co způsobilo řetězec dotazu, který má být vygenerován.

V této fázi, pokud kliknete na odkaz řazení záhlaví sloupce ztratíte hodnota filtru, který jste zadali v **vyhledávání** pole. Budete to opravíme v další části.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Přidání funkce stránkování pro studenty indexovou stránku

Pokud chcete přidat stránkování studenty indexovou stránku, vytvoříte `PaginatedList` třídu, která využívá `Skip` a `Take` příkazy k filtrování dat na serveru, místo vždy načítání všechny řádky v tabulce. Pak budete udělat další změny v `Index` metoda a přidejte stránkování tlačítek `Index` zobrazení. Následující obrázek znázorňuje tlačítka stránkování.

![studenti, kteří indexovou stránku s odkazy stránkování](sort-filter-page/_static/paging.png)

Ve složce projektu vytvořte `PaginatedList.cs`a pak nahraďte kód šablony s následujícím kódem.

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

`CreateAsync` Metody v tomto kódu trvá velikost stránky a číslo stránky a použije příslušné `Skip` a `Take` příkazy `IQueryable`. Když `ToListAsync` se volá na `IQueryable`, vrátí seznam obsahující pouze k požadované stránce. Vlastnosti `HasPreviousPage` a `HasNextPage` slouží k povolení nebo zakázání **předchozí** a **Další** stránkování tlačítka.

A `CreateAsync` metoda se používá namísto konstruktor k vytvoření `PaginatedList<T>` objekt, protože konstruktory nelze spustit asynchronní kódu.

## <a name="add-paging-functionality-to-the-index-method"></a>Přidání funkce stránkování metodu indexu

V *StudentsController.cs*, nahraďte `Index` metoda následujícím kódem.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Tento kód přidá parametr číslo stránky, aktuální parametr pořadí řazení a aktuální parametr filtru podpis metody.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

První stránka se zobrazí, nebo pokud uživatel nebyl klikli stránkování nebo řazení odkaz, všechny parametry budou mít hodnotu null.  Po kliknutí na odkaz stránkování proměnnou stránky bude obsahovat číslo stránky pro zobrazení.

`ViewData` Element s názvem CurrentSort nabízí zobrazení s aktuální pořadí řazení, protože to musí být součástí stránkování odkazy. Chcete-li zachovat stejné při stránkování pořadí řazení.

`ViewData` Element s názvem CurrentFilter nabízí zobrazení s aktuální řetězec filtru. Tato hodnota musí být součástí odkazy stránkování, aby byla zachována nastavení filtru během stránkování a je nutné ho obnovit do textového pole, pokud se zobrazí stránku znovu.

Řetězec pro hledání dojde ke změně během stránkování, stránky je nutné ho obnovit hodnotu 1, protože nový filtr může mít za následek různé data k zobrazení. Řetězec pro hledání se změní, pokud je zadána hodnota do textového pole a stisknutí tlačítka Odeslat. V takovém případě `searchString` není parametr hodnotu null.

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

Na konci `Index` metody `PaginatedList.CreateAsync` metoda převede student dotaz na jednu stránku studentů v typu kolekce, která podporuje stránkování. Této stránce studentů jsou předána do zobrazení.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

`PaginatedList.CreateAsync` Metoda přebírá číslo stránky. Dva otazníky představují operátor slučování hodnotu null. Operátor slučování null definuje výchozí hodnotu pro typ s možnou hodnotou Null; výraz `(page ?? 1)` znamená vrátí hodnotu `page` Pokud má hodnotu, nebo 1-li vrátit `page` má hodnotu null.

## <a name="add-paging-links-to-the-student-index-view"></a>Přidat stránkování odkazy na zobrazení indexu studenty

V *Views/Students/Index.cshtml*, existujícího kódu nahraďte následujícím kódem. Změny se zvýrazněnou.

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

`@model` Příkaz v horní části stránky určuje, že nyní získá zobrazení `PaginatedList<T>` objektu místo `List<T>` objektu.

Odkazy záhlaví sloupce pomocí řetězce dotazu předat aktuální řetězec pro hledání na řadič, takže uživatel může seřadit v rámci výsledky filtru:

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

Tlačítka stránkování se zobrazí podle značky pomocné rutiny:

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

Spusťte aplikaci a přejděte na stránku studenty.

![studenti, kteří indexovou stránku s odkazy stránkování](sort-filter-page/_static/paging.png)

Kliknutím na odkazy stránkování v jiné pořadí řazení do Ujistěte se, že funguje stránkování. Pak zadejte hledaný řetězec a zkuste to znovu a ověřte, že stránkování také funguje správně s řazení a filtrování stránkování.

## <a name="create-an-about-page-that-shows-student-statistics"></a>Vytvořit stránku o, která uvádí statistiku studenty

Pro web společnosti Contoso univerzity **o** stránky, budete zobrazit, kolik studenti, kteří mají zaregistrované pro každé datum registrace. To vyžaduje seskupování a jednoduché výpočty na skupiny. K tomu, můžete to udělat následující:

* Vytvořte třídu modelu zobrazení pro data, která potřebujete předat do zobrazení.

* Změňte metodu o v domovské kontroleru.

* Upravte o zobrazení.

### <a name="create-the-view-model"></a>Vytvoření modelu zobrazení

Vytvoření *SchoolViewModels* složku *modely* složky.

V nové složky, přidejte soubor třídy *EnrollmentDateGroup.cs* a nahraďte kód šablony s následujícím kódem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Upravit domácí řadiče

V *HomeController.cs*, přidejte následující příkazy v horní části souboru:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

Přidání proměnné třídy kontextu databáze okamžitě po otevření složené závorky pro třídu a získat instance kontextu z DI jádro ASP.NET:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

Nahraďte `About` metoda následujícím kódem:

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

Příkaz LINQ skupiny entit student datu registrace, vypočítá počet entit v každé skupině a ukládá výsledky do kolekce `EnrollmentDateGroup` zobrazit objekty modelu.
> [!NOTE] 
> V 1.0 verzi Entity Framework Core celou sadu výsledků je vrácen do klienta a seskupení se provádí na straně klienta. V některých případech to může způsobit problémy s výkonu. Nezapomeňte testování výkonu s provozním objemů dat a v případě potřeby pomocí nezpracovaná SQL proveďte seskupení na serveru. Informace o tom, jak používat nezpracovaná SQL najdete v tématu [poslední kurzu této série](advanced.md).

### <a name="modify-the-about-view"></a>Upravit o zobrazení

Nahraďte kód v *Views/Home/About.cshtml* soubor s následujícím kódem:

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Spusťte aplikaci a přejděte na stránku o. Počet studenty pro každé datum registrace se zobrazí v tabulce.

![O stránku](sort-filter-page/_static/about.png)

## <a name="summary"></a>Souhrn

V tomto kurzu jste se seznámili jak provádět, řazení, filtrování, stránkování a seskupení. V dalším kurzu budete zjistěte, jak zpracovávat změn datových modelů pomocí migrace.

> [!div class="step-by-step"]
> [Předchozí](crud.md)
> [další](migrations.md)  
