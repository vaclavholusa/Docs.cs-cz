---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Dokumentace Microsoftu
author: rick-anderson
description: (zahrnuje dubna 2011 Tools pro sadu Vs11) ASP.NET MVC 3 je rozhraní pro vytváření aplikací škálovatelná webů založené na standardech pomocí zavedených návrhový vzor...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 82d18865815568c5df9768fd9dd403f11ebd1714
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752638"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(zahrnuje dubna 2011 Tools pro sadu Vs11)*
> 
> ASP.NET MVC 3 je architektura určená k vytváření aplikací škálovatelná webů založené na standardech pomocí zavedených návrhových postupů a sílu technologie ASP.NET a .NET Framework.
> 
> Instaluje se vedle sebe s ASP.NET MVC 2, abyste mohli začít používat ještě dnes!
> 
> Stáhněte si [zde instalačního programu](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Hlavní funkce

- Integrovaný systém generování uživatelského rozhraní rozšiřitelných prostřednictvím balíčku NuGet
- HTML 5 povolených projektu šablony
- Výrazové zobrazení včetně nový zobrazovací modul Razor
- Výkonné hooks pomocí vkládání závislostí a globální filtry akce
- Podpora bohaté JavaScriptu s nerušivý JavaScript a jQuery ověření, JSON vazby
- *Čtení seznamu úplné funkce [níže](#overview)*

## <a name="top-links"></a>Prvních propojení

Co je nového v architektuře ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 všeobecně dostupné](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3, služba WebMatrix, NuGet, služba IIS Express a Orchard vydání – Microsoft ledna webové vydání v kontextu](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [oznamujeme vydání verze technologie ASP.NET MVC 3, služby IIS Express, SQL CE 4, Web Farm Framework, Orchard, služba WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Poznámky k verzi pro architekturu ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Instalace a Nápověda

- Instalovat pomocí ASP.NET MVC 3 [instalačního programu webové platformy (doporučeno)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Instalovat pomocí ASP.NET MVC 3 [spustitelného souboru instalačního programu](https://go.microsoft.com/fwlink/?LinkID=208140)
- Nainstalujte [ASP.NET MVC 3 pro sadu Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Přečtěte si [Úvod do ASP.NET MVC 3 kurz](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Získejte pomoc a diskutovat o architektuře ASP.NET MVC 3 v [fóra](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 – přehled

ASP.NET MVC 3 je založena na ASP.NET MVC 1 a 2, přidání skvělých funkcí, zjednoduší kódování a povolit podrobnější rozšiřitelnosti. Toto téma obsahuje přehled o řadu nových funkcí, které jsou součástí této verze, uspořádané do následujících částí:

- [Rozšiřitelné generování uživatelského rozhraní s integrací MvcScaffold](#BM_MvcScaffolding)
- [HTML 5 povolených projektu šablony](#BM_HTML5)
- [Zobrazovací modul Razor](#BM_TheRazorViewEngine)
- [Podpora pro více modulů zobrazení](#BM_Support_for_Multiple_View_Engines)
- [Vylepšení kontroleru](#BM_Controller_Improvements)
- [JavaScript a Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Vylepšení ověření modelu](#BM_Model_Validation_Improvements)
- [Vylepšení vkládání závislostí](#BM_Dependency_Injection_Improvements)
- [Další nové funkce](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Rozšiřitelné generování uživatelského rozhraní s integrací MvcScaffold

Nový systém generování uživatelského rozhraní je snazší sbírání a začít používat produktivní, pokud jste úplně nový rozhraní framework a automatizaci běžných úkolů vývoje, pokud máte zkušenosti a už víte, co děláte.

To je podporované systémem NuGet nové *generování uživatelského rozhraní* balíček s názvem **MvcScaffolding**. Pojem "Generování uživatelského rozhraní" se používá mnoho technologie softwarově "rychle generování základní přehled vašeho softwaru, který můžete následně upravit a přizpůsobit". Balíček pro generování uživatelského rozhraní, který vytváříme pro architekturu ASP.NET MVC je velmi výhodné v několika situacích:

- **Pokud jste získat ASP.NET MVC poprvé**, protože poskytuje rychlý způsob, jak získat některé užitečné pracovní kód, který potom můžete upravit a přizpůsobit podle svých potřeb. Není nutné z poškození prohlížení prázdnou stránku a máte představu, kde začít!
- **Pokud dobře znáte ASP.NET MVC a se teď s touto některé nové technologie doplněk** jako je objektově relační Mapovač, modul zobrazení, knihovnu testování, protože tvůrce těmito technologiemi, může také balíček generování uživatelského rozhraní pro ně vytvořili apod.
- **Pokud svou práci zahrnuje opakované vytvoření podobné třídy nebo soubory s nějakým**, protože můžete vytvořit vlastní podpůrné, jejichž výstupem testovací zařízení, skriptů nasazení nebo cokoli, co jiného potřebujete. Každý člen vašeho týmu můžete použít vlastní podpůrné, příliš.

Mezi další funkce v MvcScaffolding patří:

- Podpora pro projekty C# a VB
- Podpora pro moduly zobrazení Razor a ASPX
- Podporuje generování uživatelského rozhraní do oblastí, ASP.NET MVC a používání vlastních zobrazení rozložení/hlavních serverů
- Výstup můžete snadno přizpůsobit úpravou šablony T4
- Můžete přidat zcela novými podpůrné pomocí vlastní logiky prostředí PowerShell a vlastní šablony T4. Tyto (a libovolný vlastní parametr udělili jste jim) automaticky zobrazí v seznamu doplňování tabulátorů konzoly.
- Balíčky NuGet, který obsahuje další podpůrné pro různé technologie, získáte (například že už testování konceptu jednu pro funkci LINQ to SQL) a zkombinujte je dohromady

ASP.NET MVC 3 nástroje Update obsahuje skvělou podporu sady Visual Studio pro tento systém generování uživatelského rozhraní, jako například:

- Přidáte že kontroler dialogu teď podporuje úplné automatické generování uživatelského rozhraní vytvoření, čtení, aktualizace a odstraňování akce kontroleru a odpovídající zobrazení. Ve výchozím nastavení to vygeneruje uživatelské, rozhraní kód přístupu k datům pomocí EF Code First.
- Přidat Dialog kontroler podporuje *rozšiřitelný nástroj scaffold* prostřednictvím balíčku NuGet balíčky, jako *MvcScaffolding*. Díky tomu zapojení vlastní nástroj scaffold do dialogové okno, které by bylo možné vytvořit scaffold pro dalších technologií přístupu k datům, jako je například NHibernate nebo dokonce JET s ODBCDirect, pokud jste tak sklon!

Další informace o generování uživatelského rozhraní v architektuře ASP.NET MVC 3 naleznete na následujících odkazech:

- Steve Sanderson účtování řady, včetně: 

    1. [Představení: Generování uživatelského rozhraní ASP.NET MVC 3 projektu s balíčkem MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Standardní použití: typické případy použití a možnosti](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Vztah jeden mnoho](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Akce generování uživatelského rozhraní a testy jednotek](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Přepsání šablony T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Tento příspěvek: vytváření vlastních podpůrné](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Scott Hanselman příspěvek z jeho relace primárního řadiče domény 2010 [vytváření Blog s Microsoftem "Nepojmenované balíček z webových láskou"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Zpráva k vydání verze MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Šablony projektů 5 HTML

Dialogové okno Nový projekt obsahuje zaškrtávací políčko Povolit HTML 5, verzích šablon projektů. Tyto šablony využívají 1.7 Modernizr pro poskytnutí podpory kompatibility pro HTML 5 a šablon stylů CSS 3 v prohlížečích nižší úrovně.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Zobrazovací modul Razor

ASP.NET MVC 3 přináší nový zobrazovací modul s názvem Razor, která nabízí následující výhody:

- Syntaxe Razor je a výstižně, které vyžadují minimální počet úhozy na klávesnici.
- Razor se snadno učí, v části aplikace je založena na existující jazyky, jako je C# a Visual Basic.
- Visual Studio obsahuje barevné zvýrazňování technologie IntelliSense a kód pro syntaxi Razor.
- Jednotky testování bez nutnosti spuštění aplikace nebo spustit webový server může být zobrazení syntaxe Razor.

Některé nové funkce Razor, patří:

- `@model` Syntaxe pro určení typu předávaný do zobrazení.
- `@* *@` syntaxe komentáře.
- Možnost určit výchozí hodnoty (jako například `layoutpage`) jednou pro celou lokalitu.
- `Html.Raw` Metodu pro zobrazení textu bez kódování HTML ho.
- Podpora pro sdílení kódu mezi více zobrazení (*\_viewstart.cshtml* nebo  *\_viewstart.vbhtml* soubory).

Razor také zahrnuje nové pomocné rutiny HTML, jako je následující:

- `Chart`. Vykreslí grafu, nabízí stejné funkce jako ovládací prvek grafu v technologii ASP.NET 4.
- `WebGrid`. Vykreslí mřížky dat, s funkcí stránkování a řazení.
- `Crypto`. Používá k vytvoření správně algoritmy hash řetězce Salt a mají hodnotu hash hesla.
- `WebImage`. Vykreslí obrázek.
- `WebMail`. Odešle e-mailovou zprávu.

Další informace o syntaxi Razor naleznete na následujících odkazech:

- [Blogový příspěvek Scotta guthrieho, ve kterém Představujeme Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Scott Guthrie blogový příspěvek představení @model – klíčové slovo](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthrie blogový příspěvek představení rozložení Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Syntaxe Razor rozhraní API rychlé odkazy](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Zpráva k vydání verze MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Podpora pro více modulů zobrazení

**Přidat zobrazení** dialogové okno v architektuře ASP.NET MVC 3 umožňuje výběr zobrazovací modul, kterou chcete pracovat, a **nový projekt** dialogové okno umožňuje zadat výchozí zobrazovací modul pro projekt. Můžete použít modul zobrazení webových formulářů (ASPX), Razor nebo zobrazení open source modul, jako [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), nebo [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Vylepšení kontroleru

### <a name="global-action-filters"></a>Globální filtry akce

Někdy budete chtít provádět logiku před spuštěním metody akce nebo po spuštění metody akce. Z toho důvodu k dispozici ASP.NET MVC 2 filtrů akce. Filtry akce jsou vlastní atributy, které poskytují deklarativní způsob chování předběžnou akce a akce po přidání do metody akce kontroleru konkrétní. Nicméně v některých případech můžete chtít určit akce před nebo po akci chování, které platí pro všechny metody akce. MVC 3 umožňuje určit jejich přidáním do globální filtry `GlobalFilters` kolekce. Další informace o globální filtry akce najdete v následujících zdrojích:

- [Blog Scotta Guthrieho na MVC 3 ve verzi Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtrování v architektuře ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nové vlastnosti "Objekt ViewBag"

Podpora řadiče MVC 2 `ViewData` vlastnost, která umožňuje předat data do zobrazení šablony pomocí rozhraní API slovník s pozdní vazbou. MVC 3, můžete použít také poněkud jednodušší syntaxe `ViewBag` vlastnost k provedení ke stejnému účelu. Například místo zápisu `ViewData["Message"]="text"`, můžete napsat `ViewBag.Message="text"`. Není nutné definovat všechny třídy silného typu určený `ViewBag` vlastnost. Protože se dynamické vlastnosti, můžete místo toho stačí získat nebo nastavit vlastnosti a vyřeší je dynamicky za běhu. Interně `ViewBag` vlastnosti se ukládají jako dvojice název/hodnota v `ViewData` slovníku. (Poznámka: ve většině verzí předběžné verze MVC 3 `ViewBag` označovala jako vlastnost `ViewModel` vlastnosti.)

### <a name="new-actionresult-types"></a>Nové typy "ActionResult"

Následující `ActionResult` typů a odpovídající pomocné metody jsou nové nebo vylepšené v MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Vrátí stavový kód 404 HTTP klientovi.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Vrátí dočasné přesměrování (kód stavu HTTP 302) nebo trvalé přesměrování (kód stavu protokolu HTTP 301), v závislosti na parametr logické hodnoty. Ve spojení s touto změnou [řadič](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) třídy teď má tři metody pro trvalé přesměrování: `RedirectPermanent`, `RedirectToRoutePermanent`, a `RedirectToActionPermanent`. Tyto metody vrací instanci `RedirectResult` s `Permanent` nastavenou na `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Vrátí stavový kód HTTP zadané uživatelem.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript a Ajax vylepšení

Ve výchozím nastavení používají metodiky nerušivý JavaScript Ajax a ověření pomocné rutiny v MVC 3. Nerušivý JavaScript se vyhnete vkládá vložený kód JavaScript do kódu HTML. To usnadňuje kódu HTML menších a méně zaplněnou a usnadňuje vyměnit nebo přizpůsobit knihoven jazyka JavaScript. Také použití pomocné rutiny ověření v MVC 3 `jQueryValidate` modulu plug-in ve výchozím nastavení. Pokud chcete chování MVC 2, můžete zakázat pomocí nerušivého jazyka JavaScript *web.config* souboru nastavení. Další informace o vylepšení JavaScript a Ajax naleznete na následujících odkazech:

- [Základní informace o nerušivý JavaScript na wikipedii webu](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Příspěvek Nerušivý JavaScript Brada Wilsona.](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Příspěvek ověření Nerušivého jazyka JavaScript Brada Wilsona.](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Vytvoření aplikace MVC 3 s Razor a Nerušivý JavaScript](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (kurz na webu technologie ASP.NET)
- [Zpráva k vydání verze MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Ve výchozím nastavení povolené ověřování na straně klienta

V dřívějších verzích MVC, je třeba explicitně volat `Html.EnableClientValidation` metoda ze zobrazení, pokud chcete povolit ověřování na straně klienta. MVC 3. to se už nevyžaduje vzhledem k tomu, že ve výchozím nastavení je povoleno ověřování na straně klienta. (Tuto funkci můžete vypnout pomocí nastavení v *web.config* souboru.)

Aby ověřování na straně klienta pro práci budete muset stále odkaz odpovídající jQuery a knihovny jQuery ověřování ve vaší lokalitě. Můžete hostovat na vlastním serveru těchto knihoven nebo odkazovat z síť pro doručování obsahu (CDN) jako CDN od společnosti Microsoft nebo Google.

### <a name="remote-validator"></a>Program pro vzdálené ověření

ASP.NET MVC 3 podporuje nový [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) třídu, která umožňuje využít výhod jQuery ověření plug v je podpora vzdáleného validátoru. To umožňuje knihovny ověřování na straně klienta automaticky volání vlastní metody, které definujete, aby bylo možné provést logiku ověřování, které lze provést pouze na serveru na straně serveru.

V následujícím příkladu `Remote` atribut určuje, že ověřování na straně klienta bude volat akci s názvem `UserNameAvailable` na `UsersController` třídy s cílem ověřit `UserName` pole.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

Následující příklad ukazuje odpovídající kontroler.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Další informace o tom, jak používat `Remote` atributu naleznete v tématu [postupy: implementace vzdáleného ověřování v architektuře ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) v knihovně MSDN.

### <a name="json-binding-support"></a>Podpora vazeb JSON

ASP.NET MVC 3 zahrnuje integrovanou podporu vazby JSON, který povoluje metody akce přijímat data zakódovaná ve formátu JSON a navázání ho k parametrům metody akce. Tato možnost je užitečná ve scénářích zahrnující klienta šablony a datové vazby. (Klientských šablon umožňují formátování a zobrazit jednotlivé datové položky nebo množinou datových položek pomocí šablony, které jsou spouštěny na straně klienta.) MVC 3 vám umožní snadno připojit šablony klienta pomocí metody akce na serveru, které odesílat a přijímat JSON data. Další informace o podpoře vazby JSON, najdete v článku **jazyka JavaScript a AJAX vylepšení** část [blogový příspěvek Scotta Guthrieho na MVC 3 ve verzi Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Vylepšení ověření modelu

### <a name="dataannotations-metadata-attributes"></a>Atributy metadat na "DataAnnotations"

Podporuje ASP.NET MVC 3 `DataAnnotations` metadat jako atributy `DisplayAttribute`.

### <a name="validationattribute-class"></a>Třída "ValidationAttribute"

`ValidationAttribute` Byla vylepšena z třídy v rozhraní .NET Framework 4 pro podporu nového `IsValid` přetížení, která poskytuje další informace o aktuální kontext ověřování, jako je například objekt, který se ověřuje. To umožňuje bohatší scénáře, ve kterém můžete ověřit aktuální hodnotu podle jiné vlastnosti modelu. Například nové `CompareAttribute` atribut umožňuje porovnat hodnoty dvě vlastnosti modelu. V následujícím příkladu `ComparePassword` vlastnost musí odpovídat `Password` pole, aby byla platná.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Ověřování rozhraní

[IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) rozhraní umožňuje provádět ověření na úrovni modelu a umožňuje pro ověřování chybové zprávy, které jsou specifické pro stav celkové modelu, nebo mezi dvě vlastnosti v rámci modelu . MVC 3 teď načte chyby `IValidatableObject` rozhraní vazby modelu a automaticky příznaky nebo vybraná vystoupení vliv na pole v rámci zobrazení pomocí integrovaných pomocných rutin HTML formulář.

[IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) umožňuje rozhraní ASP.NET MVC za běhu zjistit, zda validátor podporu pro ověřování na straně klienta. Toto rozhraní byly navržené tak, aby ji bylo možné integrovat s celou řadu architektur ověření.

Další informace o ověření rozhraní najdete v článku **vylepšení ověření modelu** část [blogový příspěvek Scotta Guthrieho na MVC 3 ve verzi Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Mějte však na paměti, že odkaz na "IValidateObject" v blogu by měl být "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Vylepšení vkládání závislostí

ASP.NET MVC 3 poskytuje lepší podporu pro použití Dependency Injection (DI) a pro integraci s kontejnery injektáž závislostí nebo IOC (Inversion of Control). Byla přidána podpora pro DI v následujících oblastech:

- Řadiče (registrace a vkládá objekty Factory, vkládání řadiče).
- Zobrazení (registrace a vkládá moduly zobrazení, vkládání závislostí do zobrazení stránky).
- Filtry akcí (vyhledávání a vkládání filtry).
- Vazače modelů (registrace a vkládání).
- Zprostředkovatele ověření modelu (registrace a vkládání).
- Zprostředkovatele metadat modelu (registrace a vkládání).
- Zprostředkovatele hodnot (registrace a vkládání).

MVC 3 podporuje [Lokátor společných služeb](https://github.com/unitycontainer/commonservicelocator) knihovny a všech DI kontejner, který podporuje tuto knihovnu `IServiceLocator` rozhraní. Také podporuje nový `IDependencyResolver` rozhraní, které zjednodušuje integraci DI architektury.

Další informace o DI v MVC 3 naleznete na následujících odkazech:

- [Brad Wilson řadě blogových příspěvků na umístění služby](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Zpráva k vydání verze MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Další nové funkce

### <a name="nuget-integration"></a>Integrace NuGet

ASP.NET MVC 3 automaticky nainstaluje a povolí NuGet při jeho instalaci. Správce balíčků NuGet je Správce balíčků bezplatná open source, který umožňuje snadno najít, nainstalovat a používat knihovny .NET a nástroje ve vašich projektech. Funguje to všechny typy projektů Visual Studio (včetně webových formulářů ASP.NET a ASP.NET MVC).

NuGet umožňuje vývojářům, kteří udržují opensourcové projekty (například projekty, jako jsou Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks a Elmah) balení své knihovny a jejich registrace v online galerie. Pak je snadné pro vývojáře na platformě .NET, kteří chtějí používat jednu z těchto knihoven najít balíček a nainstalovat v projektech, které pracují.

S ASP.NET 3 nástroje Update zahrnují šablony projektů jazyka JavaScript knihovny předinstalované balíčky NuGet, tak, aby byly aktualizovat prostřednictvím balíčku NuGet. Entity Framework Code First také předem nainstalovaný jako balíček NuGet.

Další informace o systému NuGet najdete v tématu [dokumentace pro NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Stránky ukládání výstupu do mezipaměti

ASP.NET MVC se podporuje ukládání výstupu do mezipaměti odpovědi na celou stránku od verze 1. MVC 3 také podporuje výstup stránky ukládání do mezipaměti, což vám umožní snadno mezipaměti oblastí nebo fragmenty odpovědi. Další informace o ukládání do mezipaměti najdete v tématu **částečná stránka ukládání výstupu do mezipaměti** část [blogový příspěvek Scotta Guthrieho na MVC 3 verze Release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) a **podřízené akce ukládání výstupu do mezipaměti** část [zpráva k vydání verze MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Detailní kontrola žádosti o ověření

ASP.NET MVC je ověření integrované žádosti, která automaticky chrání před útoky prostřednictvím injektáže XSS a HTML. Ale někdy chcete explicitně zakázat žádost o ověření, třeba když chcete umožnit uživatelům odeslat HTML obsahu (například článků blogu nebo obsah CMS). Teď můžete přidávat [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) atribut k modelům nebo Zobrazit modely zakážete ověření požadavku na jednotlivých vlastností během vazby modelu. Další informace o ověření požadavku najdete v článku na následujících odkazech:

- **Nerušivý JavaScript a ověření** tématu [blogový příspěvek Scotta Guthrieho na MVC 3 verze Release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Zpráva k vydání verze MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Rozšiřitelné "nový projekt" dialogové okno

V architektuře ASP.NET MVC 3 můžete přidat šablony projektů, moduly zobrazení a projekt rozhraní pro testování částí **nový projekt** dialogové okno.

### <a name="template-scaffolding-improvements"></a>Vylepšení šablony generování uživatelského rozhraní

Šablony pro generování uživatelského rozhraní ASP.NET MVC 3 dělat lepší práci identifikující vlastnosti primárního klíče na modely a zpracování je odpovídajícím způsobem než v dřívějších verzích MVC. (Například šablony generování uživatelského rozhraní nyní Ujistěte se, že se primární klíč automaticky jako pole upravitelné formuláře.)

Ve výchozím nastavení, vytvořit a upravit scaffold teď používají `Html.EditorFor` pomocné místo `Html.TextBoxFor` pomocné rutiny. To zlepšuje podporu pro metadata o modelu ve formě dat anotace atributy, kdy **přidat zobrazení** dialogové okno generuje zobrazení.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nová přetížení pro "Html.LabelFor" a "Html.LabelForModel"

Byly přidány nové přetížení metody `LabelFor` a `LabelForModel` pomocné metody. Nová přetížení umožňují určit nebo přepsat text popisku.

### <a name="sessionless-controller-support"></a>Podpora nerelační Kontroleru

V architektuře ASP.NET MVC 3 můžete určit, zda chcete, aby třída kontroleru používání stavu relace a pokud ano, zda relace stav by měl být čtení a zápis nebo jen pro čtení. Další informace o podpoře nerelační kontroleru, naleznete v tématu [zpráva k vydání verze MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nová třída "AdditionalMetadataAttribute"

Můžete použít [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) atribut naplnit `ModelMetadata.AdditionalValues` slovník pro vlastnosti modelu. Například pokud model zobrazení obsahuje vlastnost, která má být zobrazena pouze pro správce, musíte označit tuto vlastnost jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Tato metadata je k dispozici žádné šablony zobrazení nebo editoru při vykreslení zobrazení modelu produktu. Je jenom na vás interpretovat informace metadat.

### <a name="accountcontroller-improvements"></a>Vylepšení AccountController

AccountController v šabloně projektu Internet je výrazně Vylepšená.

### <a name="new-intranet-project-template"></a>Nová šablona projektu intranetu

Nový intranetový projekt šablona je zahrnuta který umožňuje ověřování Windows a odebere AccountController.
