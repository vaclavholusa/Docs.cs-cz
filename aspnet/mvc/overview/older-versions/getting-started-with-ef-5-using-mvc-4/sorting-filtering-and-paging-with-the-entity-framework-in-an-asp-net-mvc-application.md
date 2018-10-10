---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Řazení, filtrování a stránkování s Entity Framework v aplikaci ASP.NET MVC (3 10) | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje postup vytvoření aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a sady Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8bea3d4bc19a5a47240abeb2cc015116814a8fdf
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911816"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Řazení, filtrování a stránkování s Entity Framework v aplikaci ASP.NET MVC (3 10)
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a Visual Studio 2012. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Série kurzů můžete začít od začátku nebo [stáhnout počáteční projekt pro tuto kapitolu](building-the-ef5-mvc4-chapter-downloads.md) a začněte tady.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte sami, [stáhnout dokončený kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a zkuste problém reprodukovat. Porovnáním kód Dokončený kód v obecně najdete řešení problému. Některé běžné chyby a jejich řešení najdete v tématu [chyby a náhradní řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V předchozím kurzu jste implementovali sadu webových stránek pro základní operace CRUD pro `Student` entity. V tomto kurzu přidáte řazení, filtrování a stránkování funkce, které **studenty** indexovou stránku. Také vytvoříte stránky, která provádí jednoduché seskupení.

Následující obrázek znázorňuje, co bude stránka vypadat až to budete mít. Záhlaví sloupců jsou odkazy, které může uživatel kliknout, chcete-li seřadit podle sloupce. Kliknutím na záhlaví opakovaně sloupce přepíná mezi vzestupným a sestupným řazením.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Přidat sloupec řazení odkazy na indexovou stránku studentů

K přidání řazení Student indexovou stránku, změníte `Index` metodu `Student` kontroleru a přidejte kód, který `Student` indexu zobrazení.

### <a name="add-sorting-functionality-to-the-index-method"></a>Přidání funkce k metodě Index řazení

V *Controllers\StudentController.cs*, nahraďte `Index` metodu s následujícím kódem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Tento kód přijme `sortOrder` parametr z řetězce dotazu v adrese URL. ASP.NET MVC poskytuje hodnotu řetězce dotazu jako parametr do metody akce. Parametr bude řetězec, který je buď "Name" nebo "Data", může volitelně následovat symbol podtržítka a řetězec "desc" Zadejte sestupném pořadí. Je výchozí pořadí řazení vzestupně.

Při prvním vyžádání stránky indexu není žádný řetězec dotazu. Studenty se zobrazují ve vzestupném pořadí podle `LastName`, což je výchozí podle propuštěním případ `switch` příkazu. Když uživatel klikne na sloupce záhlaví hypertextový odkaz, odpovídající `sortOrder` v řetězci dotazu je zadaná hodnota.

Dva `ViewBag` proměnné se používají tak, aby zobrazení můžete nakonfigurovat hypertextové odkazy záhlaví sloupce s hodnotami řetězec odpovídající dotaz:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Jedná se o Ternární příkazy. První z nich určuje, že pokud `sortOrder` parametr má hodnotu null nebo prázdná, `ViewBag.NameSortParm` musí být nastavená na "název\_desc"; v opačném případě musí být nastavena na prázdný řetězec. Tyto dva příkazy Povolit zobrazení nastavte sloupec, hypertextové odkazy záhlaví následujícím způsobem:

| Aktuální pořadí řazení | Poslední název hypertextový odkaz | Datum hypertextový odkaz |
| --- | --- | --- |
| Poslední název vzestupně | descending | ascending |
| Poslední název sestupně | ascending | ascending |
| Datum vzestupně | ascending | descending |
| Datum sestupně | ascending | ascending |

Metoda používá [technologii LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) zadat Tento sloupec seřadit podle. Kód vytvoří [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) proměnné před `switch` ho v příkazu, změní `switch` příkazu a volání `ToList` za `switch` příkaz. Pokud při vytváření a úpravách `IQueryable` proměnné, žádný dotaz odeslán do databáze. Dotaz není spuštěn, dokud je převést `IQueryable` objektu do kolekce voláním metody `ToList`. Proto tento kód výsledkem jednoho dotazu, který se spustí až `return View` příkazu.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Přidat záhlaví hypertextové odkazy na studenta Index zobrazení sloupce

V *Views\Student\Index.cshtml*, nahraďte `<tr>` a `<th>` prvky pro řádek záhlaví s zvýrazněný kód:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Tento kód používá informace v `ViewBag` vlastnosti, které chcete nastavit hypertextové odkazy s odpovídající dotaz hodnoty řetězce.

Spuštění stránky a klikněte na tlačítko **příjmení** a **datum registrace** záhlaví sloupce. Ověřte, že řazení funguje.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Po klepnutí **příjmení** záhlaví, studenti jsou zobrazena v sestupném pořadí poslední název.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Přidání vyhledávacího pole na indexovou stránku studentů

Pokud chcete přidat, filtrování, abyste studenty indexovou stránku, budete přidat textové pole a tlačítko pro odeslání do zobrazení a provádět odpovídající změny v `Index` metody. Do textového pole umožňují zadat řetězec k vyhledání křestního jména a poslední název pole.

### <a name="add-filtering-functionality-to-the-index-method"></a>Filtrování doplňují Index – metoda

V *Controllers\StudentController.cs*, nahraďte `Index` – metoda (změny se zvýrazní) následujícím kódem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Přidání `searchString` parametr `Index` metody. Jste také přidali do příkazu LINQ `where` clausethat vybere pouze studenti, jejichž křestní jméno nebo příjmení vyskytuje hledaný řetězec. Přijetí hledání řetězcovou hodnotu z textové pole, které přidáte k zobrazení indexu. Příkaz, který se přidá [kde](https://msdn.microsoft.com/library/bb535040.aspx) klauzule provádí pouze v případě, že je hodnota k vyhledání.

> [!NOTE]
> V mnoha případech můžete volání stejné metody na sadu entit Entity Framework nebo jako rozšiřující metody na kolekci v paměti. Výsledky jsou obvykle stejné, ale v některých případech může lišit. Například rozhraní .NET Framework provádění `Contains` metoda vrátí všechny řádky, pokud předáte prázdný řetězec, ale poskytovateli rozhraní Entity Framework pro SQL Server Compact 4.0 vrátí nulový počet řádků prázdné řetězce. Proto kódem v příkladu (vložení `Where` výroku uvnitř `if` příkaz) zajišťuje, že získáte stejné výsledky pro všechny verze SQL serveru. Také, implementace rozhraní .NET Framework `Contains` metoda provádí porovnání velká a malá písmena ve výchozím nastavení, ale zprostředkovatele Entity Framework SQL Server provést porovnávání ve výchozím nastavení. Proto volání `ToUpper` metoda provést test explicitně velkých a malých písmen zajišťuje, že výsledky neměňte při změně kódu později použít úložiště, které vrátí `IEnumerable` kolekce místo `IQueryable` objektu. (Při volání `Contains` metodu `IEnumerable` kolekce, získat implementace rozhraní .NET Framework;. při jeho volání na `IQueryable` objektu, získáte implementace poskytovatele databáze.)


### <a name="add-a-search-box-to-the-student-index-view"></a>Přidat vyhledávací pole k zobrazení indexu studenta

V *Views\Student\Index.cshtml*, přidejte zvýrazněný kód bezprostředně před zahájením `table` tag, chcete-li vytvořit popisek, textové pole a **hledání** tlačítko.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Spuštění stránky, zadejte vyhledávací řetězec a klikněte na tlačítko **hledání** k ověření, že filtrování funguje.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Všimněte si, že adresa URL neobsahuje "k" hledaný řetězec, což znamená, že pokud označit tuto stránku záložkou, nezískáte filtrovaný seznam při použití na záložku. Změníte **hledání** tlačítka pomocí řetězců dotazu pro kritéria filtru v pozdější části kurzu.

## <a name="add-paging-to-the-students-index-page"></a>Přidání stránkování na indexovou stránku studentů

Přidání stránkování k studenty indexovou stránku, začnete pomocí instalace **PagedList.Mvc** balíček NuGet. Pak provede další změny v `Index` metoda a přidejte odkazy stránkování na `Index` zobrazení. **PagedList.Mvc** je jednou z mnoha dobré stránkování a řazení balíčky pro architekturu ASP.NET MVC a jeho použití v tomto poli je určená jenom jako příklad, nikoli jako doporučení k němu přes jiné možnosti. Následující obrázek znázorňuje odkazy stránkování.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Nainstalujte balíček PagedList.MVC NuGet

NuGet **PagedList.Mvc** balíček automaticky nainstaluje **PagedList** balíčku jako závislost. **PagedList** balíček nainstaluje `PagedList` kolekce typů a rozšíření metod pro `IQueryable` a `IEnumerable` kolekce. Rozšiřující metody vytvořit jednu stránku dat v `PagedList` kolekce z vaší `IQueryable` nebo `IEnumerable`a `PagedList` kolekce poskytuje několik vlastností a metod, které usnadňují stránkování. **PagedList.Mvc** balíček nainstaluje stránkování pomocné rutiny, která zobrazuje tlačítka stránkování.

Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** a potom **spravovat balíčky NuGet pro řešení**.

V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **Online** kartu na levé straně a pak do vyhledávacího pole zadejte "stránkovaného". Když se zobrazí **PagedList.Mvc** balíček, klikněte na tlačítko **nainstalovat**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

V **vyberte projekty** klikněte **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Přidání funkce stránkování Index – metoda

V *Controllers\StudentController.cs*, přidejte `using` příkaz pro `PagedList` obor názvů:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Nahradit `Index` metodu s následujícím kódem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Tento kód přidá `page` parametr, aktuální parametr pořadí řazení a aktuální parametr filtru do podpisu metody, jak je znázorněno zde:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

První stránka se zobrazí, nebo pokud uživatel nebyl kliknul stránkování a řazení odkaz, všechny parametry budou mít hodnotu null. Pokud dojde ke kliknutí na odkaz stránkování, `page` proměnná bude obsahovat číslo stránky k zobrazení.

`A ViewBag` vlastnost představuje zobrazení s aktuální pořadí řazení, protože to musí obsahovat odkazy stránkování aby bylo možné zachovat pořadí řazení, při stránkování stejné:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Jiné vlastnosti `ViewBag.CurrentFilter`, poskytuje zobrazení aktuálního řetězce filtru. Tato hodnota musí být součástí odkazy stránkování, aby byla zachována nastavení filtru během stránkování a je nutné do textového pole. až se obnoví na stránce se zobrazí znovu. Pokud hledaný řetězec se změní při stránkování, stránky se musí resetovat na hodnotu 1, protože nový filtr může vést k zobrazení různých datových. Hledaný řetězec se změní, pokud je zadána hodnota v textovém poli a stisknutí tlačítka Odeslat. V takovém případě `searchString` parametr není null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Na konci metody `ToPagedList` rozšiřující metody na studenty `IQueryable` objekt převede student dotaz na jednu stránku studentů v typu kolekce, který podporuje stránkování. Této stránce studentů je pak předán zobrazení:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` Metoda má číslo stránky. Představují dvě otazníky [operátoru nulového sjednocení](https://msdn.microsoft.com/library/ms173224.aspx). Definuje výchozí hodnotu pro typ s možnou hodnotou Null; operátoru nulového sjednocení výraz `(page ?? 1)` znamená, že návratová hodnota z `page` Pokud má hodnotu, nebo vrátí 1, pokud `page` má hodnotu null.

### <a name="add-paging-links-to-the-student-index-view"></a>Přidat odkazy stránkování na zobrazení indexu studenta

V *Views\Student\Index.cshtml*, nahraďte existující kód následujícím kódem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

`@model` Příkazu v horní části stránky určuje, že nyní získá zobrazení `PagedList` místo objektu `List` objektu.

`using` Příkaz pro `PagedList.Mvc` uděluje přístup do pomocné rutiny MVC pro tlačítka stránkování.

Kód používá přetížení [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) , který umožňuje určit [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Výchozí hodnota [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) odešle formulář dat příspěvku, což znamená, že parametry jsou předány v textu zprávy HTTP a ne v adrese URL jako řetězce dotazu. Při zadávání HTTP GET data formuláře je předána v adrese URL jako řetězce dotazu, který uživatelům umožňuje adresa URL záložky. [W3C pokyny pro použití HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) určit, že GET by měl používat, když akce nemá za následek aktualizaci.

Do textového pole je inicializována s aktuálního vyhledávaného řetězce, takže po kliknutí na nové stránce můžete zobrazit aktuálního vyhledávaného řetězce.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Odkazy záhlaví sloupce použijte řetězec dotazu k předání aktuálního vyhledávaného řetězce kontroleru tak, aby uživatel mohl třídit v rámci filtr výsledků:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Jsou zobrazeny aktuální stránce a celkovém počtu stránek.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Pokud nejsou žádné stránky pro zobrazení, se zobrazí "Stránka 0 0". (V takovém případě je větší než počet stránek číslo stránky protože `Model.PageNumber` 1, a `Model.PageCount` je 0.)

Zobrazí se tlačítka stránkování podle `PagedListPager` pomocné rutiny:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager` Pomocník poskytuje řadu možností, které můžete přizpůsobit, včetně adresy URL a práce se styly. Další informace najdete v tématu [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) na webu GitHub.

Spuštění stránky.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Kliknutím na odkazy stránkování v jiné pořadí řazení pro Ujistěte se, že funguje stránkování. Potom zadejte hledaný řetězec a zkuste to znovu a ověřte, že stránkování také funguje správně s řazením a filtrováním stránkování.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Vytvoření o stránku, která zobrazuje statistiku studenta

Pro společnosti Contoso University webu o stránku budete zobrazovat, kolik studenty zaregistrovali pro každé datum registrace. To vyžaduje seskupování a jednoduché výpočtů na skupinách. K tomu budete postupujte takto:

- Vytvořte třídu modelu zobrazení dat, která je potřeba předat do zobrazení.
- Upravit `About` metodu `Home` kontroleru.
- Upravit `About` zobrazení.

### <a name="create-the-view-model"></a>Vytvoření zobrazení modelu

Vytvoření *modely ViewModels* složky. V této složce, přidejte soubor třídy *EnrollmentDateGroup.cs* a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Upravit domovského Kontrolér

V *HomeController.cs*, přidejte následující `using` příkazů v horní části souboru:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Přidejte proměnnou třídy kontextu databáze ihned po otevření složené závorky pro třídu:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Nahradit `About` metodu s následujícím kódem:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Příkaz LINQ skupiny studentů entity podle data registrace, vypočítá počet entit v každé skupině a ukládá výsledky v kolekci `EnrollmentDateGroup` zobrazit objekty modelu.

Přidat `Dispose` metody:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Změnit zobrazení

Nahraďte kód v *Views\Home\About.cshtml* souboru následujícím kódem:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Spusťte aplikaci a klikněte na tlačítko **o** odkaz. Počet studentů pro každé datum registrace se zobrazí v tabulce.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Volitelné: Nasazení aplikace na platformě Windows Azure

Zatím vaše aplikace je spuštěné místně v rámci služby IIS Express na vašem vývojovém počítači. Chcete-li k dispozici pro ostatní použití přes Internet, musíte nasadit na web poskytovatele hostitelských služeb. V této volitelné části tohoto kurzu nasadíte ji na webu Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Pomocí migrace Code First pro nasazení databáze

Nasazení databáze pomocí migrace Code First. Když vytvoříte profil publikování, který použijete ke konfiguraci nastavení pro nasazení ze sady Visual Studio, vyberte zaškrtávací políčko s názvem **spustit migrace Code First (spuštěno při spuštění aplikace)**. Toto nastavení způsobí, že proces nasazení pro automatickou konfiguraci aplikace *Web.config* souboru na cílovém serveru tak, aby používala Code First `MigrateDatabaseToLatestVersion` inicializátor třídy.

Visual Studio nedělá s databází během procesu nasazení. Když nasazené aplikace získá přístup k databázi po nasazení poprvé, Code First automaticky vytvoří databázi nebo aktualizuje schéma databáze na nejnovější verzi. Pokud aplikace implementuje migrace `Seed` metody, metoda spustí po vytvoření databáze nebo schéma se aktualizuje.

Vaše migrace `Seed` metoda vloží testovací data. Pokud se nasazení do produkčního prostředí, je třeba změnit `Seed` metodu tak, že pouze vkládá nějaká data, která má být vložen do provozní databáze. Aktuální model dat můžete například mít skutečné kurzů, ale fiktivní studenty v databázi vývoj. Můžete napsat `Seed` má metoda načíst i při vývoji a poté komentář fiktivní studenty, před nasazením do produkčního prostředí. Nebo můžete napsat `Seed` má metoda načíst pouze kurzů a zadejte fiktivní studenty v testovací databázi ručně pomocí uživatelského rozhraní aplikace.

### <a name="get-a-windows-azure-account"></a>Získat účet Windows Azure

Budete potřebovat účet Windows Azure. Pokud již nemáte, můžete během několika minut vytvořit Bezplatný zkušební účet. Podrobnosti najdete v tématu [bezplatná zkušební verze Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Vytvoření webu a SQL database v Windows Azure

Svůj web Windows Azure se spustí ve sdíleném hostování prostředí, což znamená, že běží na virtuálních počítačích (VM), které jsou sdíleny s ostatními klienty Windows Azure. Sdílené hostitelského prostředí je s nízkými náklady způsob, jak začít pracovat v cloudu. Později hodnota se zvyšuje webový provoz, aplikace můžete škálovat podle potřeby spouští na vyhrazených virtuálních počítačích. Pokud potřebujete složitější architektury, můžete migrovat do cloudové služby Windows Azure. Cloudové služby běží na vyhrazených virtuálních počítačích, které lze nakonfigurovat podle svých potřeb.

Windows Azure SQL Database je služba relační databáze založené na cloudu, která je založená na technologiích systému SQL Server. Nástroje a aplikace, které fungují s SQL serverem se také pracovat s SQL Database.

1. V [Windows Azure Management Portal](https://manage.windowsazure.com/), klikněte na tlačítko **weby** karta vlevo a pak klikněte na **nový**.

    ![Tlačítka Nový v portálu pro správu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Klikněte na tlačítko **vlastní web**.

    ![Vytvoření s odkazem databáze na portálu pro správu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   **Nový webový server - vytvořit vlastní** otevře se průvodce.
3. V **nový web** kroku v průvodci zadat řetězec **adresy URL** pole použít jako jedinečná adresa URL pro vaši aplikaci. Úplná adresa URL bude obsahovat co zadáte tady a příponu, která se zobrazí vedle textového pole. Na obrázku ukazuje "ConU", ale tuto adresu URL se pravděpodobně používá, budete muset zvolit jinou.

    ![Vytvoření s odkazem databáze na portálu pro správu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. V **oblasti** rozevíracího seznamu, zvolte oblast blízko vás. Toto nastavení určuje datové centrum, kterého webu se spustí v.
5. V **databáze** rozevíracího seznamu, zvolte **vytvořit zdarma 20 MB databází SQL database**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. V **název PŘIPOJOVACÍHO ŘETĚZCE DB**, zadejte *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Klikněte na šipku, která ukazuje vpravo v dolní části seznamu. Průvodce přejde **nastavení databáze** kroku.
8. V **název** zadejte *ContosoUniversityDB*.
9. V **Server** vyberte **nová databáze SQL serveru**. Dříve vytvořený server, je můžete vybrat tento server z rozevíracího seznamu.
10. Zadejte správce **PŘIHLAŠOVACÍ jméno** a **heslo**. Pokud jste vybrali **nová databáze SQL serveru** , tak nezadáváte existující jméno a heslo, ale nové jméno a heslo, které teď definujete pro pozdější použití při přístupu k databázi. Pokud jste vybrali serveru, který jste předtím vytvořili, zadáte přihlašovací údaje pro tento server. Pro účely tohoto kurzu nebude vyberete ***Upřesnit*** zaškrtávací políčko. ***Upřesnit*** možnosti umožňují nastavit databázi [kolace](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Vyberte stejné **oblasti** , kterou jste zvolili pro webovou stránku.
12. Kliknutím na značku zaškrtnutí v pravém dolním rohu políčka potvrďte, že budete hotovi.   
  
    ![Krok nastavení databáze o nový web - vytvořit pomocí Průvodce databáze](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    Následující obrázek ukazuje použití existujícího SQL serveru a přihlašovací jméno.   
  
    ![Krok nastavení databáze o nový web - vytvořit pomocí Průvodce databáze](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    Na portálu pro správu vrátí na stránku webové servery a **stav** sloupci se zobrazuje, že se vytváří web. Po chvíli (obvykle během méně než minutu) **stav** sloupci se zobrazuje, že web byl úspěšně vytvořen. Na navigačním panelu na levé straně, počet lokalit, které máte ve vašem účtu se zobrazí vedle **weby** ikonu a počet databází, které se zobrazí vedle **databází SQL** ikonu.

## <a name="deploy-the-application-to-windows-azure"></a>Nasazení aplikace do Windows Azure

1. Ve Visual Studiu klikněte pravým tlačítkem na projekt v **Průzkumníka řešení** a vyberte **publikovat** v místní nabídce.  
  
    ![Publikování v kontextové nabídce projektu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. V **profilu** karty **Publikovat Web** průvodce, klikněte na tlačítko **Import**.  
  
    ![Import nastavení publikování](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Pokud jste nepřidali dříve předplatného Windows Azure v sadě Visual Studio, proveďte následující kroky. V tomto postupu přidáte předplatné tak, aby rozevíracím seznamu v části **importovat z webu Windows Azure** bude obsahovat vaše webová stránka.

    a. V **importovat profil publikování** dialogové okno, klikněte na tlačítko **importovat z webu Windows Azure**a potom klikněte na tlačítko **odběru služby Windows Azure přidat**.

    ![Přidání odběru služby Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. V **odběrů služby Windows Azure Import** dialogové okno, klikněte na tlačítko **stáhnout soubor předplatného**.

    ![Stáhnout soubor předplatného](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. V okně prohlížeče, uložte *.publishsettings* souboru.

    ![Stáhněte si soubor .publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Zabezpečení – *publishsettings* soubor obsahuje vaše přihlašovací údaje (nekódovaný), které se používají ke správě vašich předplatných Windows Azure a služeb. Osvědčené postupy zabezpečení pro tento soubor je dočasně ukládat mimo vašeho adresáře zdrojových souborů (například v *Libraries\Documents* složky) a pak ji odstraňte po dokončení importu. Uživatel se zlými úmysly, který získá přístup k `.publishsettings` soubor můžete upravit, vytvářet a odstraňovat vaše služby Windows Azure.

    d. V **odběrů služby Windows Azure Import** dialogové okno, klikněte na tlačítko **Procházet** a přejděte do *.publishsettings* souboru.

    ![Stáhněte si sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Klikněte na tlačítko **Import**.

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. V **importovat profil publikování** dialogu **importovat z webu Windows Azure**webu vyberte z rozevíracího seznamu a potom klikněte na tlačítko **OK**.  
  
    ![Import profilu publikování](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. V **připojení** klikněte na tlačítko **ověřit připojení** abyste měli jistotu, že jsou správné nastavení.  
  
    ![Ověření připojení](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Při připojení je potvrzená, se vedle položky zobrazí zelená značka zaškrtnutí **ověřit připojení** tlačítko. Klikněte na tlačítko **Další**.  
  
    ![Úspěšně ověřené připojení](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Otevřít **vzdálený připojovací řetězec** rozevíracího seznamu v části **SchoolContext** a vyberte připojovací řetězec pro databázi, který jste vytvořili.
8. Vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**.
9. Zrušte zaškrtnutí políčka **použít tento připojovací řetězec za běhu** pro **UserContext (objekt DefaultConnection)**, protože tato aplikace není pomocí databáze členství.   
  
    ![Karta nastavení](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Klikněte na tlačítko **Další**.
11. V **ve verzi Preview** klikněte na tlačítko **spustit Náhled**.  
  
    ![Tlačítko StartPreview v kartě náhledu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    Na kartě se zobrazí seznam souborů, které budou zkopírovány na server. Zobrazení náhledu není vyžadováno pro publikování aplikace, ale je užitečná funkce je potřeba vědět. V takovém případě nemusíte dělat nic se seznam souborů, který se zobrazí. Příště tuto aplikaci nasadíte v tomto seznamu bude pouze soubory, které se změnily.  
  
    ![StartPreview výstupního souboru](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Klikněte na tlačítko **publikovat**.  
    Visual Studio spustí proces kopírování souborů na serveru Windows Azure.
13. **Výstup** okno zobrazuje, jaké akce nasazení byly provedeny a oznámí úspěšné dokončení nasazení.  
  
    ![V okně Výstup hlášením úspěšného nasazení](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Po úspěšném nasazení na adresu URL nasazené webové stránky automaticky otevře výchozí prohlížeč.  
    Aplikace, kterou jste vytvořili, je nyní spuštěna v cloudu. Klikněte na kartu studentů.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

V tuto chvíli váš *SchoolContext* databáze byla vytvořena ve službě Windows Azure SQL Database, protože jste vybrali **spustit migrace Code First (spustí se při spuštění aplikace)**. *Web.config* soubor v nasazený web byl změněn tak, aby [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializátor by spustit při prvním kódu čtení nebo zápis dat v databázi (která došlo k výběru **studenty** kartu):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Proces nasazení vytvoří taky nový připojovací řetězec *(SchoolContext\_DatabasePublish*) pro migrace Code First pro aktualizaci schématu databáze a synchronizace replik indexů databáze.

![Database_Publish připojovací řetězec](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*Objekt DefaultConnection* připojovací řetězec je pro databázi členství (který nepoužíváme v tomto kurzu). *SchoolContext* připojovací řetězec je pro databázi ContosoUniversity.

Můžete najít nasazenou verzi souboru Web.config na vašem počítači v *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Můžete přistupovat nasazeném *Web.config* vlastní soubor pomocí protokolu FTP. Pokyny najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Postupujte podle pokynů, které začínají znakem "Pokud chcete použít nástroj FTP, potřebujete tři věci: adresa URL protokolu FTP, uživatelské jméno a heslo."

> [!NOTE]
> Webové aplikace neimplementuje zabezpečení, takže každý, kdo najde adresu URL mohou změnit data. Pokyny o tom, jak zabezpečit webovou stránku, naleznete v tématu [nasazení zabezpečenou aplikaci ASP.NET MVC pomocí členství, OAuth a SQL Database na webu Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Můžete zabránit ostatním uživatelům z webu pomocí portálu správy Windows Azure nebo **Průzkumníka serveru** v sadě Visual Studio Web zastavit.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>První inicializátory kódu

V části nasazení jste viděli, [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) používá inicializátor. Kód nejprve také poskytuje jiné inicializátory, které můžete použít, včetně [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (výchozí), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) a [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Inicializátor může být užitečné pro nastavení podmínky pro testování částí. Můžete je zapsat také vlastních inicializátory a může volat inicializátor explicitně Pokud nechcete čekat, dokud se čte z aplikace nebo zapisuje do databáze. Komplexní vysvětlení inicializátory, naleznete v kapitole 6 knihu [programování rozhraní Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman a Rowan Miller.

## <a name="summary"></a>Souhrn

V tomto kurzu jste viděli, jak vytvořit datový model a provádět základní CRUD, řazení, filtrování, stránkování a seskupování funkce. V dalším kurzu zobrazí za přibližně pohledu na pokročilejší témata tak, že rozbalíte datového modelu.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k Data technologie ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [další](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
