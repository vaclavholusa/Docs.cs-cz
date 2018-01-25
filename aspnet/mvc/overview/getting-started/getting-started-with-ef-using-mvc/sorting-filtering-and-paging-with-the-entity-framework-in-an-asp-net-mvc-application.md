---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Řazení, filtrování a stránkování s platformou Entity Framework v aplikaci ASP.NET MVC | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d54c0e133bc2f6f2021821dc16cdf86cc23a5667
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Řazení, filtrování a stránkování s platformou Entity Framework v aplikaci ASP.NET MVC
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stáhnout PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio 2013. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


V tomto kurzu předchozí implementována sadu webové stránky pro základní operace CRUD pro `Student` entity. V tomto kurzu přidáte třídění, filtrování a stránkování funkce **studenty** indexovou stránku. Pokud vytvoříte stránky, který nemá jednoduchý seskupení.

Následující obrázek znázorňuje, co bude stránka vypadat po dokončení. Záhlaví sloupců jsou odkazy, které uživatel může kliknout na řadit podle sloupce. Kliknutím na záhlaví opakovaně sloupce Přepne mezi vzestupné a sestupné řazení.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Přidat sloupec řazení odkazy na indexovou stránku studenty

K přidání řazení Student indexovou stránku, změníte `Index` metodu `Student` řadiče a přidejte kód, který `Student` Index zobrazení.

### <a name="add-sorting-functionality-to-the-index-method"></a>Přidání funkce do metody Index řazení

V *Controllers\StudentController.cs*, nahraďte `Index` metoda následujícím kódem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Tento kód přijme `sortOrder` parametr z řetězce dotazu v adrese URL. ASP.NET MVC poskytuje hodnotu řetězce dotazu jako parametr pro metodu akce. Parametr bude řetězec, který je buď "Name" nebo "Datum", může volitelně následovat podtržítkem a řetězec "desc" Zadejte sestupném pořadí. Výchozí pořadí řazení je vzestupně.

Při prvním vyžádání stránky indexu není žádný řetězec dotazu. Studenti, kteří jsou zobrazeny ve vzestupném pořadí podle `LastName`, který je výchozí zřízený v případě patří prostřednictvím `switch` příkaz. Když uživatel klikne na sloupec záhlaví hypertextový odkaz, odpovídající `sortOrder` v řetězci dotazu je zadána hodnota.

Dva `ViewBag` proměnné se používají tak, aby zobrazení můžete nakonfigurovat hypertextové odkazy záhlaví sloupce s řetězcové hodnoty odpovídající dotazu:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Jedná se o Ternární příkazy. První z nich určuje, že pokud `sortOrder` parametr je null nebo prázdná, `ViewBag.NameSortParm` musí být nastavena na "název\_desc", jinak je potřeba ho nastavit na prázdný řetězec. Tyto dva příkazy zapnutí zobrazení nastavit sloupec hypertextové odkazy záhlaví následujícím způsobem:

| Aktuální řazení | Poslední název hypertextový odkaz | Datum hypertextový odkaz |
| --- | --- | --- |
| Poslední název vzestupné | descending | ascending |
| Poslední sestupném název | ascending | ascending |
| Datum vzestupné | ascending | descending |
| Datum sestupném | ascending | ascending |

Používá metodu [technologie LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) zadat Tento sloupec seřadit podle. Kód vytvoří [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) proměnné před `switch` ho v příkazu, změní `switch` prohlášení a volání `ToList` metoda po `switch` příkaz. Při vytváření a úprava `IQueryable` proměnné, bude odeslán žádný dotaz do databáze. Dotaz není provést, dokud je převést `IQueryable` objektu do kolekce voláním metody `ToList`. Proto tento kód výsledkem jeden dotaz, který není provést, dokud `return View` příkaz.

