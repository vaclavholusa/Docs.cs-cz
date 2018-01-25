---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Řazení, filtrování a stránkování s platformou Entity Framework v aplikaci ASP.NET MVC (3 10) | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f9b68abeba19561a327bad5ee4be80d79af1a550
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Řazení, filtrování a stránkování s platformou Entity Framework v aplikaci ASP.NET MVC (3 10)
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio 2012. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Kurz řady můžete začít od začátku nebo [stáhnout počáteční projekt pro tato kapitola](building-the-ef5-mvc4-chapter-downloads.md) a začněte zde.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte, [stáhnout dokončené kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a pokuste se znovu provést váš problém. Řešení problému můžete získat obecně tak, že porovnáte svůj kód Dokončený kód. Pro některé běžné chyby a jak je vyřešit, najdete v části [chyby a řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


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

Přidali jste `searchString` parametru `Index` metoda. Jste také přidali do příkazu LINQ `where` clausethat vybere pouze studenty, jejichž křestní jméno nebo příjmení obsahuje řetězec pro hledání. Hodnota řetězce vyhledávání byl přijat z textové pole, které přidáte do zobrazení indexu. Příkaz, který přidá [kde](https://msdn.microsoft.com/library/bb535040.aspx) klauzule se spustí pouze v případě, že je hodnota pro vyhledávání.

> [!NOTE]
> V mnoha případech můžete volat stejnou metodu na sadu entit Entity Framework nebo jako metody rozšíření na kolekci v paměti. Výsledky jsou obvykle stejné, ale v některých případech může být odlišné. Například implementace rozhraní .NET Framework `Contains` metoda vrátí všechny řádky, pokud předáte prázdný řetězec, ale zprostředkovatel Entity Framework pro SQL Server Compact 4.0 vrací nulový počet řádků prázdné řetězce. Proto kód v ukázce (vložení `Where` příkaz uvnitř `if` příkaz) zajišťuje, že dostanete stejné výsledky pro všechny verze systému SQL Server. Navíc implementace rozhraní .NET Framework `Contains` metoda provádí malá a velká písmena porovnání ve výchozím nastavení, ale zprostředkovatelé Entity Framework SQL Server provést porovnávání ve výchozím nastavení. Proto volání `ToUpper` metoda aby test explicitně velká a malá písmena zajišťuje výsledky se nemění při změně kódu později použít úložiště, která se vrátit `IEnumerable` kolekce místo `IQueryable` objektu. (Při volání `Contains` metodu `IEnumerable` kolekce, získat implementace rozhraní .NET Framework;. při jeho volání na `IQueryable` objektu získat implementace zprostředkovatele databáze.)


### <a name="add-a-search-box-to-the-student-index-view"></a>Vyhledávací pole, přidejte do zobrazení indexu studenty

V *Views\Student\Index.cshtml*, přidejte zvýrazněný kód bezprostředně před otevření `table` značky, aby bylo možné vytvořit popisek, textové pole a **vyhledávání** tlačítko.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Spuštění stránky, zadejte vyhledávací řetězec a klikněte na tlačítko **vyhledávání** ověřte funkčnost filtrování.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Všimněte si, že adresa URL neobsahuje "služby" hledaný řetězec, což znamená, že pokud vytvoříte záložku této stránce, nebude dostanete filtrovaný seznam při použití záložky. Změníte **vyhledávání** tlačítko pomocí řetězce dotazu pro kritéria filtrování později v tomto kurzu.

## <a name="add-paging-to-the-students-index-page"></a>Přidat stránkování na indexovou stránku studenty

Pokud chcete přidat stránkování studenty indexovou stránku, spusťte nainstalováním **PagedList.Mvc** balíček NuGet. Pak budete udělat další změny v `Index` metoda a přidejte stránkování odkazy na `Index` zobrazení. **PagedList.Mvc** je jedním z mnoha dobrý řazení a stránkování balíčky pro architekturu ASP.NET MVC a jeho použití v tomto poli je určená jenom jako příklad, ne jako doporučení pro něj přes jiné možnosti. Následující obrázek znázorňuje odkazy stránkování.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Nainstalujte balíček PagedList.MVC NuGet

NuGet **PagedList.Mvc** balíček automaticky nainstaluje **PagedList** balíček jako závislost. **PagedList** balíček nainstaluje `PagedList` kolekci typů a rozšíření metody pro `IQueryable` a `IEnumerable` kolekce. Rozšiřující metody vytvořit jednu stránku dat v `PagedList` kolekce z vaší `IQueryable` nebo `IEnumerable`a `PagedList` kolekce poskytuje několik vlastnosti a metody, které usnadňují stránkování. **PagedList.Mvc** balíček nainstaluje stránkování pomocné rutiny, která zobrazuje tlačítka stránkování.

Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** a potom **spravovat balíčky NuGet pro řešení**.

V **spravovat balíčky NuGet** dialogové okno, klikněte **Online** kartě na levé straně a pak zadejte do vyhledávacího pole "stránkovaného". Až se zobrazí **PagedList.Mvc** balíček, klikněte na tlačítko **nainstalovat**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

V **vyberte projekty** pole, klikněte na tlačítko **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Přidání funkce stránkování metodu indexu

V *Controllers\StudentController.cs*, přidejte `using` příkaz pro `PagedList` obor názvů:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Nahraďte `Index` metoda následujícím kódem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Tento kód přidá `page` parametr, aktuální parametr pořadí řazení a aktuální parametr filtru pro podpis metody, jak je vidět tady:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

První stránka se zobrazí, nebo pokud uživatel nebyl klikli stránkování nebo řazení odkaz, všechny parametry budou mít hodnotu null. Po kliknutí na odkaz stránkování `page` proměnná bude obsahovat číslo stránky pro zobrazení.

`A ViewBag`Vlastnost nabízí zobrazení s aktuální pořadí řazení, protože to musí být součástí stránkování odkazy. Chcete-li zachovat pořadí řazení, stejné při stránkování:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Jinou vlastnost `ViewBag.CurrentFilter`, nabízí zobrazení s aktuální řetězec filtru. Tato hodnota musí být součástí odkazy stránkování, aby byla zachována nastavení filtru během stránkování a je nutné ho obnovit do textového pole, pokud se zobrazí stránku znovu. Řetězec pro hledání dojde ke změně během stránkování, stránky je nutné ho obnovit hodnotu 1, protože nový filtr může mít za následek různé data k zobrazení. Řetězec pro hledání se změní, pokud je zadána hodnota do textového pole a stisknutí tlačítka Odeslat. V takovém případě `searchString` parametr není null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Na konci metody `ToPagedList` rozšiřující metody na na studentů `IQueryable` objekt převede student dotaz na jednu stránku studentů v typu kolekce, která podporuje stránkování. Této stránce studentů je předána do zobrazení:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` Metoda přebírá číslo stránky. Představují dva otazníky [slučování null operátor](https://msdn.microsoft.com/library/ms173224.aspx). Operátor slučování null definuje výchozí hodnotu pro typ s možnou hodnotou Null; výraz `(page ?? 1)` znamená vrátí hodnotu `page` Pokud má hodnotu, nebo 1-li vrátit `page` má hodnotu null.

### <a name="add-paging-links-to-the-student-index-view"></a>Přidat stránkování odkazy na zobrazení indexu studenty

V *Views\Student\Index.cshtml*, existujícího kódu nahraďte následujícím kódem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

`@model` Příkaz v horní části stránky určuje, že nyní získá zobrazení `PagedList` objektu místo `List` objektu.

`using` Příkaz pro `PagedList.Mvc` dává přístup do pomocné rutiny MVC tlačítka stránkování.

Kódu používá přetížení [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) který umožňuje zadat [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Výchozí hodnota [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) odesílat data formuláře s POST, což znamená, že jsou parametry předány v textu zprávy HTTP a není v adrese URL jako řetězce dotazu. Když zadáte GET protokolu HTTP, data formuláře je předán v adrese URL jako řetězce dotazu, který uživatelům umožňuje bookmark adresu URL. [W3C pokyny pro použití metody GET protokolu HTTP](http://www.w3.org/2001/tag/doc/whenToUseGet.html) určit, že by měl používat GET při akci nevede aktualizaci.

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

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Kliknutím na odkazy stránkování v jiné pořadí řazení do Ujistěte se, že funguje stránkování. Pak zadejte hledaný řetězec a zkuste to znovu a ověřte, že stránkování také funguje správně s řazení a filtrování stránkování.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Vytvoření o stránky, která obsahuje statistiku studenty

Pro společnosti Contoso univerzity webu o stránce budete zobrazit, kolik studenti, kteří mají zaregistrované pro každé datum registrace. To vyžaduje seskupování a jednoduché výpočty na skupiny. K tomu, můžete to udělat následující:

- Vytvořte třídu modelu zobrazení pro data, která potřebujete předat do zobrazení.
- Změnit `About` metoda v `Home` řadiče.
- Upravit `About` zobrazení.

### <a name="create-the-view-model"></a>Vytvoření modelu zobrazení

Vytvoření *ViewModels* složky. V této složce přidat soubor třídy *EnrollmentDateGroup.cs* a existujícího kódu nahraďte následujícím kódem:

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

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Volitelné: Nasazení aplikace do systému Windows Azure

Pokud vaše aplikace spuštěna místně v IIS Express na vašem vývojovém počítači. Chcete-li k dispozici pro ostatní uživatelé používat přes Internet, musíte ji nasadit do webové poskytovatele hostitelských služeb. V této volitelné části kurzu nasadíte ji na webu systému Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Pomocí migrace Code First pro nasazení databáze

Pro nasazení databáze používáte migrace Code First. Když vytvoříte profil publikování, který použijete ke konfiguraci nastavení pro nasazení ze sady Visual Studio, vyberte zaškrtávací políčko, které je označené **spustit migrace Code First (spuštěno při spuštění aplikace)**. Toto nastavení způsobí, že proces nasazení pro automatickou konfiguraci aplikace *Web.config* souboru na cílovém serveru tak, aby používala Code First `MigrateDatabaseToLatestVersion` inicializátoru třídy.

Visual Studio nemá žádný s databází během procesu nasazení. Když nasazené aplikace získá přístup k databázi po nasazení poprvé, Code First automaticky vytvoří databázi nebo aktualizuje schéma databáze na nejnovější verzi. Pokud aplikace implementuje migrace `Seed` metoda, metoda spustí po vytvoření databáze nebo schéma se aktualizuje.

Vaše migrace `Seed` metoda vloží testovacích datech. Pokud byly nasazení v produkčním prostředí, je třeba změnit `Seed` metody, které se vloží pouze data, která má být vložen do provozní databáze. V aktuální datového modelu můžete třeba skutečné kurzy ale fiktivních studenti, kteří mají v databázi vývoj. Můžete napsat `Seed` má metoda v vývoj i zatížení a potom komentář fiktivních studenty před nasazením do produkčního prostředí. Nebo můžete napsat `Seed` metoda načíst pouze kurzy, a zadejte fiktivních studenty v databázi testovací ručně pomocí uživatelského rozhraní aplikace.

### <a name="get-a-windows-azure-account"></a>Získat účet služby Windows Azure

Budete potřebovat účet služby Windows Azure. Pokud jste již nemáte, můžete si během několika minut vytvořit Bezplatný zkušební účet. Podrobnosti najdete v tématu [bezplatná zkušební verze systému Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Vytvoření webu a databáze SQL ve službě Windows Azure

Váš web systému Windows Azure se spustí ve sdíleném hostování prostředí, což znamená, že běží na virtuálních počítačích (VM), které jsou sdíleny s ostatními klienty systému Windows Azure. Sdílené hostování prostředí je nízkonákladové způsob, jak začít pracovat v cloudu. Později webového provozu se zvyšuje, aplikace můžete škálovat podle potřeby spuštěním na vyhrazených virtuálních počítačích. Pokud potřebujete složitější architekturu, můžete migrovat do služby Windows Azure Cloud Service. Cloudové služby, spusťte na vyhrazených virtuálních počítačích, které lze nakonfigurovat podle svých potřeb.

Windows Azure SQL Database je služba relační databáze založené na cloudu, která je založen na technologiích systému SQL Server. Nástroje a aplikace, které pracují s SQL serverem také pracovat s databází SQL.

1. V [Windows Azure Management Portal](https://manage.windowsazure.com/), klikněte na tlačítko **weby** levé kartě a pak klikněte na **nový**.

    ![Tlačítko Nový na portálu pro správu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Klikněte na tlačítko **vlastní vytvořit**.

    ![Vytvoření pomocí připojení databáze na portálu pro správu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

 **Nový webový server - vytvořit vlastní** otevře se průvodce.
3. V **nový web** krok v průvodci zadejte řetězec ve **URL** pole, které chcete použít jako jedinečnou adresu URL pro vaši aplikaci. Úplnou adresu URL bude obsahovat co zadáte a příponu, která se zobrazí vedle textového pole. Na obrázku "ConU", ale tuto adresu URL je pravděpodobně prováděné, budete muset. Zvolte jiný.

    ![Vytvoření pomocí připojení databáze na portálu pro správu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. V **oblast** rozevíracího seznamu vyberte oblast blízko vás. Toto nastavení určuje, které datového centra, webový server se spustí v.
5. V **databáze** rozevíracího seznamu vyberte **vytvoření databáze SQL 20 MB volného**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. V **název PŘIPOJOVACÍHO ŘETĚZCE DB**, zadejte *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Klikněte na šipku, která odkazuje na vpravo v dolní části pole. Průvodce přejde **nastavení databáze** krok.
8. V **název** zadejte *ContosoUniversityDB*.
9. V **Server** vyberte **novou databázi SQL serveru**. Případně pokud jste dříve vytvořili serveru, můžete vybrat tento server z rozevíracího seznamu.
10. Zadejte správce **PŘIHLAŠOVACÍ jméno** a **heslo**. Pokud jste vybrali **novou databázi SQL serveru** nezadáváte existující jméno a heslo, zadáváte nové jméno a heslo, které teď definujete pro pozdější použití při přístupu k databázi. Pokud jste vybrali serveru, který jste vytvořili dříve, budete zadejte přihlašovací údaje pro tento server. V tomto kurzu nebude vyberete ***Upřesnit*** zaškrtávací políčko. ***Upřesnit*** možnosti umožňují nastavit databázi [kolace](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Vyberte stejnou **oblast** kterou jste zvolili pro webovou stránku.
12. Klikněte na ikonu zaškrtnutí v pravém dolním rohu políčka potvrďte, že budete hotovi.   
  
    ![Krok nastavení databáze z nový webový server - vytvořit pomocí Průvodce databáze](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

 Následující obrázek ukazuje použití existující Server SQL a přihlášení.   
  
    ![Krok nastavení databáze z nový webový server - vytvořit pomocí Průvodce databáze](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
 Vrátí portálu pro správu na stránku webové servery a **stav** sloupci se zobrazuje vytvoření webu. Po chvíli (obvykle během méně než minuty) **stav** sloupci se zobrazuje, že lokality byl úspěšně vytvořen. V navigačním panelu na levé straně, počet lokalit, které máte ve vašem účtu se zobrazí vedle **weby** ikonu a počet databází, se zobrazí vedle **databází SQL** ikonu.

## <a name="deploy-the-application-to-windows-azure"></a>Nasazení aplikace do systému Windows Azure

1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **publikovat** v místní nabídce.  
  
    ![Publikování v kontextové nabídce projektu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. V **profil** kartě **Publikovat Web** průvodce, klikněte na tlačítko **Import**.  
  
    ![Import nastavení publikování](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Pokud jste to ještě přidali dříve předplatného Windows Azure v sadě Visual Studio, proveďte následující kroky. Tímto postupem přidáte vašeho předplatného, aby rozevíracího seznamu v části **importovat z webu systému Windows Azure** bude obsahovat vašeho webu.

    a. V **importovat profil publikování** dialogové okno, klikněte na tlačítko **importovat z webu systému Windows Azure**a potom klikněte na **odběr služby Windows Azure přidat**.

    ![Přidat odběr služby Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. V **odběry systému Windows Azure Import** dialogové okno, klikněte na tlačítko **soubor odběru stahování**.

    ![Stáhněte si soubor odběru](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. V okně prohlížeče, uložte *.publishsettings* souboru.

    ![Stáhněte si soubor .publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Zabezpečení – *publishsettings* soubor obsahuje vaše pověření (nekódovaných), které se používají ke správě odběry služby Windows Azure a služby. Nejlepším způsobem zabezpečení pro tento soubor je dočasně uložit mimo adresáře zdroje (například v *Libraries\Documents* složky) a poté jej odstraňte po dokončení importu. Uživatel se zlými úmysly, který získá přístup k `.publishsettings` soubor můžete upravit, vytvářet a odstraňovat vaše služby Windows Azure.

    d. V **odběry systému Windows Azure Import** dialogové okno, klikněte na tlačítko **Procházet** a přejděte do *.publishsettings* souboru.

    ![Sub – stažení](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Klikněte na tlačítko **Import**.

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. V **importovat profil publikování** dialogové okno, vyberte **importovat z webu systému Windows Azure**, vyberte webový server z rozevíracího seznamu a pak klikněte na tlačítko **OK**.  
  
    ![Import profilu publikování](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. V **připojení** , klikněte na **ověřit připojení** a ujistěte se, zda jsou správné nastavení.  
  
    ![Ověření připojení](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Při připojení byl ověřen, je vedle zobrazí zelená značka zaškrtnutí **ověřit připojení** tlačítko. Klikněte na tlačítko **Další**.  
  
    ![Úspěšně ověřená připojení](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Otevřete **vzdáleného připojovací řetězec** rozevíracím seznamu v části **SchoolContext** a vyberte připojovací řetězec pro databázi, který jste vytvořili.
8. Vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**.
9. Zrušte zaškrtnutí políčka **použít tento připojovací řetězec za běhu** pro **UserContext (objekt DefaultConnection)**, protože tato aplikace není pomocí databáze členství.   
  
    ![Karta nastavení](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Klikněte na tlačítko **Další**.
11. V **Preview** , klikněte na **spustit Náhled**.  
  
    ![Tlačítko StartPreview na kartě Preview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
 Na kartě zobrazí seznam souborů, které se zkopírují na serveru. Zobrazení náhledu není vyžadován pro publikování aplikace, ale je užitečné funkce znát. V takovém případě nemusíte dělat nic s seznam souborů, který se zobrazí. Pouze soubory, které se změnily při příštím nasadit tuto aplikaci, bude v tomto seznamu.  
  
    ![Výstup souboru StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Klikněte na tlačítko **publikování**.  
 Visual Studio spustí proces kopírování souborů na serveru systému Windows Azure.
13. **Výstup** okno zobrazuje, jaké akce nasazení byly provedeny a hlásí úspěšné dokončení nasazení.  
  
    ![Výstup – okno hlášením úspěšného nasazení](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Po úspěšném nasazení na adresu URL nasazené webové stránky automaticky otevře výchozí prohlížeč.  
 Aplikace, kterou jste vytvořili je nyní spuštěna v cloudu. Klikněte na kartu studenty.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

V tomto okamžiku vaší *SchoolContext* databáze byla vytvořena ve službě Windows Azure SQL Database, protože jste vybrali **spustit migrace Code First (spuštěno při spuštění aplikace)**. *Web.config* změnil soubor na nasazené webu tak, aby [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializátoru by spustit při prvním kódu čtení nebo zápisu dat v databázi (která se stalo, pokud jste vybrali **studenty** kartě):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Proces nasazení taky vytvořit nový připojovací řetězec *(SchoolContext\_DatabasePublish*) pro migrace Code First pro aktualizace schématu databáze a synchronizace replik indexů databáze.

![Database_Publish připojovací řetězec](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*Objekt DefaultConnection* připojovací řetězec je pro databázi členství (které nejsou používáme v tomto kurzu). *SchoolContext* připojovací řetězec je pro databázi ContosoUniversity.

Můžete najít soubor Web.config na vašem počítači v nasazené verzi *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Dostanete nasazené *Web.config* samotném souboru pomocí protokolu FTP. Pokyny najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Postupujte podle pokynů, které začínají na "pomocí nástroje FTP, budete potřebovat tři věci: adresy URL FTP, uživatelské jméno a heslo."

> [!NOTE]
> Webové aplikace neimplementuje zabezpečení, takže každý, kdo najde adresu URL můžete změnit data. Pokyny k zabezpečení webu, najdete v části [nasazení aplikace zabezpečené ASP.NET MVC s členství, OAuth a SQL Database na webu systému Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Můžete zabránit jinými lidmi pomocí webu pomocí portálu Windows Azure Management Portal nebo **Průzkumníka serveru** v sadě Visual Studio k zastavení webu.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>První inicializátory kódu

V části nasazení jste viděli [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializátoru používá. Kód nejprve taky poskytuje další inicializátory, které můžete použít, včetně [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (výchozí), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) a [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Inicializátoru může být užitečná pro nastavení podmínky pro testování částí. Můžete taky napsat vlastní inicializátory a můžete volat inicializátoru explicitně Pokud nechcete čekat, až aplikace čte z nebo zapisuje do databáze. Komplexní vysvětlení inicializátory, naleznete v kapitole 6 knihy [programovací rozhraní Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman a Rowan Lukeš.

## <a name="summary"></a>Souhrn

V tomto kurzu jste viděli, jak vytvořit datový model a implementovat základní CRUD, řazení, filtrování, stránkování a funkce seskupování. V dalším kurzu brzy prohlížení pokročilejší témata rozšířením datový model.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k dat ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Předchozí](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[další](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
