---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Tento dokument popisuje verzi rozhraní ASP.NET MVC 4.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: dbcea6090a0376b8732e02c0891721672bfe50f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Tento dokument popisuje verzi rozhraní ASP.NET MVC 4.


- [Poznámky k instalaci](#_Toc303253802)
- [Dokumentace](#_Toc303253803)
- [Podpora](#_Toc303253804)
- [Požadavky na software](#_Toc303253805)
- [Nové funkce v architektuře ASP.NET MVC 4](#_Toc303253807)

    - [Webové rozhraní API v ASP.NET](#_Toc317096197)
    - [Vylepšení výchozí šablony projektů](#_Toc303253808)
    - [Šablona projektu mobilní](#_Toc303253809)
    - [Režimy zobrazení](#_Toc303253810)
    - [jQuery Mobile, přepínači zobrazení a přepsání prohlížeče](#_Toc303253811)
    - [Podpora úloh pro asynchronní řadiče](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Migrace databáze](#_Toc303253818)
    - [Prázdná šablona projektu](#_Toc303253819)
    - [Přidat řadič do libovolné složky projektu](#_Toc303253820)
    - [Vytváření sady a minifikace](#_Toc303253821)
    - [Povolení přihlášení ze sítě Facebook a jiných lokalitách pomocí OAuth a OpenID](#_Toc303253822)
- [Upgrade projektu aplikace ASP.NET MVC 3 na rozhraní ASP.NET MVC 4](#_Toc303253806)
- [Změny z verze Release Candidate architektury ASP.NET MVC 4](#_Toc303253817)
- [Známé problémy a nejnovější změny](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Poznámky k instalaci

ASP.NET MVC 4 pro Visual Studio 2010 můžete nainstalovat z [domovskou stránku ASP.NET MVC 4](../mvc/mvc4.md) pomocí služby instalace webové platformy.

Doporučujeme odinstalaci všechny dříve nainstalované náhledy ASP.NET MVC 4 před instalací architektury ASP.NET MVC 4. ASP.NET MVC 4 Beta a verze Release Candidate můžete upgradovat na ASP.NET MVC 4 bez odinstalace.

Tato verze není kompatibilní s všechny verze preview rozhraní .NET Framework 4.5. Žádné nainstalované preview verze rozhraní .NET Framework 4.5 je nutné upgradovat samostatně na finální verzi před instalací architektury ASP.NET MVC 4.

ASP.NET MVC 4 může být nainstalovaná a spustit-souběžného v architektuře ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentace

Dokumentace k rozhraní ASP.NET MVC je k dispozici na webu MSDN na následující adrese URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Kurzy a další informace o architektuře ASP.NET MVC jsou dostupné na stránce webu ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Podpora

ASP.NET MVC 4 je plně podporována. Pokud máte dotazy týkající se práce v této verzi můžete také publikovat je do fóra ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), kde jsou často schopen poskytnout neformální podporu členové komunity služby ASP.NET.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Požadavky na software

Komponenty architektury ASP.NET MVC 4 pro sadu Visual Studio vyžadují prostředí PowerShell 2.0 a buď Visual Studio 2010 s aktualizací Service Pack 1 nebo Visual Web Developer Express 2010 s aktualizací Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Nové funkce v architektuře ASP.NET MVC 4

Tato část popisuje funkce, které byly zavedeny ve verzi ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>Rozhraní API pro ASP.NET Web

ASP.NET MVC 4 obsahuje rozhraní ASP.NET Web API, nové rozhraní pro vytváření služeb HTTP, které mohou být využity širokou škálou klientů včetně prohlížečů a mobilních zařízení. Rozhraní ASP.NET Web API je také ideální platformu pro vytváření služeb RESTful.

Rozhraní ASP.NET Web API zahrnuje podporu pro následující funkce:

- **Moderní programovací model HTTP:** přímo přistupovat a zpracování požadavků HTTP a odpovědi v webová rozhraní API pomocí nové, silného typu HTTP objektový model. Stejný programovací model a kanál protokolu HTTP je symetricky dostupný na klientovi prostřednictvím nové *HttpClient* typu.
- **Podpora pro trasy plně:** rozhraní ASP.NET Web API podporuje úplnou sadu možností směrování ASP.NET, včetně parametry trasy a omezení trasy. Kromě toho použijte jednoduché konvence k mapování akcí na metody HTTP.
- **Vyjednávání obsahu:** klient a server můžou spolupracovat a určit správném formátu pro data návratu z webového rozhraní API. Rozhraní ASP.NET Web API poskytuje výchozí podporu pro formát XML, JSON, a kódovaná adresou URL formuláře formáty a můžete rozšířit tato podpora přidáním vlastní formátování, nebo dokonce nahrazení výchozí strategie vyjednávání obsahu.
- **Modelu a ověření vazeb:** vazače modelů poskytují snadný způsob, jak extrahovat data z různých částí požadavku HTTP a převést objekty .NET, které se dají použít v akcích webového rozhraní API ty části zprávy. Ověření je také provádět akce parametry podle datových poznámek.
- **Filtry:** rozhraní ASP.NET Web API podporuje filtry včetně dobře známé filtry, jako *[Authorize]* atribut. Můžete vytvářet a zařadit vlastní filtry akce, autorizace a výjimek.
- **Vytváření dotazu:** použití *[Queryable]* atribut filtru na akci, která vrátí *IQueryable* povolení podpory pro dotazování webového rozhraní API prostřednictvím konvence dotazu OData.
- **Vylepšené možnosti testování:** místo nastavení HTTP podrobnosti ve statickém kontextu objekty, webové rozhraní API akce pracují s instancí *HttpRequestMessage* a *objekt HttpResponseMessage*. Vytvoření projektu testování částí společně s projektu webového rozhraní API začít rychle zápis testů částí pro fungování vašeho webového rozhraní API.
- **Konfigurace založená na kódu:** výhradně prostřednictvím kódu se provádí konfiguraci webového rozhraní API ASP.NET, a vaše konfigurace vyčistit soubory. Slouží ke konfiguraci body rozšiřitelnosti vzoru Lokátor zadaná služba.
- **Vylepšená podpora pro kontejnery v inverzi řízení (IoC):** rozhraní ASP.NET Web API poskytuje podpory pro kontejnery IoC prostřednictvím abstrakci překladač závislostí vylepšené
- **Hostování na vlastním:** webová rozhraní API je možné hostovat v vlastního procesu kromě služby IIS, při použití stále potenciál trasy a další funkce webového rozhraní API.
- **Vytvoření vlastní Nápověda a testovat stránky:** teď můžete snadno vytvářet vlastní Nápověda a testovat stránky webového rozhraní API pomocí nové *IApiExplorer* službu a získat úplný runtime popis webového rozhraní API.
- **Monitorovací a diagnostické:** rozhraní ASP.NET Web API nyní poskytuje infrastrukturu lehká trasování, která lze snadno integrovat existující řešení protokolování, jako je například System.Diagnostics, trasování a protokolování rozhraní třetích stran. Můžete povolit trasování tím, že poskytuje *ITraceWriter* implementace a její přidání do vaší konfigurace rozhraní web API.
- **Odkaz generování:** používat rozhraní ASP.NET Web API *UrlHelper* ke generování odkazy na související prostředky ve stejné aplikaci.
- **Šablona projektu webového rozhraní API:** vyberte nový formulář průvodce Nový projekt MVC 4 s rozhraním ASP.NET Web API rychle získat nastavení a spuštění projektu webového rozhraní API.
- **Generování uživatelského rozhraní:** použití **přidat kontroler** dialogovém okně můžete rychle vygenerovat řadič webového rozhraní API, která je založena na Entity Framework na základě typu modelu.

Pro další informace o rozhraní ASP.NET Web API naleznete [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Vylepšení výchozí šablony projektů

Šablony, která se používá k vytvoření nové projekty ASP.NET MVC 4 je aktualizovaná k vytvoření webu více moderních vyhledávání:

![](mvc4-release-notes/_static/image1.png)

Kromě kosmetické vylepšení obsahuje vylepšené funkce v nové šabloně. Šablona využívá techniku zvanou adaptivního vykreslování na vzhledu v stolních počítačů a mobilních prohlížečů bez nutnosti přizpůsobení.

![](mvc4-release-notes/_static/image2.png)

Pokud chcete zobrazit adaptivního vykreslování v akci, můžete použít emulátoru mobilního nebo zkuste použít Změna velikosti okna Prohlížeč pro stolní počítač, na menší. Když v okně prohlížeče získá dostatečně malé, se změní rozložení stránky.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Šablona projektu mobilní

Pokud se spuštění nového projektu a chcete vytvořit web speciálně pro mobilní a tablet prohlížeče, můžete v šabloně projektů nové mobilní aplikace. To je založené na technologii jQuery Mobile, knihovny open source pro vytváření dotykovým uživatelského rozhraní:

![](mvc4-release-notes/_static/image3.png)

Tato šablona obsahuje stejnou strukturu aplikací jako šablonu internetovou aplikaci (a kódu kontroleru je téměř identický), ale je navržen tak, funkční a chovat i na mobilních zařízeních se systémem touch pomocí jQuery Mobile. Další informace o tom, jak struktury a stylu mobilní uživatelského rozhraní, najdete v článku [jQuery mobilní projektu webu](http://jquerymobile.com/).

Pokud již máte orientované na ploše lokality, které chcete přidat mobilní optimalizované zobrazení nebo pokud chcete vytvořit v jedné lokalitě, který slouží jinak stylem zobrazení pro stolní počítače a mobilní prohlížeče, můžete novou funkci režimů zobrazení. (Viz další část).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Režimy zobrazení

Nová funkce režimů zobrazení umožňuje aplikaci, vyberte zobrazení v závislosti na prohlížeči, který je vytvoření požadavku. Například pokud prohlížeč pro stolní počítač požadavky na domovskou stránku, aplikace může použít šablonu Views\Home\Index.cshtml. Pokud prohlížeč pro mobilní zařízení požadavky na domovskou stránku, aplikace může vrátit Views\Home\Index.mobile.cshtml šablony.

Rozložení a částečné můžete také přepsat u určitého prohlížeče typů. Příklad:

- Pokud vaše složku Views\Shared obsahuje i \_Layout.cshtml a \_Layout.mobile.cshtml šablony, ve výchozím nastavení aplikace bude používat \_Layout.mobile.cshtml během požadavky z mobilních prohlížečů a \_Layout.cshtml během dalších požadavků.
- Pokud složka obsahuje oba \_MyPartial.cshtml a \_MyPartial.mobile.cshtml pokyn @Html.Partial("\_MyPartial") vykreslí \_MyPartial.mobile.cshtml během požadavky od mobile prohlížeče, a \_MyPartial.cshtml během dalších požadavků.

Pokud chcete vytvořit více konkrétních zobrazení, rozložení nebo částečné zobrazení pro jiná zařízení, můžete zaregistrovat nový *DefaultDisplayMode* instance k určení, který název pro vyhledání, pokud požadavek splňuje určité podmínky. Můžete například přidat následující kód, který *aplikace\_spustit* metoda v souboru Global.asax zaregistrovat řetězec "iPhone" jako režim zobrazení, která se použije, když prohlížeč Apple iPhone odešle žádost:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Po tento kód spustí, když požádá prohlížeč Apple iPhone, vaše aplikace bude používat Views\Shared\\_Layout.iPhone.cshtml rozložení (pokud existuje). Další informace o režimu zobrazení najdete v tématu [ASP.NET MVC 4 mobilní funkce](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Nainstalovat aplikace, které používají DisplayModeProvider [pevné DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) balíček NuGet. [ASP.NET patří 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322) zahrnuje [pevné DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) balíček NuGet v nové šablony projektu. V tématu [ASP.NET MVC 4 Mobile ukládání do mezipaměti chyb Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) podrobnosti o opravu.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile a mobilní funkce

Informace o vytváření mobilních aplikací pomocí ASP.NET MVC 4 pomocí jQuery Mobile, najdete v tématu kurzu [ASP.NET MVC 4 mobilní funkce](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Podpora úloh pro asynchronní řadiče

Nyní můžete napsat metody asynchronní akce v jedné metody, které vracejí objekt typu *úloh* nebo *úloh&lt;ActionResult&gt;*.

 Další informace najdete v části [použití asynchronních metod v architektuře ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 podporuje verzi 1.6 a novějších verzích Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migrace databáze

Projekty ASP.NET MVC 4 nyní zahrnují Entity Framework 5. Jednou z skvělé funkce na Entity Framework 5 je podpora pro databáze migrace. Tato funkce umožňuje snadno momentální svého schématu databáze pomocí migrace zaměřené na kód při zachování dat v databázi. Další informace o databázi migrace najdete v tématu [přidáním nového pole modelu film a tabulku](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) v [Úvod do architektury ASP.NET MVC 4 kurzu](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Prázdná šablona projektu

MVC prázdný v šabloně projektů je skutečně prázdný teď tak, aby bylo možné spustit z úplně čistou projektem. Starší verze produktu prázdná šablona projektu byl přejmenován na Basic.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Přidat řadič do libovolné složky projektu

Teď můžete klikněte pravým tlačítkem a vyberte **přidat kontroler** z libovolné složky ve vašem projektu MVC. To vám dává větší flexibilitu pro uspořádání řadičů, ale chcete, včetně zachování řadičů MVC a webového rozhraní API v samostatné složky.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Sdružování a minimalizace

Sdružování a minimalizace framework umožňuje snížit počet požadavků HTTP, které webové stránky musí být kombinací jednotlivé soubory do jedné připojené soubor skriptů a šablon stylů CSS. Potom může snížit celkovou velikost těchto požadavků podle zmenšování obsah sady. Zmenšování může zahrnovat činnosti, jako například odstraňuje prázdné znaky na zkrátit názvy proměnných na i sbalení podle jejich sémantiku Selektory šablon stylů CSS. Sady deklarovaný a nakonfigurované v kódu a jsou snadno odkazovaná v zobrazeních prostřednictvím pomocné metody, které lze vygenerovat buď jeden odkaz na sadu nebo při ladění, více odkazuje na jednotlivé obsah sady. Další informace najdete v části [sdružování a Minifikace](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Povolení přihlášení ze sítě Facebook a jiných lokalitách pomocí OAuth a OpenID

Výchozí šablony v šabloně projektu ASP.NET MVC 4 Internet teď zahrnuje podporu pro OAuth a OpenID přihlášení s použitím knihovna DotNetOpenAuth. Informace o konfiguraci zprostředkovatele účtu OAuth nebo OpenID najdete v tématu [účtu OAuth/OpenID podpora WebForms, MVC a webových stránkách](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) a [OAuth a OpenID funkci dokumentace na webových stránkách ASP.NET](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Upgrade projektu aplikace ASP.NET MVC 3 na rozhraní ASP.NET MVC 4

ASP.NET MVC 4 lze nainstalovat node souběžně s ASP.NET MVC 3 na stejném počítači, který poskytuje flexibilitu při volbě, když chcete upgradovat aplikaci ASP.NET MVC 3, ASP.NET MVC 4.

Nejjednodušší způsob, jak upgradovat je vytvoření nového projektu ASP.NET MVC 4 a zkopírujte všechna zobrazení, řadičů, kód a obsah souborů z existující projekt MVC 3 do nového projektu a potom aktualizovat sestavení odkazuje v novém projektu tak, aby odpovídaly žádné šablony bez MVC v Vložená assembiles, který používáte. Pokud jste provedli změny v souboru Web.config v projektu MVC 3, musí také sloučit tyto změny do souboru Web.config v projektu MVC 4.

Pokud chcete ručně upgradovat do verze 4 existující aplikaci ASP.NET MVC 3, postupujte takto:

1. V souboru Web.config. všechny soubory v projektu (je v kořenovém adresáři projektu, jeden ve složce zobrazení a jeden v zobrazení složky pro každou oblast ve vašem projektu), nahraďte všechny výskyty následující text (Poznámka: System.Web.WebPages, verze = 1.0.0.0 nebyl nalezen v projekty vytvořené pomocí sady Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    Následující odpovídající text:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. V kořenovém souboru Web.config, aktualizovat *webPages:Version* element "2.0.0.0" a přidejte nový *PreserveLoginUrl* klíč, který má hodnotu "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. V Průzkumníku řešení klikněte pravým tlačítkem na odkazy a vyberte spravovat balíčky NuGet. V levém podokně vyberte **zdroj oficiální balíčku Online\NuGet**, aktualizujte následující:

    - ASP.NET MVC 4
    - (Volitelné) jQuery, jQuery ověření a jQuery uživatelského rozhraní
    - (Volitelné) Rozhraní Entity Framework
    - (Optonal) Modernizr
4. V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a pak vyberte Uvolnit projekt. Potom znovu klikněte pravým tlačítkem a vyberte Upravit *ProjectName*.csproj.
5. Vyhledejte *ProjectTypeGuids* elementu a nahraďte {E3E379DF-F4C6-4180-9B81-6769533ABE47} {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
6. Uložte změny, zavřete soubor projektu (.csproj), kterou jste upravovali, klikněte pravým tlačítkem na projekt a potom vyberte znovu načíst projekt.
7. Pokud projekt odkazuje na knihovny jakékoli třetí strany, kompilovaná pomocí předchozích verzích rozhraní ASP.NET MVC, otevřete v kořenovém souboru Web.config a přidejte následující tři *bindingRedirect* elementů v rámci  *konfigurace* části: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Změny z verze Release Candidate architektury ASP.NET MVC 4

V poznámkách k verzi ASP.NET MVC 4 Release Candidate naleznete zde:

Hlavní změny z Release Candidate. ASP.NET MVC 4 v této verzi jsou shrnuté níž:

- **Podle konfigurace řadiče:** řadiče rozhraní ASP.NET Web API je spojena s vlastní atribut, který implementuje *IControllerConfiguration* nastavit své vlastní formátování, selektor akcí a parametr vazače . *HttpControllerConfigurationAttribute* byla odebrána.
- **Za trasy obslužné rutiny zpráv:** nyní můžete určit popisovač poslední zpráv v řetězci žádost pro danou trasu. To umožňuje podporu podél pravé architektury, pokud chcete používat směrování k odeslání do svých vlastních (jinou hodnotu než*IHttpController*) koncových bodů.
- **Průběhu oznámení:** *ProgressMessageHandler* generuje oznámení o průběhu pro odesílané entity požadavků i pro stahované entity odpovědí. Pomocí této obslužné rutiny je možné udržovat přehled o tom, jak daleko jsou nahrávání textu žádosti nebo stahování text odpovědi.
- **Push obsah:** *PushStreamContent* třída umožňuje scénáře, kde chce poskytovatel dat zapisovat přímo do požadavku nebo odpovědi (synchronně nebo nesynchronně) pomocí datového proudu. Když *PushStreamContent* je připravena přijímat data, zavolá delegáta akce s do výstupního datového proudu. Vývojář může pak zápisu do datového proudu pro stejně dlouho jako potřebné a zavřít datový proud při zápisu byla dokončena. *PushStreamContent* zjistí ukončovací datového proudu a dokončení základní asynchronní *úloh* pro vypsání obsah.
- **Vytváření chybové odpovědi:** použití *HttpError* typ konzistentně představují informace o chybě z například ověření chyby a výjimky stále respektováním *IncludeErrorDetailPolicy*. Použít novou *CreateErrorResponse* rozšiřující metody snadno vytvářet chybové odpovědi s *HttpError* jako obsah. *HttpError* obsah je plně obsahu vyjednal.
- **Odebrat MediaRangeMapping:** vyjednavač obsahu výchozí teď zpracovává rozsahy typu média.
- **Výchozí parametr vazby parametrů jednoduchého typu je nyní [FromUri]:** v předchozích verzích rozhraní ASP.NET Web API výchozí parametr vazby pro jednoduchý typ. parametry použít vazby modelu. Výchozí parametr vazby parametrů jednoduchého typu je nyní *[FromUri]*.
- **Výběr akce ctí požadované parametry:** výběr akce v rozhraní ASP.NET Web API nyní pouze vybere akci. Pokud jsou k dispozici všechny požadované parametry, které pocházejí z identifikátoru URI. Parametr lze zadat jako volitelné tím, že poskytuje výchozí hodnotu argumentu ve podpis metody akce.
- **Přizpůsobení vazby parametru HTTP:** použít *ParameterBindingAttribute* přizpůsobit vazbu parametru pro parametr určité akce nebo použít *ParameterBindingRules* na *HttpConfiguration* k přizpůsobení vazby parametrů více široce.
- **Vylepšení objekt MediaTypeFormatter:** formátovací moduly mají nyní přístup k kompletní *HttpContent* instance.
- **Ukládání do vyrovnávací paměti výběr zásad hostitele:** implementace a konfigurace *IHostBufferPolicySelector* služby v rozhraní ASP.NET Web API umožňující hostiteli k určení zásad pro ukládání do vyrovnávací paměti po který se má použít.
- **Přístup klientské certifikáty, bez rozpoznání hostitele:** použití *GetClientCertificate* metodu rozšíření k získání zadaný certifikát klienta z zprávu požadavku.
- **Rozšíření vyjednávání obsahu:** odvozování z přizpůsobit vyjednávání obsahu *DefaultContentNegotiator* a přepsáním kteréhokoli aspektu vyjednávání obsahu, který chcete.
- **Podpora pro vracení odpovědí 406 Nepřijatelný:** můžete nyní vrátit 406 Nepřijatelný odpovědi v rozhraní ASP.NET Web API při vytvořením nebyl nalezen vhodný formátovací modul *DefaultContentNegotiator* s *excludeMatchOnTypeOnly* parametr nastaven na *true*.
- **Čtení dat formuláře jako NameValueCollection nebo JToken:** si můžete přečíst v řetězci dotazu identifikátoru URI, nebo v těle žádosti jako data ve formuláři *NameValueCollection* pomocí *ParseQueryString* a  *ReadAsFormDataAsync* rozšiřující metody v uvedeném pořadí. Podobně můžete číst data formuláře v řetězci dotazu identifikátoru URI, nebo v těle žádosti jako *JToken* pomocí *TryReadQueryAsJson* a *ReadAsAsync*&lt;T&gt; rozšiřující metody v uvedeném pořadí.
- **S více částmi vylepšení:** je nyní možné zapsat *MultipartStreamProvider* , je zcela podle typu MIME s více částmi dat, můžete načíst a zobrazit výsledek způsobem optimální pro uživatele. Krok zpracování post lze také připojit na *MultipartStreamProvider* implementace udělat ať následného zpracování je chce na částí textu vícedílné zprávy standardu MIME, který umožňuje. Například *MultipartFormDataStreamProvider* implementace čte data části formuláře HTML a přidá je do *NameValueCollection* tak, aby byly snadno získat v volající.
- **Odkaz vylepšení generování:** *UrlHelper* už závisí na *HttpControllerContext*. Nyní máte přístup *UrlHelper* z jakékoli kontextu kde *HttpRequestMessage* je k dispozici.
- **Změna pořadí spuštění obslužné rutiny zpráv:** obslužné rutiny zpráv jsou nyní spouštěny v pořadí, které jsou nakonfigurované místo v obráceném pořadí.
- **Pomocník pro zapojení do obslužné rutiny zpráv:** nové *HttpClientFactory* , můžete se propojit *DelegatingHandlers* a vytvoření *HttpClient* s jste připraveni... požadované kanál. Také poskytuje funkce pro zapojení až s alternativní vnitřní obslužné rutiny (výchozí hodnota je *HttpClientHandler*) a také udělá spojení při použití *HttpMessageInvoker* nebo jiný  *DelegatingHandler* místo *HttpClient* jako původce volání top.
- **Podpora pro sítím CDN v optimalizaci webů ASP.NET:** optimalizaci webů ASP.NET teď poskytuje podporu pro sady další adresu URL, která odkazuje na stejný prostředku v síti pro doručování obsahu CDN umožňuje pro každé zadejte alternativní cesty. Podpora sítím CDN umožňuje získat váš skript a stylu sady geograficky blíže k příjemce end webových aplikací. Když není k dispozici CDN produkční aplikace by měla implementovat zálohu. Test záložní.
- **Rozhraní ASP.NET Web API tras a konfigurace přesunut do *WebApiConfig.Register* statickou metodu, která může být resused v testovací kód.** Rozhraní ASP.NET Web API trasy byly dříve přidány v *RouteConfig.RegisterRoutes* společně s standardní MVC tras. Výchozí směrování ASP.NET Web API a konfigurace jsou nyní zpracovávány v samostatném *WebApiConfig.Register* metoda usnadňuje testování.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a nejnovější změny

- **Verze RC a RTM technologie ASP.NET MVC 4 nesprávně vrátit zobrazení plochy v mezipaměti, pokud má být vrácen mobilní zobrazení.**

    - V tématu [ASP.NET MVC 4 Mobile ukládání do mezipaměti chyb pevné](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) podrobnosti o opravu. Je možné nainstalovat opravu z [pevné DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) balíček NuGet.
- **Nejnovější změny ve zobrazovací modul Razor**. Následující typy byly odebrány z *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Také byly odebrány následující metody: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Když WebMatrix.WebData.dll je zahrnutý v adresáři/Bin aplikace ASP.NET MVC 4, přebírá adresu URL pro ověřování pomocí formulářů.** Přidání WebMatrix.WebData.dll sestavení do vaší aplikace (například výběrem "ASP.NET Web Pages se syntaxí Razor" při použití dialogové okno Přidat nasaditelné závislosti) se přepíše přihlášení přesměrování ověřování/účet/Logon místo nebo účet nebo přihlášení očekávaným ve výchozím nastavení technologie ASP.NET MVC jsou řadič MVC účtu. Pokud chcete zakázat toto chování a používat zadaná adresa URL již v části ověření souboru Web.config, můžete přidat appSetting názvem PreserveLoginUrl a nastavte ji na hodnotu true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Správce balíčků NuGet se nepodaří nainstalovat při pokusu o instalaci ASP.NET MVC 4 pro souběžné instalace sady Visual Studio 2010 a program Visual Web Developer 2010.** Ke spuštění sady Visual Studio 2010 a program Visual Web Developer 2010 node souběžně s ASP.NET MVC 4 je nutné nainstalovat ASP.NET MVC 4 po obě verze sady Visual Studio již byl nainstalován.
- **Odinstalace ASP.NET MVC 4 selže, pokud již byl odinstalován požadavky.** Chcete-li řádně odinstalovat ASP.NET MVC musí 4you odinstalovat ASP.NET MVC 4 před odinstalací nástroje Visual Studio.
- **Instalace technologie ASP.NET MVC 4 dělí aplikace ASP.NET MVC 3 RTM.** Verze aplikací ASP.NET MVC 3, které se vytvořily s verzi RTM (s [ASP.NET MVC 3 nástroje aktualizace](https://www.microsoft.com/download/details.aspx?id=1491) vydání) vyžaduje následující změny fungování souběžného s ASP.NET MVC 4. Vytváření projektu bez provedení těchto výsledků aktualizací k chybám kompilace. 

    **Požadované aktualizace**

  1. V kořenovém souboru Web.config, přidejte nový *&lt;appSettings&gt;* položka s klíčem *webPages:Version* a hodnotu *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a pak vyberte Uvolnit projekt. Potom znovu klikněte pravým tlačítkem a vyberte Upravit *ProjectName*.csproj.
  3. Vyhledejte následující odkazy na sestavení: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Nahraďte je následující:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Uložte změny, zavřete soubor projektu (.csproj) byly úpravy a potom klikněte pravým tlačítkem na projekt a vyberte znovu načíst.

- **Změna projektu ASP.NET MVC 4 k cíli 4.0, 4.5 nelze aktualizovat odkaz na sestavení EntityFramework:** Pokud změníte projektu ASP.NET MVC 4 k cíli 4.0 po které se budou zaměřovat 4.5 odkaz na sestavení EntityFramework budou i nadále odkazovat verze 4.5. Chcete-li vyřešit tento problém odinstalovat a znovu nainstalujte balíček EntityFramework NuGet.
- **403 Zakázáno při spuštění aplikace ASP.NET MVC 4 v Azure po změně cíl 4.0, 4.5:** Pokud změníte projektu ASP.NET MVC 4 k cíli 4.0, po které se budou zaměřovat 4.5 a nasadit do Azure, mohou se zobrazit chybou 403 Zakázáno za běhu. Chcete-li vyřešit tento problém, přidejte následující do souboru web.config: `<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012, dojde k chybě při zadávání '\' v řetězcový literál v souboru nástroje Razor.** Postup řešení problému zadejte uvozovky literálu řetězce první.
- <strong>Procházení k &quot;účet nebo spravovat&quot; ve výsledcích Internet šablony v chybě modulu runtime pro jazyky, CHS, TRK a CHT.</strong> Vyřešte problém upravit stránce oddělit <em>@User.Identity.Name</em> umístěním jako jediný obsah v rámci <em>&lt;silné&gt;</em> značky.
- **Nepodporují se zprostředkovatelé Google a LinkedIn v rámci webů Azure.** Zprostředkovatelé alternativní ověřování používejte při nasazování na weby Azure.
- **Při použití UriPathExtensionMapping 8 služby IIS Express nebo IIS, by se při pokusu o použití rozšíření přijímat chyb 404 nebyl nalezen.** Obslužné rutiny statických souborů koliduje s požadavky na webové rozhraní API, která použít *UriPathExtensionMappings*. Nastavit *runAllManagedModulesForAllRequests = true* v souboru web.config. Chcete-li vyřešit tento problém.
- **Už je volána metoda Controller.Execute.** Všechny řadiče MVC jsou nyní vždycky spustí asynchronně.
