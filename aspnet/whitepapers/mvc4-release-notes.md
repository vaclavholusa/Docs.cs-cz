---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument popisuje verzi technologie ASP.NET MVC 4.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: a4f78061850ef5ad8c3381daafdb5ea6bca4cb2f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752724"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Tento dokument popisuje verzi technologie ASP.NET MVC 4.


- [Poznámky k instalaci](#_Toc303253802)
- [Dokumentace](#_Toc303253803)
- [Podpora](#_Toc303253804)
- [Požadavky na software](#_Toc303253805)
- [Nové funkce v architektuře ASP.NET MVC 4](#_Toc303253807)

    - [Webové rozhraní API v ASP.NET](#_Toc317096197)
    - [Vylepšení pro výchozí šablony projektu](#_Toc303253808)
    - [Šablona projektu mobilní](#_Toc303253809)
    - [Režimy zobrazení](#_Toc303253810)
    - [jQuery Mobile, přepínání zobrazení a přepsání prohlížeče](#_Toc303253811)
    - [Podpora úkol pro asynchronní řadiče](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Migrace databází](#_Toc303253818)
    - [Prázdná šablona projektu](#_Toc303253819)
    - [Přidat kontroler do libovolné složky projektu](#_Toc303253820)
    - [Vytváření sady a minifikace](#_Toc303253821)
    - [Povolení přihlášení z Facebooku a dalších lokalit pomocí OAuth a OpenID](#_Toc303253822)
- [Upgrade projektu aplikace ASP.NET MVC 3 na ASP.NET MVC 4](#_Toc303253806)
- [Změny v architektuře ASP.NET MVC 4 Release Candidate](#_Toc303253817)
- [Známé problémy a změny způsobující chyby](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Poznámky k instalaci

ASP.NET MVC 4 pro Visual Studio 2010 můžete nainstalovat pomocí [domovskou stránku ASP.NET MVC 4](../mvc/mvc4.md) pomocí instalačního programu webové platformy.

Doporučujeme, abyste odinstalace všechny dříve nainstalované verze Preview architektury ASP.NET MVC 4 před instalací technologie ASP.NET MVC 4. ASP.NET MVC 4 Beta a verze Release Candidate můžete upgradovat na ASP.NET MVC 4 bez odinstalování.

Tato verze není kompatibilní s vydané verze preview rozhraní .NET Framework 4.5. Samostatně je nutné upgradovat všechny nainstalované ve verzi preview verze rozhraní .NET Framework 4.5 na finální verzi před instalací technologie ASP.NET MVC 4.

ASP.NET MVC 4, může být nainstalovaná a spuštění – souběžně v architektuře ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentace

Dokumentace k ASP.NET MVC je k dispozici na webu MSDN na následující adrese URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Kurzy a další informace o architektuře ASP.NET MVC jsou k dispozici na stránce na webu technologie ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Podpora

ASP.NET MVC 4 je plně podporováno. Pokud máte dotazy týkající se práce v této verzi můžete také publikovat je do fóra ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), kde jsou často schopni poskytovat podporu neformální členové komunity technologie ASP.NET.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Požadavky na software

Komponenty architektury ASP.NET MVC 4 pro Visual Studio vyžadují prostředí PowerShell 2.0 a Visual Studio 2010 s aktualizací Service Pack 1 nebo Visual Web Developer Express 2010 s aktualizací Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Nové funkce v architektuře ASP.NET MVC 4

Tato část popisuje funkce, které se zavedly ve verzi ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>Rozhraní API pro ASP.NET Web

ASP.NET MVC 4 zahrnuje webové rozhraní API ASP.NET, nové rozhraní pro vytváření služeb HTTP, které mohou být využity širokou škálou klientů včetně prohlížečů a mobilních zařízení. ASP.NET Web API je také ideální platformu pro vytváření služby typu REST.

Rozhraní ASP.NET Web API zahrnuje podporu pro následující funkce:

- **Moderní programovací model HTTP:** přímo přistupovat k a manipulaci s žádostí HTTP a odpovědí v rozhraní Web API s využitím nové, silného typu HTTP objektový model. Stejné programovací model a kanálu protokolu HTTP je symetricky dostupný na klientovi, prostřednictvím nového *HttpClient* typu.
- **Plnou podporu pro trasy:** rozhraní ASP.NET Web API podporuje kompletní sadu funkcí trasu směrování ASP.NET, včetně parametry trasy a omezení. Kromě toho pomocí jednoduchého konvence mapování akcí na metody HTTP.
- **Vyjednávání obsahu:** klienta a serveru můžou spolupracovat a určit správný formát pro data se vrací z webového rozhraní API. Rozhraní ASP.NET Web API poskytuje výchozí podporu pro XML, JSON, a kódovaná adresou URL formuláře formátů a mohou rozšířit tuto podporu tak, že přidáte vlastní formátovací moduly, nebo dokonce nahrazujte výchozí strategie vyjednávání obsahu.
- **Vazby modelu a ověření:** vazače modelů poskytují snadný způsob, jak extrahovat data z různých částí požadavek HTTP a převést tyto části zprávy do objektů .NET, které mohou být využívána akcí webového rozhraní API. Ověření se provádí také na parametry akce založené na datových poznámek.
- **Filtry:** rozhraní ASP.NET Web API podporuje filtry, včetně dobře známých filtry, jako *[Authorize]* atribut. Můžete vytvářet a zapojte vlastní filtry pro akce a povolení zpracování výjimek.
- **Vytváření dotazu:** použití *[Queryable]* atribut filtru na akci, která vrátí *IQueryable* povolení podpory pro dotazování na vaše webové rozhraní API prostřednictvím konvence prostředí OData pro dotazování.
- **Vylepšené testovatelnost:** místo nastavení podrobností protokolu HTTP v objektech statického kontextu, webové rozhraní API pracovní akce s instancemi *HttpRequestMessage* a *objekt HttpResponseMessage*. Vytvořte projekt testu jednotek spolu s projekt webového rozhraní API tak, aby mohli začít rychle zápis testů jednotek pro vaše webového rozhraní API funkce.
- **Konfigurace založená na kódu:** konfigurace ASP.NET Web API se provádí pouze pomocí kódu, vyčistit byste museli opustit váš konfigurační soubory. Pomocí vzoru Lokátor poskytované služby můžete nakonfigurovat body rozšiřitelnosti.
- **Vylepšená podpora pro kontejnery ovládacích IOC (Inversion):** rozhraní ASP.NET Web API poskytuje skvělou podporu pro technologie IoC kontejnerů prostřednictvím abstrakce překladač závislostí vylepšené
- **Hostování na vlastním serveru:** webová rozhraní API je možné hostovat ve vašem vlastním procesu kromě IIS stále přitom výkon trasy a dalších funkcích služby webového rozhraní API.
- **Vlastní Nápověda k vytvoření a testování stránek:** teď můžete snadno vytvářet vlastní nápovědy a testovat stránky pro vaše webového rozhraní API pomocí nových *IApiExplorer* služby runtime úplný popis vašeho webového rozhraní API.
- **Monitorování a Diagnostika:** rozhraní ASP.NET Web API nyní infrastrukturu lehký trasování, která umožňuje snadno integrovat se stávajícími řešeními protokolování, jako je například System.Diagnostics, trasování událostí pro Windows a jiných rozhraní protokolování. Můžete povolit trasování tím, že poskytuje *ITraceWriter* provádění a jeho přidání do konfigurace webového rozhraní API.
- **Propojit generování:** pomocí rozhraní ASP.NET Web API *UrlHelper* ke generování odkazů k souvisejícím prostředkům ve stejné aplikaci.
- **Šablona projektu webového rozhraní API:** vyberte nový formulář průvodce Nový projekt MVC 4 rychle začít pracovat s rozhraním ASP.NET Web API projekt webového rozhraní API.
- **Generování uživatelského rozhraní:** použití **přidat kontroler** založené na dialogu rychle scaffold kontroleru webového rozhraní API založené na Entity Framework typ modelu.

Další informace o rozhraní ASP.NET Web API najdete [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Vylepšení pro výchozí šablony projektu

Jak vytvořit web více moderního vzhledu byl aktualizován šablonu, která se používá k vytvoření nových projektech ASP.NET MVC 4:

![](mvc4-release-notes/_static/image1.png)

Kromě kosmetické vylepšení obsahuje vylepšené funkce v nové šablony. Šablona využívá techniky označované jako adaptivního vykreslování do vypadat dobře v počítačových prohlížečů a mobilní prohlížeče bez jakéhokoli přizpůsobení.

![](mvc4-release-notes/_static/image2.png)

Zobrazíte adaptivního vykreslování v akci, můžete použít mobilní emulátor nebo zkuste použít mění velikost okna prohlížeči v počítači. Chcete-li být menší. Pokud okno prohlížeče získá dostatečně malá, se změní rozložení stránky.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Šablona projektu mobilní

Pokud začíná nový projekt a chcete vytvořit web speciálně pro mobilní zařízení a prohlížečů tablet, můžete použít novou šablonu projektu mobilní aplikace. To je založené na technologii jQuery Mobile, knihovny open source pro vytváření optimalizovaných pro dotykové ovládání uživatelského rozhraní:

![](mvc4-release-notes/_static/image3.png)

Tato šablona obsahuje stejnou strukturu aplikace jako šablony internetovou aplikaci (a kódu kontroleru je prakticky totožný), ale je ve stylu pomocí jQuery Mobile vypadat dobře a chovat i na mobilních zařízeních s dotykovým ovládáním. Další informace o tom, jak strukturovat a stylu mobilní uživatelského rozhraní, najdete v článku [jQuery Mobile projektu webu](http://jquerymobile.com/).

Pokud už máte orientované na ploše lokality, které chcete přidat zobrazení optimalizovaných pro mobilní zařízení nebo pokud chcete vytvořit jeden web, který slouží jinak upravený zobrazení pro stolní počítače a mobilní prohlížeče, můžete novou funkci režimů zobrazení. (Viz další části.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Režimy zobrazení

Nová funkce režimů zobrazení umožní aplikaci vyberte zobrazení v závislosti na prohlížeči, který provádí požadavek. Například pokud prohlížeč pro stolní počítač požadavků na domovskou stránku, aplikaci použít Views\Home\Index.cshtml šablony. Pokud mobilního prohlížeče požadavků na domovskou stránku, aplikace může vrátit Views\Home\Index.mobile.cshtml šablony.

Rozložení a částečných zobrazení lze také přepsat u určitého prohlížeče typů. Příklad:

- Pokud obsahuje složku Views\Shared \_Layout.cshtml a \_Layout.mobile.cshtml šablony, ve výchozím nastavení bude aplikace používat \_Layout.mobile.cshtml během žádosti od mobilních prohlížečů a \_Layout.cshtml během dalších požadavků.
- Pokud složka obsahuje oba \_MyPartial.cshtml a \_MyPartial.mobile.cshtml instrukce @Html.Partial("\_MyPartial") bude vykreslení \_MyPartial.mobile.cshtml během požadavků z mobile prohlížeče, a \_MyPartial.cshtml během dalších požadavků.

Pokud chcete vytvořit podrobnější zobrazení, rozložení nebo částečná zobrazení pro další zařízení, můžete zaregistrovat nový *DefaultDisplayMode* instance k určení, které název vyhledávat požadavek splňuje určité podmínky. Můžete například přidat následující kód, který *aplikace\_Start* metody v souboru Global.asax pro registraci řetězec "iPhone" jako režim zobrazení, která se použije, když prohlížeč Apple iPhone odešle požadavek:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Po spuštění tohoto kódu, pokud prohlížeče pro iPhone Apple odešle požadavek, bude aplikace používat Views\Shared\\_Layout.iPhone.cshtml rozložení (pokud existuje). Další informace o režim zobrazení, naleznete v tématu [funkcí technologie ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Nainstalujte aplikací s použitím DisplayModeProvider [pevné DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) balíček NuGet. [ASP.NET Fall 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322) zahrnuje [pevné DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) balíčku NuGet, nové projektové šablony. Zobrazit [ASP.NET MVC 4 Mobile ukládání do mezipaměti chyb Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) podrobnosti o opravu.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile a mobilní funkce

Informace o sestavení mobilní aplikace s ASP.NET MVC 4 pomocí jQuery Mobile, najdete v kurzu [funkcí technologie ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Podpora úkol pro asynchronní řadiče

Teď můžete psát asynchronní akce metody jako jedné metody, které vracejí objekt typu *úloh* nebo *úloh&lt;ActionResult&gt;*.

 Další informace najdete v části [použití asynchronních metod v architektuře ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 podporuje verzi 1.6 a novějších verzích Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migrace databází

Projekty ASP.NET MVC 4 teď obsahují Entity Framework 5. Jednou z nejlepších funkcí v Entity Framework 5 je podpora migracemi databází. Tato funkce umožňuje snadno vyvíjet schématu databáze pomocí migrace zaměřený na kód při zachování dat v databázi. Další informace o migrace databází, naleznete v tématu [přidání nového pole do modelu a tabulky Movie](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) v [Úvod do ASP.NET MVC 4 kurzu](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Prázdná šablona projektu

MVC prázdná šablona projektu je skutečně prázdný nyní tak, aby bylo možné spustit z zcela čisté břidlicová. V předchozích verzích šablonu prázdného projektu byla přejmenována na Basic.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Přidat kontroler do libovolné složky projektu

Teď klikněte pravým tlačítkem a vyberte **přidat kontroler** z libovolné složky ve vašem projektu MVC. To poskytuje větší flexibilitu při uspořádání vašich řadičů představ, včetně udržování vašeho řadiče MVC a webového rozhraní API do samostatné složky.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Sdružování a Minifikace

Sdružování a minifikace rozhraní můžete snížit počet požadavků HTTP, které je potřeba na webové stránce díky kombinaci jednotlivých souborů do jedné sady souborů pro skripty a CSS. Potom může snížit celkovou velikost těchto požadavků podle minifikace obsah sady. Minifikace může obsahovat aktivity, jako jsou odstranění zkrácení názvy proměnných i sbalení selektorů CSS podle jejich sémantiku prázdné znaky. Sady jsou deklarovány a konfigurovat v kódu a jsou snadno odkazovat v zobrazeních prostřednictvím pomocné metody, které můžete buď vygenerovat jediný odkaz na sadu nebo při ladění, více odkazuje na jednotlivé obsah sady. Další informace najdete v části [sdružování a Minifikace](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Povolení přihlášení z Facebooku a dalších lokalit pomocí OAuth a OpenID

Výchozí šablony v šabloně projektu ASP.NET MVC 4 Internet teď zahrnuje podporu pro přihlášení OAuth a OpenID pomocí knihovna DotNetOpenAuth. Informace o konfiguraci zprostředkovatele účtu OAuth nebo OpenID, naleznete v tématu [podpora účtu OAuth/OpenID pro webové formuláře, MVC a webových stránkách](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) a [OAuth a OpenID funkcí dokumentace k ASP.NET Web Pages](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Upgrade projektu aplikace ASP.NET MVC 3 na ASP.NET MVC 4

ASP.NET MVC 4 je nainstalovat souběžně s ASP.NET MVC 3 na stejném počítači, který poskytuje flexibilitu při výběru, kdy se má upgradovat aplikaci ASP.NET MVC 3 na ASP.NET MVC 4.

Nejjednodušší způsob, jak upgradovat je k vytvoření nového projektu ASP.NET MVC 4 a zkopírujte všechny zobrazení, kontrolery, kódu a obsah souborů z existující projekt MVC 3 na nový projekt a potom aktualizovat sestavení odkazuje v novém projektu tak, aby odpovídaly všechny šablony mimo MVC v Vložená assembiles, který používáte. Pokud jste provedli změny v souboru Web.config v projektu MVC 3, musíte také sloučit tyto změny do souboru Web.config v projekt MVC 4.

Ručně upgradovat existující aplikaci ASP.NET MVC 3 na verzi 4, postupujte takto:

1. V souboru Web.config všechny soubory v projektu (je v kořenovém adresáři projektu, jeden v zobrazení složky a druhý ve složce zobrazení pro každou oblast, ve vašem projektu), nahraďte každou instanci následující text (Poznámka: System.Web.WebPages, verze = 1.0.0.0 nebyl nalezen v projekty vytvořené pomocí sady Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    s odpovídající následujícím textem:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. V kořenovém souboru Web.config aktualizujte *webPages:Version* element na "2.0.0.0" a přidejte nový *PreserveLoginUrl* klíč, který má hodnotu "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. V Průzkumníku řešení klikněte pravým tlačítkem myši na odkazy a vyberte spravovat balíčky NuGet. V levém podokně vyberte **zdroj balíčku oficiální Online\NuGet**, aktualizujte následující:

    - ASP.NET MVC 4
    - (Volitelné) jQuery, jQuery ověření a uživatelské rozhraní jQuery
    - (Volitelné) Entity Framework
    - (Optonal) Modernizr
4. V Průzkumníku řešení klikněte pravým tlačítkem myši na název projektu a pak vyberte Uvolnit projekt. Klikněte pravým tlačítkem na název a vyberte Upravit *ProjectName*csproj.
5. Vyhledejte *ProjectTypeGuids* prvku a nahraďte {E53F8FEA-EAE0-44A6-8774-FFD645390401} s {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Změny uložte, zavřete soubor projektu (.csproj), který jste dříve upravovali, klikněte pravým tlačítkem na projekt a poté vyberte znovu načíst projekt.
7. Pokud projekt odkazuje na žádné knihovny třetích stran, které jsou kompilovány pomocí předchozí verze technologie ASP.NET MVC, otevřete v kořenovém souboru Web.config a přidat následující tři *bindingRedirect* prvků  *konfigurace* části: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Změny v architektuře ASP.NET MVC 4 Release Candidate

Zpráva k vydání verze pro ASP.NET MVC 4 Release Candidate najdete tady:

Z ASP.NET MVC 4 Release Candidate hlavní změny v této verzi jsou shrnuté dole:

- **Za konfiguraci kontroleru:** kontrolerů rozhraní Web API ASP.NET může být přiděleny s vlastní atribut, který implementuje *IControllerConfiguration* nastavit vlastní formátovací moduly, selektor akcí a parametr vazače . *HttpControllerConfigurationAttribute* byla odebrána.
- **Podle postupu obslužné rutiny zpráv:** teď můžete zadat popisovač poslední zprávy v řetězci žádost pro danou trasu. To umožňuje podporu pro jízdní podél platformy, které používají k odeslání do své vlastní směrování (jinou hodnotu než*IHttpController*) koncových bodů.
- **Oznámení o průběhu:** *ProgressMessageHandler* generuje oznámení o průběhu pro odesílané entity požadavků i pro stahované entity odpovědí. Pomocí této rutiny je možné sledovat, jak daleko se nahrávání tělo požadavku nebo stahování tělo odpovědi.
- **Vložit obsah:** *PushStreamContent* třída umožňuje scénáře, kdy chce poskytovatel dat psát přímo do požadavku nebo odpovědi (synchronně nebo nesynchronně) pomocí datového proudu. Když *PushStreamContent* je připravena přijímat data volání delegáta akce s výstupního datového proudu. Vývojář poté můžete napsat do datového proudu pro dlouho podle potřeby a zavřít datový proud při psaní se dokončilo. *PushStreamContent* zjistí zavření datového proudu a dokončení asynchronní základní *úloh* pro vypsání obsahu.
- **Vytváření chybové odpovědi:** použití *HttpError* typ konzistentně představující informace o chybě z například výjimek a chyb při ověřování stále respektováním *IncludeErrorDetailPolicy*. Pomocí nové *CreateErrorResponse* metody rozšíření můžete snadno vytvořit chybové odpovědi s *HttpError* jako obsah. *HttpError* obsah je plně obsahu vyjedná.
- **Odebrat MediaRangeMapping:** vyjednavače obsahu výchozí nyní zpracovává rozsahy typu média.
- **Výchozí parametr vazby parametrů jednoduchého typu, je teď [FromUri]:** v předchozích verzích rozhraní ASP.NET Web API výchozí parametr vazby pro jednoduchý typ. parametry používá vazbu modelu. Výchozí parametr vazby parametrů jednoduchého typu, je teď *[FromUri]*.
- **Výběr akce respektuje požadované parametry:** výběr akce v rozhraní ASP.NET Web API nyní pouze vybere akci Pokud jsou k dispozici všechny požadované parametry, které pochází z identifikátoru URI. Parametr lze zadat jako volitelné tím, že poskytuje výchozí hodnota pro argument v podpisu metody akce.
- **Upravit vazby parametru HTTP:** použít *ParameterBindingAttribute* přizpůsobit vazbu parametru pro parametr určité akce nebo použít *ParameterBindingRules* na *HttpConfiguration* přizpůsobení vazby parametrů šířeji.
- **Vylepšení objekt MediaTypeFormatter:** formátovací moduly mají nyní přístup k úplné *HttpContent* instance.
- **Výběr zásad ukládání do vyrovnávací paměti hostitele:** implementovat a konfigurovat *IHostBufferPolicySelector* službu v rozhraní ASP.NET Web API umožňuje hostiteli k určení zásad pro ukládání do vyrovnávací paměti po který se má použít.
- **Přístup ke klientským certifikátům v hostiteli certifikátům:** použití *GetClientCertificate* metodu rozšíření k získání zadaný certifikát klienta ze zprávy požadavku.
- **Rozšíření vyjednávání obsahu:** upravit vyjednávání obsahu tak, že odvozený od *DefaultContentNegotiator* a přepsání jiného aspektu vyjednávání obsahu, kterou byste uvítali.
- **Podpora pro vracení odpovědí na 406 Nepřijatelný:** můžete nyní vracet 406 Nepřijatelný odpovědi v rozhraní ASP.NET Web API. když nenajde vhodný formátovací modul tak, že vytvoříte *DefaultContentNegotiator* s *excludeMatchOnTypeOnly* parametr nastaven na *true*.
- **Čtení dat formuláře jako kolekce NameValueCollection nebo JToken:** si můžete přečíst data formuláře v řetězci dotazu identifikátoru URI nebo v textu žádosti jako *NameValueCollection* pomocí *ParseQueryString* a  *ReadAsFormDataAsync* metody rozšíření v uvedeném pořadí. Podobně můžete číst data formuláře v řetězci dotazu identifikátoru URI nebo v textu žádosti jako *JToken* pomocí *TryReadQueryAsJson* a *ReadAsAsync*&lt;T&gt; metody rozšíření v uvedeném pořadí.
- **Vylepšení vícedílné zprávy standardu:** je teď možné zapisovat *MultipartStreamProvider* , který je zcela přizpůsobená pro typ dat vícedílné zprávy standardu MIME, že může číst a zobrazit výsledek optimální způsobem, který uživatel. Následné zpracování kroku lze také připojit na *MultipartStreamProvider* , který umožňuje implementovat provedení jakýkoli příspěvek zpracování chce na částí textu vícedílné zprávy standardu MIME. Například *MultipartFormDataStreamProvider* implementace přečte formulář HTML části dat a přidá je do *NameValueCollection* tak, aby byly snadno získat na od volajícího.
- **Propojit vylepšení generování:** *UrlHelper* už závisí na *HttpControllerContext*. Nyní máte přístup *UrlHelper* z jakýkoli kontext kde *HttpRequestMessage* je k dispozici.
- **Změna pořadí provádění obslužné rutiny zpráv:** obslužné rutiny zpráv jsou nyní provedeny v pořadí, ve kterém jsou nakonfigurované místo v obráceném pořadí.
- **Pomocník pro zapojení do obslužné rutiny zpráv:** nové *HttpClientFactory* , která můžete nastavit *DelegatingHandlers* a vytvoření *HttpClient* s jste připravení začít požadované kanálu. Také poskytuje funkce pro její nahoru s alternativní vnitřní obslužné rutiny (výchozí hodnota je *HttpClientHandler*) stejně jako provádět vedení pomocí *HttpMessageInvoker* nebo jiném  *DelegatingHandler* místo *HttpClient* jako původce volání nahoře.
- **Podpora CDN v optimalizaci webů ASP.NET:** optimalizaci webů ASP.NET teď poskytuje podporu pro CDN alternativní cesty umožňuje zadat pro každou sadu další adresa URL, která odkazuje na tento stejný prostředek na síť pro doručování obsahu. Podpora CDN umožní, abyste váš skript a styl sady blíž koncovým uživatelům webových aplikací. Produkční aplikace by měla implementovat záložní CDN není k dispozici. Testování na náhradní řešení.
- **Trasy rozhraní ASP.NET Web API a konfigurace se přesunula do *WebApiConfig.Register* statická metoda, která může být resused v kódu testu.** Trasy rozhraní ASP.NET Web API byly dříve přidány v *RouteConfig.RegisterRoutes* spolu s standardní MVC trasy. Výchozí trasy rozhraní Web API ASP.NET a konfigurace nyní provádí v samostatném *WebApiConfig.Register* metoda usnadňuje testování.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a změny způsobující chyby

- **Verze RC a RTM technologie ASP.NET MVC 4 nesprávně vrátí zobrazení uložené v mezipaměti klientů při mobilní zobrazení má být vrácen.**

    - Zobrazit [ASP.NET MVC 4 Mobile ukládání do mezipaměti chyby opravené](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) podrobnosti o opravu. Oprava si můžete nainstalovat pomocí [pevné DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) balíček NuGet.
- **Rozbíjející změny v zobrazovací modul Razor**. Následující typy byly odebrány z *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Také byly odstraněny následující metody: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **WebMatrix.WebData.dll je obsažen v adresáři/Bin aplikací technologie ASP.NET MVC 4, převezme adresu URL pro ověřování pomocí formulářů.** Přidání WebMatrix.WebData.dll sestavení pro vaši aplikaci (například tak, že vyberete "ASP.NET Web Pages se syntaxí Razor" při použití dialogového okna Přidat Nasaditelných závislostí) se přepíše přihlášení přesměrování ověřování/účet/Logon spíše než / / přihlášení k účtu očekávaným ve výchozím nastavení kontroler ASP.NET MVC účtu. K tomuto chování zabráníte a použijte adresu URL již definováno v sekci ověřování souboru Web.config, můžete přidat nastavení aplikace volá PreserveLoginUrl a nastavte ho na hodnotu true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Správce balíčků NuGet se nepodařilo nainstalovat při pokusu o instalaci technologie ASP.NET MVC 4 pro souběžné instalace sady Visual Studio 2010 a Visual Web Developer 2010.** Ke spuštění sady Visual Studio 2010 a Visual Web Developer 2010 souběžně s ASP.NET MVC 4 je nutné nainstalovat technologie ASP.NET MVC 4 po již byly nainstalovány obě verze systému Visual Studio.
- **Odinstalování technologie ASP.NET MVC 4 selže, pokud již byly odinstalovány požadavky.** Čistě odinstalování technologie ASP.NET MVC 4you odinstalujte před odinstalací sady Visual Studio ASP.NET MVC 4.
- **Instalace technologie ASP.NET MVC 4 dělí aplikace ASP.NET MVC 3 RTM.** Vydání aplikace ASP.NET MVC 3, které byly vytvořeny verzi RTM (ne s [ASP.NET MVC 3 nástroje Update](https://www.microsoft.com/download/details.aspx?id=1491) release) vyžaduje následující změny fungování souběžně s ASP.NET MVC 4. Vytváření projektu bez provedení těchto aktualizací výsledky v chyby kompilace. 

    **Požadované aktualizace**

  1. V kořenovém souboru Web.config přidejte nový *&lt;appSettings&gt;* položka s klíčem *webPages:Version* a hodnota *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. V Průzkumníku řešení klikněte pravým tlačítkem myši na název projektu a pak vyberte Uvolnit projekt. Klikněte pravým tlačítkem na název a vyberte Upravit *ProjectName*csproj.
  3. Vyhledejte následující odkazy na sestavení: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Nahraďte je následující:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Změny uložte, zavřete soubor projektu (.csproj) dříve upravovali a pak klikněte pravým tlačítkem na projekt a vyberte znovu načíst.

- **Změna projektu ASP.NET MVC 4 do cíle 4.0 4.5 nelze aktualizovat odkaz na sestavení EntityFramework:** Pokud změníte projektu ASP.NET MVC 4 do cíle 4.0 po odkaz na sestavení objektu EntityFramework, se stále odkazují na která cílí 4.5 verze 4.5. Chcete-li vyřešit tento problém odinstalovat a znovu nainstalujte balíček NuGet objektu EntityFramework.
- **403 Zakázáno při spuštění aplikace ASP.NET MVC 4 v Azure po změně na cíl 4.0, 4.5:** Pokud změníte projektu ASP.NET MVC 4 do cíle 4.0 po zaměřen 4.5 a pak nasadit do Azure, může se zobrazit Chyba 403 Zakázáno v době běhu. Chcete-li vyřešit tento problém, přidejte následující text do souboru web.config: `<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012, dojde k chybě při zadávání "\' v řetězcovém literálu v souboru Razor.** Postup řešení problému zadejte první pravou uvozovku řetězcového literálu.
- <strong>Přejdete na adresu &quot;účtu/Správa&quot; ve výsledcích šablony Internet chybu modulu runtime pro jazykovou sadou CHS, rev a CHT jazyky.</strong> Chcete-li vyřešit tento problém upravte stránku k tomu oddělit <em>@User.Identity.Name</em> tak, že ji vložíte jako jediný obsah v rámci <em>&lt;silné&gt;</em> značky.
- **Zprostředkovatelé Google a LinkedIn se nepodporují v rámci webů Azure.** Použití poskytovatelů alternativní ověření při nasazování na weby Azure.
- **Při použití UriPathExtensionMapping 8 služby IIS Express nebo IIS, by se zobrazí chyby 404 Nenalezeno, při pokusu o použití rozšíření.** Obslužná rutina statického souboru koliduje s požadavky do webového rozhraní API, která pomocí *UriPathExtensionMappings*. Nastavte *runAllManagedModulesForAllRequests = true* v souboru web.config pro tento problém obejít.
- **Už je volána metoda Controller.Execute.** Všechny kontrolery MVC jsou teď vždy spuštěn asynchronně.