Jako alternativu k zápisu jiný LINQ příkazy pro každé pořadí řazení můžete dynamicky vytvořit dotaz LINQ. Informace o dynamické LINQ najdete v tématu [dynamické LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Přidání záhlaví hypertextové odkazy na zobrazení indexu Student sloupce

V *Views\Student\Index.cshtml*, nahraďte `<tr>` a `<th>` prvky pro řádek záhlaví s zvýrazněný kód:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Tento kód používá informace v `ViewBag` řetězce hodnoty vlastnosti, které chcete nastavit hypertextové odkazy s odpovídající dotazu.

Spuštění stránky a klikněte na tlačítko **příjmení** a **datum registrace** záhlaví sloupce. Ověřte, že řazení funguje.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Po kliknutí **příjmení** záhlaví, studenti, kteří jsou zobrazeny v sestupném pořadí poslední název.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Přidat vyhledávací pole na indexovou stránku studenty

Pokud chcete přidat, filtrování studenty indexovou stránku, budete přidat textové pole a tlačítko pro odeslání do zobrazení a provádět odpovídající změny v `Index` metoda. Textové pole umožňují zadejte řetězec, který chcete vyhledat v název první a poslední název pole.

### <a name="add-filtering-functionality-to-the-index-method"></a>Přidat další filtrování funkce Index – metoda

V *Controllers\StudentController.cs*, nahraďte `Index` metoda následujícím kódem (změny se zvýrazněnou):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Přidali jste `searchString` parametru `Index` metoda. Hodnota řetězce vyhledávání byl přijat z textové pole, které přidáte do zobrazení indexu. Jste také přidali do příkazu LINQ `where` klauzuli, která vybere pouze studenty, jejichž křestní jméno nebo příjmení obsahuje řetězec pro hledání. Příkaz, který přidá [kde](https://msdn.microsoft.com/library/bb535040.aspx) klauzule se spustí pouze v případě, že je hodnota pro vyhledávání.

> [!NOTE]
> V mnoha případech můžete volat stejnou metodu na sadu entit Entity Framework nebo jako metody rozšíření na kolekci v paměti. Výsledky jsou obvykle stejné, ale v některých případech může být odlišné.
> 
> Například implementace rozhraní .NET Framework `Contains` metoda vrátí všechny řádky, pokud předáte prázdný řetězec, ale zprostředkovatel Entity Framework pro SQL Server Compact 4.0 vrací nulový počet řádků prázdné řetězce. Proto kód v ukázce (vložení `Where` příkaz uvnitř `if` příkaz) zajišťuje, že dostanete stejné výsledky pro všechny verze systému SQL Server. Navíc implementace rozhraní .NET Framework `Contains` metoda provádí malá a velká písmena porovnání ve výchozím nastavení, ale zprostředkovatelé Entity Framework SQL Server provést porovnávání ve výchozím nastavení. Proto volání `ToUpper` metoda aby test explicitně velká a malá písmena zajišťuje výsledky se nemění při změně kódu později použít úložiště, která se vrátit `IEnumerable` kolekce místo `IQueryable` objektu. (Při volání `Contains` metodu `IEnumerable` kolekce, získat implementace rozhraní .NET Framework;. při jeho volání na `IQueryable` objektu získat implementace zprostředkovatele databáze.)
> 
> Null zpracování může být také pro jiné databázi zprostředkovatelé nebo pokud používáte jiné `IQueryable` objekt ve srovnání s použijete `IEnumerable` kolekce. Například v některých scénářích `Where` podmínky, jako `table.Column != 0` nemusí vrátit sloupce, které mají `null` jako hodnotu. Další informace najdete v tématu [nesprávné zpracování null proměnných v klauzuli 'where'](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).


### <a name="add-a-search-box-to-the-student-index-view"></a>Vyhledávací pole, přidejte do zobrazení indexu studenty

V *Views\Student\Index.cshtml*, přidejte zvýrazněný kód bezprostředně před otevření `table` značky, aby bylo možné vytvořit popisek, textové pole a **vyhledávání** tlačítko.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Spuštění stránky, zadejte vyhledávací řetězec a klikněte na tlačítko **vyhledávání** ověřte funkčnost filtrování.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Všimněte si, že adresa URL neobsahuje "služby" hledaný řetězec, což znamená, že pokud vytvoříte záložku této stránce, nebude dostanete filtrovaný seznam při použití záložky. To platí také pro odkazy řazení sloupců, jako se celý seznam seřadíte. Změníte **vyhledávání** tlačítko pomocí řetězce dotazu pro kritéria filtrování později v tomto kurzu.

## <a name="add-paging-to-the-students-index-page"></a>Přidat stránkování na indexovou stránku studenty

Pokud chcete přidat stránkování studenty indexovou stránku, spusťte nainstalováním **PagedList.Mvc** balíček NuGet. Pak budete udělat další změny v `Index` metoda a přidejte stránkování odkazy na `Index` zobrazení. **PagedList.Mvc** je jedním z mnoha dobrý řazení a stránkování balíčky pro architekturu ASP.NET MVC a jeho použití v tomto poli je určená jenom jako příklad, ne jako doporučení pro něj přes jiné možnosti. Následující obrázek znázorňuje odkazy stránkování.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Nainstalujte balíček PagedList.MVC NuGet

NuGet **PagedList.Mvc** balíček automaticky nainstaluje **PagedList** balíček jako závislost. **PagedList** balíček nainstaluje `PagedList` kolekci typů a rozšíření metody pro `IQueryable` a `IEnumerable` kolekce. Rozšiřující metody vytvořit jednu stránku dat v `PagedList` kolekce z vaší `IQueryable` nebo `IEnumerable`a `PagedList` kolekce poskytuje několik vlastnosti a metody, které usnadňují stránkování. **PagedList.Mvc** balíček nainstaluje stránkování pomocné rutiny, která zobrazuje tlačítka stránkování.

Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** a potom **Konzola správce balíčků**.

V **Konzola správce balíčků** okno, zajistěte, aby **zdroj balíčku** je **nuget.org** a **výchozí projekt** je **ContosoUniversity**a potom zadejte následující příkaz:

`Install-Package PagedList.Mvc`

![Nainstalujte PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Sestavte projekt. 

### <a name="add-paging-functionality-to-the-index-method"></a>Přidání funkce stránkování metodu indexu

V *Controllers\StudentController.cs*, přidejte `using` příkaz pro `PagedList` obor názvů:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Nahraďte `Index` metoda následujícím kódem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

Tento kód přidá `page` parametr, aktuální parametr pořadí řazení a aktuální parametr filtru pro podpis metody:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

První stránka se zobrazí, nebo pokud uživatel nebyl klikli stránkování nebo řazení odkaz, všechny parametry budou mít hodnotu null. Po kliknutí na odkaz stránkování `page` proměnná bude obsahovat číslo stránky pro zobrazení.

A `ViewBag` vlastnost nabízí zobrazení s aktuální pořadí řazení, protože to musí být součástí stránkování odkazy. Chcete-li zachovat pořadí řazení, stejné při stránkování:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Jinou vlastnost `ViewBag.CurrentFilter`, nabízí zobrazení s aktuální řetězec filtru. Tato hodnota musí být součástí odkazy stránkování, aby byla zachována nastavení filtru během stránkování a je nutné ho obnovit do textového pole, pokud se zobrazí stránku znovu. Řetězec pro hledání dojde ke změně během stránkování, stránky je nutné ho obnovit hodnotu 1, protože nový filtr může mít za následek různé data k zobrazení. Řetězec pro hledání se změní, pokud je zadána hodnota do textového pole a stisknutí tlačítka Odeslat. V takovém případě `searchString` parametr není null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Na konci metody `ToPagedList` rozšiřující metody na na studentů `IQueryable` objekt převede student dotaz na jednu stránku studentů v typu kolekce, která podporuje stránkování. Této stránce studentů je předána do zobrazení:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` Metoda přebírá číslo stránky. Představují dva otazníky [slučování null operátor](https://msdn.microsoft.com/library/ms173224.aspx). Operátor slučování null definuje výchozí hodnotu pro typ s možnou hodnotou Null; výraz `(page ?? 1)` znamená vrátí hodnotu `page` Pokud má hodnotu, nebo 1-li vrátit `page` má hodnotu null.

### <a name="add-paging-links-to-the-student-index-view"></a>Přidat stránkování odkazy na zobrazení indexu studenty

V *Views\Student\Index.cshtml*, existujícího kódu nahraďte následujícím kódem. Změny se zvýrazněnou.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

`@model` Příkaz v horní části stránky určuje, že nyní získá zobrazení `PagedList` objektu místo `List` objektu.

`using` Příkaz pro `PagedList.Mvc` dává přístup do pomocné rutiny MVC tlačítka stránkování.

Kódu používá přetížení [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) který umožňuje zadat [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Výchozí hodnota [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) odesílat data formuláře s POST, což znamená, že jsou parametry předány v textu zprávy HTTP a není v adrese URL jako řetězce dotazu. Když zadáte GET protokolu HTTP, data formuláře je předán v adrese URL jako řetězce dotazu, který uživatelům umožňuje bookmark adresu URL. [W3C pokyny pro použití metody GET protokolu HTTP](http://www.w3.org/2001/tag/doc/whenToUseGet.html) , abyste měli při akci nevede aktualizaci použít GET.

Textové pole je inicializován aktuální řetězec pro hledání, po kliknutí na tlačítko Nová stránka se zobrazí aktuální hledaný řetězec.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Odkazy záhlaví sloupce pomocí řetězce dotazu předat aktuální řetězec pro hledání na řadič, takže uživatel může seřadit v rámci výsledky filtru:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Zobrazí se aktuální stránku a celkový počet stránek.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Pokud nejsou žádné stránky zobrazení, zobrazí se "Stránka 0 0". (V takovém případě je větší než počet stránek číslo stránky protože `Model.PageNumber` je 1, a `Model.PageCount` je 0.)

Tlačítka stránkování jsou zobrazeny `PagedListPager` pomocné rutiny:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager` Pomocník poskytuje řadu možností, které můžete přizpůsobit, včetně adres URL a styly. Další informace najdete v tématu [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) na webu GitHub.

Spuštění stránky.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Kliknutím na odkazy stránkování v jiné pořadí řazení do Ujistěte se, že funguje stránkování. Pak zadejte hledaný řetězec a zkuste to znovu a ověřte, že stránkování také funguje správně s řazení a filtrování stránkování.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Vytvoření o stránky, která obsahuje statistiku studenty

Pro společnosti Contoso univerzity webu o stránce budete zobrazit, kolik studenti, kteří mají zaregistrované pro každé datum registrace. To vyžaduje seskupování a jednoduché výpočty na skupiny. K tomu, můžete to udělat následující:

- Vytvořte třídu modelu zobrazení pro data, která potřebujete předat do zobrazení.
- Změnit `About` metoda v `Home` řadiče.
- Upravit `About` zobrazení.

### <a name="create-the-view-model"></a>Vytvoření modelu zobrazení

Vytvoření *ViewModels* složku ve složce projektu. V této složce přidat soubor třídy *EnrollmentDateGroup.cs* a nahraďte kód šablony s následujícím kódem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Upravit domácí řadiče

V *HomeController.cs*, přidejte následující `using` příkazy v horní části souboru:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Přidání proměnné třídy kontextu databáze okamžitě po otevření složené závorky pro třídu:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Nahraďte `About` metoda následujícím kódem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Příkaz LINQ skupiny entit student datu registrace, vypočítá počet entit v každé skupině a ukládá výsledky do kolekce `EnrollmentDateGroup` zobrazit objekty modelu.

Přidat `Dispose` metoda:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Upravit o zobrazení

Nahraďte kód v *Views\Home\About.cshtml* soubor s následujícím kódem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Spusťte aplikaci a klikněte **o** odkaz. Počet studenty pro každé datum registrace se zobrazí v tabulce.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Souhrn

V tomto kurzu jste viděli, jak vytvořit datový model a implementovat základní CRUD, řazení, filtrování, stránkování a funkce seskupování. V dalším kurzu brzy prohlížení pokročilejší témata rozšířením datový model.

Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit. Můžete také vyžádat nová témata na [zobrazit mi jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Odkazy na další zdroje Entity Framework najdete v [přístup k datům ASP.NET - doporučené prostředky](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Předchozí](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[další](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
