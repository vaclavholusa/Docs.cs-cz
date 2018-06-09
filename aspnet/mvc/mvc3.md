---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (zahrnuje Duben 2011 Tools – aktualizace) ASP.NET MVC 3 je rozhraní pro vytváření škálovatelných standardy webových aplikací pomocí zavedené návrhový vzor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: c7eee987b28a5d7f8b40fe89a7bf7517ec06646f
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034734"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(zahrnuje Duben 2011 Tools – aktualizace)*
> 
> ASP.NET MVC 3 je rozhraní pro vytváření škálovatelných standardy webových aplikací pomocí vzory zavedené návrhu a výkonu technologie ASP.NET a rozhraní .NET Framework.
> 
> Souběžně sdílená nainstaluje s ASP.NET MVC 2, proto začněte ji používat ještě dnes!
> 
> Stažení [zde instalační program](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Hlavní funkce

- Integrované generování uživatelského rozhraní systému extensible prostřednictvím balíčku NuGet
- Šablony HTML 5 povoleno projektů
- Výrazovou zobrazení včetně nový zobrazovací modul Razor
- Výkonné háky pomocí vkládání závislostí a globální filtry akce
- Podpora bohaté JavaScript s nerušivý JavaScript, jQuery ověření a JSON vazby
- *Čtení seznamu úplné funkce [níže](#overview)*

## <a name="top-links"></a>Horní odkazy

Co je nového v architektuře ASP.NET MVC 3

- Phil Haack: [rozhraní ASP.NET MVC 3 vydání](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3, služba WebMatrix, NuGet, IIS Express a Orchard vydané - Microsoft leden webové vydání v kontextu.](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [uvedení verzi ASP.NET MVC 3, IIS Express, SQL CE 4, rozhraní Web Farm Framework, Orchard, služba WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Poznámky k verzi pro architekturu ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Instalace a Nápověda

- Instalovat pomocí ASP.NET MVC 3 [instalačního programu webové platformy (doporučeno)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Instalovat pomocí ASP.NET MVC 3 [spustitelný instalační program](https://go.microsoft.com/fwlink/?LinkID=208140)
- Nainstalujte [rozhraní ASP.NET MVC 3 pro Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Pro čtení [Úvod do kurz ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Získání nápovědy a diskutovat o architektuře ASP.NET MVC 3 v [fóra](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Přehled rozhraní ASP.NET MVC 3

ASP.NET MVC 3 je založený na ASP.NET MVC 1 a 2, přidávání funkcí obě zjednoduší kódování a povolit hlubší rozšíření. Toto téma obsahuje přehled mnoha nových funkcí, které jsou zahrnuté v této verzi uspořádány do následujících částí:

- [Rozšiřitelné generování uživatelského rozhraní s integrací MvcScaffold](#BM_MvcScaffolding)
- [Šablony HTML 5 povoleno projektů](#BM_HTML5)
- [Zobrazovací modul Razor](#BM_TheRazorViewEngine)
- [Podpora pro více modulů zobrazení](#BM_Support_for_Multiple_View_Engines)
- [Vylepšení řadiče](#BM_Controller_Improvements)
- [JavaScript a Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Vylepšení ověření modelu](#BM_Model_Validation_Improvements)
- [Vylepšení vkládání závislostí](#BM_Dependency_Injection_Improvements)
- [Další nové funkce](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Rozšiřitelné generování uživatelského rozhraní s integrací MvcScaffold

Nové generování uživatelského rozhraní systému je snazší vyzvednutí a začít používat efektivní, pokud jste rozhraní framework zcela nové a automatizaci běžných úkolů vývoj, pokud máte zkušenosti a již víte, jaké úlohy.

To je podporované systémem NuGet nové *generování uživatelského rozhraní* balíček s názvem **MvcScaffolding**. Termín "Generování uživatelského rozhraní" je používá mnoho technologie softwarově znamená "rychle generování základní outline vašeho softwaru, které lze poté upravit a přizpůsobit". Generování uživatelského rozhraní balíčku, který vytváříme pro architekturu ASP.NET MVC je výrazně užitečné v několika scénářích:

- **Pokud jste získat rozhraní ASP.NET MVC první**, protože nabízí rychlý způsob, jak získat některé užitečné pracovní kód, který potom můžete upravit a přizpůsobit podle svých potřeb. Díky tomu můžete z poškození prohlížení prázdné stránky, které mají žádné nápad kde začít!
- **Pokud je dobře znát ASP.NET MVC a jsou nyní zkoumat některé nové technologie rozšíření** jako objekt relační mapper, modul zobrazení, knihovnu testování, protože tvůrce že technologie může mít také vytvořit balíček generování uživatelského rozhraní pro něj apod.
- **Pokud vaše pracovní zahrnuje opakovaného vytvoření podobné třídy nebo soubory nějaká**, protože můžete vytvořit vlastní scaffolders, jejichž výstupem testovací zařízení, skripty nasazení nebo jiného ať už budete potřebovat. Vaše vlastní scaffolders všem členům týmu můžete použít příliš.

Další funkce v MvcScaffolding zahrnují:

- Podpora pro projekty C# a VB
- Podpora pro syntaxi Razor a ASPX zobrazení modulů
- Podporuje generování uživatelského rozhraní do oblastí ASP.NET MVC a pomocí rozložení vlastní zobrazení/hlavních serverů
- Výstup můžete snadno přizpůsobit úpravou šablony T4
- Můžete přidat pomocí vlastní logiky prostředí PowerShell a vlastní šablony T4 úplně novou scaffolders. Tyto (a jakékoli vlastní parametry, kterým jste dali) automaticky zobrazí v seznamu dokončení karta konzoly.
- Můžete získat balíčky NuGet, které obsahují další scaffolders pro různé technologie (například ověření – koncepce jednu pro technologie LINQ to SQL nyní existuje) a kombinovat a párovat je společně

Nástroje pro aktualizaci ASP.NET MVC 3 obsahuje sady Visual Studio podpory pro tento systém generování uživatelského rozhraní, jako například:

- Přidáte že dialogové okno řadiče teď podporuje úplné automatické generování uživatelského rozhraní vytvoření, čtení, aktualizaci a odstraňování akce kontroleru a odpovídající zobrazení. Ve výchozím nastavení to scaffold data přístupového kódu pomocí platformy EF Code First.
- Přidat dialogové okno řadiče podporuje *extensible scaffold* prostřednictvím balíčku NuGet balíčků, jako *MvcScaffolding*. To umožňuje zapojení vlastní scaffold do dialogu, který by umožnil vytvoření scaffold jiných technologií přístupu data například NHibernate nebo i JET s ODBCDirect, pokud jste tak sklon!

Další informace o generování uživatelského rozhraní v architektuře ASP.NET MVC 3 najdete v následujících zdrojích informací:

- Steve Sanderson post řady, včetně: 

    1. [Úvod: Vygenerujte projektu ASP.NET MVC 3 s balíčkem MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Standardní využití: typické případy použití a možnosti](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Vztahy jeden mnoho](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Akce generování uživatelského rozhraní a testy jednotek](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Přepsání šablon T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Tento příspěvek: vytvoření vlastní scaffolders](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Scott Hanselman post ze svého primárního řadiče domény 2010 relace [vytváření blogu se společností Microsoft "Nepojmenované balíček z webové láska"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Poznámky k verzi MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Šablony projektů 5 HTML

Dialogové okno Nový projekt obsahuje zaškrtávací políčko Povolit standardu HTML 5, verzích šablony projektů. Tyto šablony využít Modernizr 1.7 kvůli zajištění podpory kompatibility pro HTML 5 a šablon stylů CSS 3 v prohlížečích na nižší úrovni.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Zobrazovací modul Razor

ASP.NET MVC 3 obsahuje nový zobrazovací modul s názvem Razor, který nabízí následující výhody:

- Syntaxe Razor je, čisté a stručné sdělení, které vyžadují s minimálním počtem stisknutí kláves.
- Syntaxe Razor je snadné Další informace najdete v části aplikace je založena na existující jazyky, jako je C# a Visual Basic.
- Visual Studio obsahuje zabarvení IntelliSense a kód pro syntaxi Razor.
- Jednotka testována bez nutnosti spuštění aplikace nebo spustit webový server může být zobrazení syntaxe Razor.

Některé nové funkce Razor zahrnují následující:

- `@model` Syntaxe pro určení typu, je předaná do zobrazení.
- `@* *@` syntaxe komentáře.
- Umožňuje zadat výchozí hodnoty (například `layoutpage`) jednou pro celý web.
- `Html.Raw` Metodu pro zobrazení textu bez kódování HTML ho.
- Podpora pro sdílení kódu mezi více zobrazení (*\_viewstart.cshtml* nebo  *\_viewstart.vbhtml* soubory).

Syntaxe Razor obsahuje taky nové pomocné rutiny HTML, například následující:

- `Chart`. Vykreslí graf, nabízí stejné funkce jako ovládací prvek grafu v technologii ASP.NET 4.
- `WebGrid`. Vykreslí datová mřížka, dokončení s funkcemi stránkování a řazení.
- `Crypto`. Používá k vytvoření správně algoritmy hash řetězce Salt a použita hodnota hash hesla.
- `WebImage`. Vykreslí bitovou kopii.
- `WebMail`. Odešle e-mailovou zprávu.

Další informace o syntaxi Razor najdete v následujících zdrojích informací:

- [Scott Guthrie blogový příspěvek představení Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Scott Guthrie blog příspěvek představení @model – klíčové slovo](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthrie do blogu představení rozložení Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Referenční dokumentace rozhraní API rychlé Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Poznámky k verzi MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Podpora pro více modulů zobrazení

**Přidat zobrazení** dialogové okno v architektuře ASP.NET MVC 3 umožňuje vybrat zobrazovací modul, kterým chcete pracovat, a **nový projekt** dialogové okno umožňuje určit výchozí modul zobrazení pro projekt. Můžete vybrat modul zobrazení webových formulářů (ASPX), Razor nebo modul zobrazení open source, jako [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), nebo [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Vylepšení řadiče

### <a name="global-action-filters"></a>Globální filtry akce

Někdy budete chtít provést logiku před spuštěním metody akce nebo po spuštění metody akce. Za tímto účelem k dispozici ASP.NET MVC 2 filtrů akce. Filtry akce jsou vlastní atributy, které poskytují deklarativní způsob přidání chování akce před a po akce na konkrétní řadič metody akce. Ale v některých případech můžete chtít zadat akce před nebo po akce chování, které platí pro všechny metody akce. MVC 3 umožňuje určit jejich přidáním do globální filtry `GlobalFilters` kolekce. Další informace o globální filtry akce najdete v následujících zdrojích informací:

- [Scott Guthrie blog na ve verzi Preview 3 MVC](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtrování v architektuře ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nové vlastnosti "ViewBag"

Podpora řadiče MVC 2 `ViewData` vlastnost, která umožňuje předat data do zobrazení šablony pomocí slovník pozdní vazbou rozhraní API. V MVC 3, můžete použít také poněkud jednodušší syntaxi pomocí `ViewBag` vlastnost provedete stejnému účelu. Například místo psaní `ViewData["Message"]="text"`, můžete napsat `ViewBag.Message="text"`. Není nutné definovat všechny silně typované třídy používat `ViewBag` vlastnost. Protože je dynamických vlastností, můžete místo toho právě získat nebo nastavit vlastnosti a bude je vyřešit dynamicky za běhu. Interně `ViewBag` vlastnosti se ukládají jako dvojice název/hodnota v `ViewData` slovníku. (Poznámka: ve většině předprodejní verze MVC 3 `ViewBag` vlastnost název `ViewModel` vlastnost.)

### <a name="new-actionresult-types"></a>Nové typy "ActionResult"

Následující `ActionResult` typy a odpovídající pomocné metody jsou nové nebo rozšířené v MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Vrátí stavový kód 404 HTTP do klienta.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Vrátí dočasné přesměrování (kód stavu HTTP 302) nebo trvalé přesměrování (kód stavu protokolu HTTP 301), v závislosti na parametr typu Boolean. Ve spojení s touto změnou [řadič](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) třída teď má tři metody pro provádění trvalé přesměrování: `RedirectPermanent`, `RedirectToRoutePermanent`, a `RedirectToActionPermanent`. Tyto metody vrací instanci třídy `RedirectResult` s `Permanent` vlastnost nastavena na hodnotu `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Vrátí stavový kód HTTP zadaného uživatelem.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript a vylepšení Ajax

Ve výchozím nastavení pomocí rozhraní Ajax a ověření pomocné rutiny v MVC 3 nerušivý JavaScript přístup. Nerušivý JavaScript zabraňuje vložení vložené JavaScript do HTML. To usnadňuje kódu HTML menší a menší přeplněnosti a usnadňuje vyměnit nebo přizpůsobit knihoven jazyka JavaScript. Ověření pomocné rutiny v MVC 3 použít také `jQueryValidate` modulu plug-in ve výchozím nastavení. Pokud chcete chování MVC 2, můžete zakázat pomocí nerušivý JavaScript *web.config* souboru nastavení. Další informace o vylepšení JavaScript a Ajax najdete v následujících zdrojích informací:

- [Základní informace o nerušivý JavaScript na webu Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Nerušivý JavaScript Post Brada Wilsona.](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Post ověření Nerušivý JavaScript Brada Wilsona.](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Vytvoření aplikace MVC 3 s Razor a Nerušivý JavaScript](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (kurz na webu technologie ASP.NET)
- [Poznámky k verzi MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Ve výchozím nastavení povoleno ověřování na straně klienta

V dřívějších verzích MVC, je třeba explicitně volat `Html.EnableClientValidation` metoda ze zobrazení, aby bylo možné povolit ověřování na straně klienta. V MVC 3 totiž už ve výchozím nastavení je povoleno ověřování na straně klienta. (Můžete zakázat pomocí nastavení *web.config* souboru.)

Aby ověřování na straně klienta pro práci budete muset stále odkazovat na příslušné jQuery a ověření knihovny jQuery ve vaší lokalitě. Můžete tyto knihovny hostování na vlastním serveru nebo na ně odkazovat z síti pro doručování obsahu (CDN) jako sítím CDN od společnosti Microsoft nebo Google.

### <a name="remote-validator"></a>Vzdáleného validátoru

ASP.NET MVC 3 podporuje nové [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) třídu, která umožňuje využít výhod jQuery ověření plug v je podpora vzdáleného validátoru. To umožňuje knihovnu ověřování na straně klienta, která má automaticky volání vlastní metody, které definujete, aby bylo možné provést ověření logiky, která lze provést pouze na serveru na straně serveru.

V následujícím příkladu `Remote` atribut určuje, že ověření klienta zavolá akci s názvem `UserNameAvailable` na `UsersController` třída k ověřování `UserName` pole.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

Následující příklad ukazuje odpovídající kontroler.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Další informace o tom, jak používat `Remote` atributů najdete v tématu [postupy: implementace vzdáleného ověření v architektuře ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) v knihovně MSDN.

### <a name="json-binding-support"></a>Podpora formátu JSON vazby

ASP.NET MVC 3 zahrnuje integrovanou podporu vazby JSON, který povoluje metody akce přijímat data zakódovaná ve formátu JSON a navázání ho k parametrům metody akce. Tato možnost je užitečná ve scénářích zahrnující šablony klienta a datové vazby. (Klient šablony umožňují formátování a zobrazení jednotlivé položky nebo sadu datových položek pomocí šablon, které se spouští na klientovi.) MVC 3 umožňuje snadno připojit šablony klienta pomocí metod akce na serveru, které odesílat a přijímat JSON data. Další informace o podpoře vazby JSON najdete v tématu **JavaScript a vylepšení AJAX** části [Scott Guthrie MVC 3 Preview příspěvku na blogu](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Vylepšení ověření modelu

### <a name="dataannotations-metadata-attributes"></a>Atributy metadat "DataAnnotations"

ASP.NET MVC 3 podporuje `DataAnnotations` metadata atributy, jako `DisplayAttribute`.

### <a name="validationattribute-class"></a>"ValidationAttribute" – Třída

`ValidationAttribute` Třída bylo vylepšeno v rozhraní .NET Framework 4 pro podporu nového `IsValid` přetížení, které poskytuje další informace o kontextu aktuální ověření, například jaké objektu je ověřován. To umožňuje bohatší scénáře, kde můžete ověřit aktuální hodnota založené na jiné vlastnosti modelu. Například nové `CompareAttribute` atributů umožňují porovnat hodnoty dvě vlastnosti modelu. V následujícím příkladu `ComparePassword` vlastnost musí odpovídat `Password` pole, aby byla platná.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Ověřování rozhraní

[IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) rozhraní umožňuje provádět ověření na úrovni modelu a umožňuje ověřování chybové zprávy, které jsou specifické pro stav celkové modelu, nebo mezi dvě vlastnosti v modelu . MVC 3 nyní načte chyby `IValidatableObject` rozhraní při vazby modelu a automaticky příznaky nebo označuje vliv na pole v rámci zobrazení pomocí integrované Pomocníci formuláře HTML.

[IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) umožňuje rozhraní ASP.NET MVC zjistilo za běhu zda validátor podporu pro ověření klienta. Toto rozhraní byl navržen tak, aby se dá integrovat celou řadu architektur ověření.

Další informace o ověření rozhraní najdete v tématu **vylepšení ověření modelu** části [Scott Guthrie MVC 3 Preview příspěvku na blogu](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Upozorňujeme však, že odkaz na "IValidateObject" v blogu měl být "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Vylepšení vkládání závislostí

ASP.NET MVC 3 poskytuje lepší podporu pro použití vkládání závislostí (DI) a pro integraci s kontejnery vkládání závislostí nebo v inverzi ovládací prvek (IOC). Byla přidána podpora pro DI v těchto oblastech:

- Řadiče (registrace a vložení objekty Factory, vložení řadiče).
- Zobrazení (registrace a vložení moduly zobrazení, vložení závislosti do zobrazení stránky).
- Filtry akcí (vyhledání a vložení filtry).
- Vazače modelů (registrace a vložení).
- Zprostředkovatele ověření modelu (registrace a vložení).
- Zprostředkovatele metadat modelu (registrace a vložení).
- Zprostředkovatele hodnot (registrace a vložení).

Podporuje MVC 3 [Lokátor společných služeb](https://github.com/unitycontainer/commonservicelocator) knihovny a všech DI kontejner, který podporuje tuto knihovnu `IServiceLocator` rozhraní. Podporuje také novou `IDependencyResolver` rozhraní, které usnadňuje integraci DI architektury.

Další informace o DI v MVC 3 najdete v následujících zdrojích informací:

- [Brad Wilson řadu příspěvcích na blogu na umístění služby](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Poznámky k verzi MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Další nové funkce

### <a name="nuget-integration"></a>Integrace NuGet

ASP.NET MVC 3 automaticky nainstaluje a povolí NuGet v rámci jeho instalaci. NuGet je bezplatný balíček open-source správce, který umožňuje snadno najít, nainstalovat a použít v vašich projektů knihovny .NET a nástroje. Funguje s všechny typy projektů Visual Studio (včetně webových formulářů ASP.NET a architektura ASP.NET MVC).

NuGet umožňuje vývojářům, kteří Udržovat projekty s otevřeným zdrojem (například projekty jako Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks a Elmah) balíčku své knihovny a jejich registrace v online galerie. Pak je snadné pro vývojáře .NET, kteří chtějí používat jednu z těchto knihoven k vyhledání balíčku a nainstalujte ho do projekty, které pracují na.

S aktualizací 3 nástroje ASP.NET zahrnují šablony projektů JavaScript knihovny předinstalované balíčky NuGet, tak, aby byly v aktualizovatelné prostřednictvím balíčku NuGet. Entity Framework Code First předem nainstaluje taky jako balíčku NuGet.

Další informace o systému NuGet najdete v tématu [NuGet dokumentaci](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Části stránky ukládání výstupu do mezipaměti

ASP.NET MVC je podporována ukládání výstupu do mezipaměti celou stránku odpovědí od verze 1. MVC 3 podporuje také části stránky výstupní ukládání do mezipaměti, které vám umožní snadno mezipaměti oblasti nebo fragmenty odpovědi. Další informace o ukládání do mezipaměti najdete v tématu **částečná stránka ukládání výstupu do mezipaměti** části [Scott Guthrie příspěvku na blogu na verzi release candidate MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) a **podřízené akce ukládání výstupu do mezipaměti** části [poznámky k verzi MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Podrobnou kontrolu nad ověření žádosti

ASP.NET MVC je integrované ověření, který automaticky pomáhá chránit před útoky XSS a HTML vkládání. Ale někdy chcete explicitně zakázat žádosti o ověření, například pokud chcete uživatelům post HTML obsahu (například v položkách blogu nebo obsah CMS). Nyní můžete přidat [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) atribut modely nebo Zobrazit modely zakážete ověření požadavku na jednotlivých vlastností během vazby modelu. Další informace o ověření žádosti najdete v následujících zdrojích informací:

- **Nerušivý JavaScript a ověření** kapitoly [Scott Guthrie příspěvku na blogu na verzi release candidate MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Poznámky k verzi MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Rozšiřitelné "nový projekt" dialogové okno

ASP.NET MVC 3 můžete přidat šablony projektů, moduly zobrazení a jednotkové testování projektu architektury **nový projekt** dialogové okno.

### <a name="template-scaffolding-improvements"></a>Vylepšení šablony generování uživatelského rozhraní

Šablony generování uživatelského rozhraní ASP.NET MVC 3 pracovat lépe identifikovat vlastnosti primárního klíče v modelech a zpracování je správně než v dřívějších verzích MVC. (Například šablony generování uživatelského rozhraní nyní zkontrolujte, že není jako pole upravitelné formuláře vygeneroval primární klíč.)

Ve výchozím nastavení, vytvořit a upravit scaffold teď použít `Html.EditorFor` pomocné místo `Html.TextBoxFor` pomocné rutiny. To zlepšuje podporu pro metadata pro model ve formátu data poznámky atributy, kdy **přidat zobrazení** dialogové okno generuje zobrazení.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nové přetížení pro "Html.LabelFor" a "Html.LabelForModel"

Byly přidány nové přetížení metody pro `LabelFor` a `LabelForModel` pomocné metody. Nové přetížení umožňují zadat nebo přepsat na text popisku.

### <a name="sessionless-controller-support"></a>Podporu nerelační řadiče

ASP.NET MVC 3, můžete určit, zda chcete třídy kontroleru se používání stavu relace a pokud ano, jestli stav relace musí být pro čtení a zápis nebo jen pro čtení. Další informace o podpoře nerelační řadiče najdete v tématu [poznámky k verzi MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nová třída "AdditionalMetadataAttribute"

Můžete použít [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) atribut naplnit `ModelMetadata.AdditionalValues` slovník pro vlastnosti modelu. Například pokud model zobrazení má vlastnost, která má být zobrazena pouze pro správce, musíte označit vlastnosti jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Tato metadata je k dispozici žádné šablony zobrazení nebo editoru při vykreslení zobrazení modelu produktu. Je na vás interpretovat informace metadat.

### <a name="accountcontroller-improvements"></a>Vylepšení AccountController

AccountController v šabloně projektů Internetu, se výrazně zlepšila.

### <a name="new-intranet-project-template"></a>Nová šablona projektu intranetu

Nová šablona projektu intranetu je součástí který umožňuje ověřování systému Windows a odebere AccountController.
