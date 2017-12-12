---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Tento dokument popisuje verzi technologie ASP.NET MVC 4 Beta pro Visual Studio 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 4af2df61ab4507b1f100d6bb75777da1168c5a75
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Tento dokument popisuje verzi technologie ASP.NET MVC 4 Beta pro Visual Studio 2010.
> 
> > [!NOTE]
> > Toto není nejnovější verze. Jsou k dispozici v poznámkách k verzi ASP.NET MVC 4 RC [zde](mvc4-release-notes.md).


- [Poznámky k instalaci](#_Toc303253802)
- [Dokumentace](#_Toc303253803)
- [Podpora](#_Toc303253804)
- [Požadavky na software](#_Toc303253805)
- [Upgrade projektu aplikace ASP.NET MVC 3 na rozhraní ASP.NET MVC 4](#_Toc303253806)
- [Nové funkce ve verzi Beta architektury ASP.NET MVC 4](#_Toc303253807)

    - [Rozhraní ASP.NET Web API](#_Toc317096197)
    - [Aplikace ASP.NET jedné stránky](#_Toc317096198)
    - [Vylepšení výchozí šablony projektů](#_Toc303253808)
    - [Šablona projektu mobilní](#_Toc303253809)
    - [Režimy zobrazení](#_Toc303253810)
    - [jQuery Mobile, přepínači zobrazení a přepsání prohlížeče](#_Toc303253811)
    - [Recepty pro generování kódu v sadě Visual Studio](#_Toc303253812)
    - [Podpora úloh pro asynchronní řadiče](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Známé problémy a nejnovější změny](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Poznámky k instalaci

ASP.NET MVC 4 Beta pro Visual Studio 2010 můžete nainstalovat z [domovskou stránku ASP.NET MVC 4](../mvc/mvc4.md) pomocí služby instalace webové platformy.

Je nutné odinstalovat všechny dříve nainstalované náhledy ASP.NET MVC 4 před instalací technologie ASP.NET MVC 4 Beta.

Tato verze není kompatibilní s Developer Preview rozhraní .NET Framework 4.5. Rozhraní .NET 4.5 Developer Preview je nutné odinstalovat před instalací beta verze ASP.NET MVC 4.

ASP.NET MVC 4 lze nainstalovat a spustit vedle sebe v architektuře ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentace

Dokumentace k rozhraní ASP.NET MVC je k dispozici na webu MSDN na následující adrese URL:

[https://go.microsoft.com/fwlink/?LinkId=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Kurzy a další informace o architektuře ASP.NET MVC jsou dostupné na stránce webu ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Podpora

Toto je verze preview a není oficiálně podporován. Pokud máte dotazy týkající se práce s touto verzí, odešlete je na fóru ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), kde jsou často schopen poskytnout neformální podporu členové komunity služby ASP.NET.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Požadavky na software

Komponenty architektury ASP.NET MVC 4 pro sadu Visual Studio vyžadují prostředí PowerShell 2.0 a buď Visual Studio 2010 s aktualizací Service Pack 1 nebo Visual Web Developer Express 2010 s aktualizací Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Upgrade projektu aplikace ASP.NET MVC 3 na rozhraní ASP.NET MVC 4

ASP.NET MVC 4 lze nainstalovat node souběžně s ASP.NET MVC 3 na stejném počítači, který poskytuje flexibilitu při volbě, když chcete upgradovat aplikaci ASP.NET MVC 3, ASP.NET MVC 4.

Nejjednodušší způsob, jak upgradovat je vytvoření nového projektu ASP.NET MVC 4 a zkopírujte všechna zobrazení, řadičů, kód a obsah souborů z existující projekt MVC 3 do nového projektu a potom aktualizovat sestavení odkazuje v novém projektu tak, aby odpovídaly původním projektem. Pokud jste provedli změny v souboru Web.config v projektu MVC 3, musí také sloučit tyto změny do souboru Web.config v projektu MVC 4.

Pokud chcete ručně upgradovat do verze 4 existující aplikaci ASP.NET MVC 3, postupujte takto:

1. Ve všech souborech Web.config v projektu (je v kořenovém adresáři projektu, jeden ve složce zobrazení a jeden v zobrazení složky pro každou oblast ve vašem projektu) nahraďte všechny výskyty následující text:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    Následující odpovídající text:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. V kořenovém souboru Web.config, aktualizovat *webPages:Version* element "2.0.0.0" a přidejte nový *PreserveLoginUrl* klíč, který má hodnotu "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. V Průzkumníku řešení, odstraňte odkaz na *System.Web.Mvc* (která odkazuje na verzi 3 DLL). Pak přidejte odkaz na *System.Web.Mvc* (v4.0.0.0). Konkrétně proveďte následující změny k aktualizaci odkazy na sestavení. Zde jsou uvedeny podrobnosti:

    1. Odstraňte odkazy na následující sestavení v Průzkumníku řešení: 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. Přidejte odkazy na následující sestavení: 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a pak vyberte Uvolnit projekt. Potom znovu klikněte pravým tlačítkem a vyberte Upravit *ProjectName*.csproj.
5. Vyhledejte *ProjectTypeGuids* elementu a nahraďte {E3E379DF-F4C6-4180-9B81-6769533ABE47} {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
6. Uložte změny, zavřete soubor projektu (.csproj), kterou jste upravovali, klikněte pravým tlačítkem na projekt a potom vyberte znovu načíst projekt.
7. Pokud projekt odkazuje na knihovny jakékoli třetí strany, kompilovaná pomocí předchozích verzích rozhraní ASP.NET MVC, otevřete v kořenovém souboru Web.config a přidejte následující tři *bindingRedirect* elementů v rámci  *konfigurace* části: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nové funkce ve verzi Beta architektury ASP.NET MVC 4

Tato část popisuje funkce, které byly zavedeny v ASP.NET MVC 4 Beta verzi.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>Rozhraní API pro ASP.NET Web

ASP.NET MVC 4 nyní zahrnuje webové rozhraní API ASP.NET, nové rozhraní pro vytváření služeb HTTP, které můžete využity širokou škálou klientů včetně prohlížečů a mobilních zařízení. Rozhraní ASP.NET Web API je také ideální platformu pro vytváření služeb RESTful.

Rozhraní ASP.NET Web API zahrnuje podporu pro následující funkce:

- **Moderní programovací model HTTP:** přímo přistupovat a zpracování požadavků HTTP a odpovědi v webová rozhraní API pomocí nové, silného typu HTTP objektový model. Stejný programovací model a kanál protokolu HTTP je symetricky dostupný na klientovi prostřednictvím nového typu HttpClient.
- **Podpora pro trasy plně**: rozhraní Web API nyní podporuje úplnou sadu funkcí trasy, které byly vždy součástí webové zásobníku, včetně parametry trasy a omezení. Kromě toho mapování akce má plnou podporu pro konvence, takže už nemusíte použít atributy, jako je například [HttpPost] do třídy a metody.
- **Vyjednávání obsahu**: klient a server můžou spolupracovat a určit správném formátu pro data návratu z rozhraní API. Poskytujeme výchozí podpora XML, JSON a formáty kódovaná adresou URL formuláře a můžete rozšířit tato podpora přidáním vlastní formátování, nebo dokonce nahrazení výchozí strategie vyjednávání obsahu.
- **Modelu a ověření vazeb:** vazače modelů poskytují snadný způsob, jak extrahovat data z různých částí požadavku HTTP a převést objekty .NET, které se dají použít v akcích webového rozhraní API ty části zprávy.
- **Filtry:** webových rozhraní API teď podporuje filtrů, a to včetně dobře známé filtry, například atribut [Authorize]. Můžete vytvářet a zařadit vlastní filtry akce, autorizace a výjimek.
- **Vytváření dotazu:** jednoduše vrácením IQueryable&lt;T&gt;, webové rozhraní API bude podporovat dotazování prostřednictvím konvence prostředí OData pro adresy URL.
- **Vylepšené možnosti testování podrobností protokolu HTTP:** místo nastavení HTTP podrobnosti ve statickém kontextu objekty, webového rozhraní API akce můžou teď spolupracovat s instancí objektu HttpRequestMessage a objekt HttpResponseMessage. Obecné verzích tyto objekty existují také do práce s pomocí vašich vlastních typů kromě typy HTTP.
- **Vylepšené inverzi z ovládacího prvku (IoC) prostřednictvím překladače závislostí:** vzoru Lokátor služby implementované překladač závislostí MVC webového rozhraní API teď používá k získání instance pro mnoho různých zařízení.
- **Konfigurace založená na kódu:** výhradně prostřednictvím kódu se provádí konfiguraci webového rozhraní API, a vaše konfigurace vyčistit soubory.
- **Hostování na vlastním:** webová rozhraní API je možné hostovat v vlastního procesu kromě služby IIS, při použití stále potenciál trasy a další funkce webového rozhraní API.

Pro další informace o rozhraní ASP.NET Web API naleznete [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>Aplikace ASP.NET jedné stránky

ASP.NET MVC 4 nyní zahrnuje časné prostředí pro vytváření aplikací jediné stránce s významné interakce na straně klienta pomocí jazyka JavaScript a webová rozhraní API. Tato podpora zahrnuje:

- Sada knihoven jazyka JavaScript pro širší místní interakce s data uložená v mezipaměti
- Další součásti webového rozhraní API pro jednotku práce a podpora vrstvy DAL
- K šabloně projektu MVC pomocí generování uživatelského rozhraní, abyste mohli rychle začít

Pro další informace o jedné stránky aplikace podporují v architektuře ASP.NET MVC 4, navštivte [https://www.asp.net/single-page-application](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Vylepšení výchozí šablony projektů

Šablony, která se používá k vytvoření nové projekty ASP.NET MVC 4 je aktualizovaná k vytvoření webu více moderních vyhledávání:

![](mvc4-beta-release-notes/_static/image1.png)

Kromě kosmetické vylepšení obsahuje vylepšené funkce v nové šabloně. Šablona využívá techniku zvanou adaptivního vykreslování na vzhledu v stolních počítačů a mobilních prohlížečů bez nutnosti přizpůsobení.

![](mvc4-beta-release-notes/_static/image2.png)

Pokud chcete zobrazit adaptivního vykreslování v akci, můžete použít emulátoru mobilního nebo zkuste použít Změna velikosti okna Prohlížeč pro stolní počítač, na menší. Když v okně prohlížeče získá dostatečně malé, se změní rozložení stránky.

Další vylepšení výchozí šablona projektu je použití JavaScript zajistit bohatší uživatelského rozhraní. Příklady, jak používat jQuery dialogovém okně uživatelského rozhraní k dispozici bohaté přihlašovací obrazovku jsou odkazy na přihlášení a registrace, které se používají v šabloně:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Šablona projektu mobilní

Pokud se spuštění nového projektu a chcete vytvořit web speciálně pro mobilní a tablet prohlížeče, můžete v šabloně projektů nové mobilní aplikace. To je založené na technologii jQuery Mobile, knihovny open source pro vytváření dotykovým uživatelského rozhraní:

![](mvc4-beta-release-notes/_static/image4.png)

Tato šablona obsahuje stejnou strukturu aplikací jako šablonu internetovou aplikaci (a kódu kontroleru je téměř identický), ale je navržen tak, funkční a chovat i na mobilních zařízeních se systémem touch pomocí jQuery Mobile. Další informace o tom, jak struktury a stylu mobilní uživatelského rozhraní, najdete v článku [jQuery mobilní projektu webu](http://jquerymobile.com/).

Pokud již máte orientované na ploše lokality, které chcete přidat mobilní optimalizované zobrazení nebo pokud chcete vytvořit v jedné lokalitě, který slouží jinak stylem zobrazení pro stolní počítače a mobilní prohlížeče, můžete novou funkci režimů zobrazení. (Viz další část).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Režimy zobrazení

Nová funkce režimů zobrazení umožňuje aplikaci, vyberte zobrazení v závislosti na prohlížeči, který je vytvoření požadavku. Například pokud prohlížeč pro stolní počítač požadavky na domovskou stránku, aplikace může použít šablonu Views\Home\Index.cshtml. Pokud prohlížeč pro mobilní zařízení požadavky na domovskou stránku, aplikace může vrátit Views\Home\Index.mobile.cshtml šablony.

Rozložení a částečné můžete také přepsat u určitého prohlížeče typů. Příklad:

- Pokud vaše složku Views\Shared obsahuje i \_Layout.cshtml a \_Layout.mobile.cshtml šablony, ve výchozím nastavení aplikace bude používat \_Layout.mobile.cshtml během požadavky z mobilních prohlížečů a \_Layout.cshtml během dalších požadavků.
- Pokud složka obsahuje oba \_MyPartial.cshtml a \_MyPartial.mobile.cshtml pokyn @Html.Partial("\_MyPartial") vykreslí \_MyPartial.mobile.cshtml během požadavky od mobile prohlížeče, a \_MyPartial.cshtml během dalších požadavků.

Pokud chcete vytvořit více konkrétních zobrazení, rozložení nebo částečné zobrazení pro jiná zařízení, můžete zaregistrovat nový *DefaultDisplayMode* instance k určení, který název pro vyhledání, pokud požadavek splňuje určité podmínky. Můžete například přidat následující kód, který *aplikace\_spustit* metoda v souboru Global.asax zaregistrovat řetězec "iPhone" jako režim zobrazení, která se použije, když prohlížeč Apple iPhone odešle žádost:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Po tento kód spustí, když požádá prohlížeč Apple iPhone, vaše aplikace bude používat Views\Shared\\_Layout.iPhone.cshtml rozložení (pokud existuje).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, přepínači zobrazení a přepsání prohlížeče

jQuery Mobile je knihovna s otevřeným zdrojem pro vytváření dotykovým webového uživatelského rozhraní. Pokud chcete použít jQuery Mobile s aplikací ASP.NET MVC 4, můžete stáhnout a nainstalovat balíček NuGet, který vám pomůže začít pracovat. Chcete-li ji nainstalovat z konzole Správce balíčků Visual Studio, zadejte následující příkaz:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Tím se nainstaluje jQuery Mobile a některé pomocné soubory, včetně následujících:

- Zobrazení a sdílených nebo\_Layout.Mobile.cshtml, což je jQuery Mobile na základě rozložení.
- Součást přepínači zobrazení, který se skládá z zobrazení a sdílených nebo\_ViewSwitcher.cshtml částečné zobrazení a kontroler ViewSwitcherController.cs.

Po instalaci balíčku, spusťte aplikaci pomocí mobilní prohlížeče (nebo ekvivalentní, jako Firefox [uživatele agenta přepínači](http://chrispederick.com/work/user-agent-switcher/) rozšíření). Uvidíte, že vaše stránky hledat výrazně lišit, protože jQuery Mobile zpracovává rozložení a styly. Abyste mohli využívat tohoto, můžete provést následující:

- Vytvoření mobilní konkrétní zobrazení přepsání, jak je popsáno v části [režimů zobrazení](#_Toc303253810) dříve (například vytvořit Views\Home\Index.mobile.cshtml k přepsání Views\Home\Index.cshtml pro mobilní prohlížeče).
- Pro čtení [jQuery mobilní dokumentaci](http://jquerymobile.com/) Další informace o tom, jak přidat dotykovým prvky uživatelského rozhraní v mobilních zařízeních.

Konvence pro optimalizované mobilní webové stránky je přidat odkaz, jejíž text je něco jako zobrazení plochy nebo režim plnou verzi webu, který umožňuje uživatelům přepnout na ploše verzi stránky. Balíček jQuery.Mobile.MVC obsahuje komponentu ukázka přepínači zobrazení pro tento účel. Používá se ve výchozím nastavení Views\Shared\\_Layout.Mobile.cshtml zobrazení, a to vypadá to při vykreslení stránky:

![](mvc4-beta-release-notes/_static/image5.png)

Pokud návštěvníky klikněte na odkaz, budou se přepnout do plochy verzi stejné stránce.

Protože vaše plochy rozložení nebude obsahovat přepínači zobrazení ve výchozím nastavení, nebude mít návštěvníky způsob, jak získat pro režim mobilní. Chcete-li povolit, přidejte následující odkaz na  *\_ViewSwitcher* na vaší ploše rozložení právě uvnitř  *&lt;textu&gt;*  element:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

K přepínači zobrazení používá novou funkci s názvem přepsání prohlížeče. Tato funkce umožňuje aplikaci zacházet se žádostmi, jako kdyby kdyby přicházely z jiného prohlížeče (uživatelského agenta) než ten, který jsou ve skutečnosti z. V následující tabulce jsou uvedené metody, které poskytuje přepsání prohlížeče.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Přepíše hodnotu skutečného uživatelského agenta žádosti pomocí zadaného uživatelského agenta. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Vrátí hodnotu přepsání uživatelského agenta žádosti nebo řetězec skutečného uživatelského agenta, pokud přepsání není určeno. |
| `HttpContext.GetOverriddenBrowser()` | Vrátí *HttpBrowserCapabilitiesBase* instance, která odpovídá uživatelský agent nastaveno pro požadavek (skutečné nebo přepsané). Tuto hodnotu můžete získat vlastnosti, jako *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Odebere každého přepsaného uživatelského agenta pro aktuální požadavek. |

Přepíše prohlížeče je základní funkce ASP.NET MVC 4 a je k dispozici, i když nenainstalujete jQuery.Mobile.MVC balíčku. Však ovlivňuje pouze zobrazení, rozložení a výběr částečného zobrazení – nemá vliv na jakékoli jiné funkce ASP.NET, která je závislá na *Request.Browser* objektu.

Ve výchozím nastavení je uložen uživatelský agent přepsání pomocí souboru cookie. Pokud chcete uložit přepsání jinde (například v databázi), můžete nahradit výchozí zprostředkovatel (*BrowserOverrideStores.Current*). Dokumentace pro tohoto zprostředkovatele, bude k dispozici v novější verzi rozhraní ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Recepty pro generování kódu v sadě Visual Studio

Nová funkce recepty umožňuje sadě Visual Studio pro generování kódu pro konkrétní řešení podle balíčky, které můžete nainstalovat pomocí nástroje NuGet. Rozhraní framework recepty usnadňuje vývojářům psát modulů plug-in generování kódu, který můžete použít také k nahrazení generátory předdefinované kódu pro přidat zobrazení, přidat oblast a přidat kontroler. Protože recepty jsou nasazeny jako balíčky NuGet, mohou být snadno změnami do správy zdrojového kódu a automaticky sdíleny s všechny vývojáři na projekt. Jsou k dispozici na základě za řešení.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Podpora úloh pro asynchronní řadiče

Nyní můžete napsat metody asynchronní akce v jedné metody, které vracejí objekt typu *úloh* nebo *úloh&lt;ActionResult&gt;*.

Například, pokud používáte Visual C# 5 (nebo pomocí [asynchronní CTP](https://msdn.microsoft.com/en-us/vstudio/async.aspx)), můžete vytvořit metody asynchronní akce, která vypadá třeba takto:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

V předchozích metody akce, volání *newsService.GetHeadlinesAsync* a *sportsService.GetScoresAsync* se nazývají asynchronně a vlákna z fondu vláken nejsou blokovány.

Metody asynchronní akce, které vracejí *úloh* instance může také podporovat vypršení časových limitů. Chcete-li zrušit své metodě akce, přidejte parametr typu *CancellationToken* k označení metody akce. Následující příklad ukazuje metodu asynchronní akce obsahující vypršení časového limitu 2 500 milisekund, který zobrazí *TimedOut* zobrazit klientovi, pokud dojde k vypršení časového limitu.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta podporuje září 2011 1.5 verzi sady Windows Azure SDK.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a nejnovější změny

- **Po instalaci technologie ASP.NET MVC 4 Beta, může pozastavení CSHTML nebo VBHTML editor v editoru Visual Studio 2010 Service Pack 1 CSHTML nebo VBHTML po dlouhou dobu, po zadání fragment kódu nebo JavaScript uvnitř cshtml nebo vbhtml soubory.** K tomu dojde pouze v aplikacích ASP.NET MVC 4, které jste právě vytvořili a nebyla dosud kompilována.

    Kompilace projektu k získání sestavení do složky bin je alternativní řešení. Poznámka: Pokud jste vyčistěte projekt, který odebere sestavení ze složky koše, editor problém se vraťte.

    To bude opraven v příští verzi.
- **Šablon projektu C# pro sadu Visual Studio 11 Beta obsahovat nesprávný připojovací řetězec v Global.asax.cs.** Výchozí připojení zadaný v aplikaci\_Start – metoda pro projekty vytvořené v produktu Visual Studio 11 Beta obsahují LocalDB připojovací řetězec, který obsahuje neuvozené zpětné lomítko (\) znak. Výsledkem je chyba připojení při pokouší získat přístup k DbContext Entity Framework, který generuje SqlException.

    Chcete-li tento problém, řídicí znak zpětné lomítko, v aplikaci\_spuštění metoda Global.asax.cs přečte následujícím způsobem:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Aplikace ASP.NET MVC 4, které cílí na rozhraní .NET 4.5, vyvolá výjimku FileLoadException při pokusu o přístup sestavení System.Net.Http.dll při spuštění v rozhraní .NET 4.0.** Aplikace ASP.NET MVC 4 vytvořil v rámci rozhraní .NET 4.5 obsahují vazbu přesměrování, který bude mít za následek FileLoadException, která uvádí, který "Nepodařilo se načíst soubor nebo sestavení 'System.Net.Http nebo jeden z jejich závislých." Když aplikaci spustit v systému s .NET 4.0 nainstalovat. Chcete-li tento problém, odeberte ze souboru web.config následující přesměrování vazby:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    Element vazby sestavení v souboru web.config upravené by měla vypadat takto:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- **Šablony položky "Přidat kontroler" v projektech Visual Basic generuje nesprávný obor názvů při vyvolání *** z uvnitř oblasti.** Když přidáte řadič oblasti v projektu aplikace ASP.NET MVC, která používá jazyka Visual Basic, šablony položky vloží do řadičem nesprávný obor názvů. Výsledkem je chyba "soubor nebyl nalezen", když přejdete na všechny akce v kontroleru.  
  
 Vygenerovaný obor názvů vynechá všechno, co po kořenového oboru názvů. Například je obor názvů vygenerované *RootNamespace* ale měl by být *RootNamespace.Areas.AreaName.Controllers* .
- **Nejnovější změny ve zobrazovací modul Razor.** Jako součást přepsání analyzátor Razor, byly odebrány následující typy z *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 Také byly odebrány následující metody: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Když v adresáři/Bin aplikace ASP.NET MVC 4 je součástí WebMatrix.WebData.dll, přebírá adresu URL pro ověřování pomocí formulářů.** Přidání WebMatrix.WebData.dll sestavení do vaší aplikace (například výběrem "ASP.NET Web Pages se syntaxí Razor" při použití dialogové okno Přidat nasaditelné závislosti) se přepíše přihlášení přesměrování ověřování/účet/Logon místo nebo účet nebo přihlášení očekávaným ve výchozím nastavení technologie ASP.NET MVC jsou řadič MVC účtu. Pokud chcete zakázat toto chování a používat zadaná adresa URL již v části ověření souboru Web.config, můžete přidat appSetting názvem PreserveLoginUrl a nastavte ji na hodnotu true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Správce balíčků NuGet se nepodaří nainstalovat při pokusu o instalaci ASP.NET MVC 4 pro souběžné instalace sady Visual Studio 2010 a program Visual Web Developer 2010.** Ke spuštění sady Visual Studio 2010 a program Visual Web Developer 2010 node souběžně s ASP.NET MVC 4 je nutné nainstalovat ASP.NET MVC 4 po obě verze sady Visual Studio již byl nainstalován.
- **Odinstalace ASP.NET MVC 4 selže, pokud již byl odinstalován požadavky.** Chcete-li řádně odinstalovat ASP.NET MVC musí 4you odinstalovat ASP.NET MVC 4 před odinstalací nástroje Visual Studio.
- **Spuštění výchozí projekt webového rozhraní API obsahuje pokyny, které řídí nesprávně uživateli přidat trasy pomocí metody RegisterApis, která neexistuje.** V metodě RegisterRoutes pomocí ASP.NET směrovací tabulka musí být přidaní trasy.
- **Instalace technologie ASP.NET MVC 4 Beta dělí aplikace ASP.NET MVC 3 RTM.** Aplikace ASP.NET MVC 3, které byly vytvořeny s RTM verze (není verze ASP.NET MVC 3 nástroje pro aktualizaci) vyžaduje následující změny fungování souběžného pomocí technologie ASP.NET MVC 4 Beta. Vytváření projektu bez provedení těchto výsledků aktualizací k chybám kompilace. 

    **Požadované aktualizace**

    1. V kořenovém souboru Web.config, přidejte nový  *&lt;appSettings&gt;*  položka s klíčem *webPages:Version* a hodnotu *1.0.0.0*.

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
    2. V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a pak vyberte Uvolnit projekt. Potom znovu klikněte pravým tlačítkem a vyberte Upravit *ProjectName*.csproj.
    3. Vyhledejte následující odkazy na sestavení: 

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

        Nahraďte je následující:

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
    4. Uložte změny, zavřete soubor projektu (.csproj) byly úpravy a potom klikněte pravým tlačítkem na projekt a vyberte znovu načíst.
