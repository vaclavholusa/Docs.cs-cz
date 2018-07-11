---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument popisuje verzi technologie ASP.NET MVC 4 Beta verze pro Visual Studio 2010.
ms.author: aspnetcontent
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: b9d50114a239b67b1adc263f6ea6d3a811bcc8f7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816688"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Tento dokument popisuje verzi technologie ASP.NET MVC 4 Beta verze pro Visual Studio 2010.
> 
> > [!NOTE]
> > Toto není nejnovější verze. Zpráva k vydání verze technologie ASP.NET MVC 4 RC jsou k dispozici [tady](mvc4-release-notes.md).


- [Poznámky k instalaci](#_Toc303253802)
- [Dokumentace](#_Toc303253803)
- [Podpora](#_Toc303253804)
- [Požadavky na software](#_Toc303253805)
- [Upgrade projektu aplikace ASP.NET MVC 3 na ASP.NET MVC 4](#_Toc303253806)
- [Nové funkce ve verzi Beta architektury ASP.NET MVC 4](#_Toc303253807)

    - [Webové rozhraní API v ASP.NET](#_Toc317096197)
    - [ASP.NET jedním jednostránková aplikace](#_Toc317096198)
    - [Vylepšení pro výchozí šablony projektu](#_Toc303253808)
    - [Šablona projektu mobilní](#_Toc303253809)
    - [Režimy zobrazení](#_Toc303253810)
    - [jQuery Mobile, přepínání zobrazení a přepsání prohlížeče](#_Toc303253811)
    - [Recepty pro generování kódu v sadě Visual Studio](#_Toc303253812)
    - [Podpora úkol pro asynchronní řadiče](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Známé problémy a změny způsobující chyby](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Poznámky k instalaci

ASP.NET MVC 4 Beta verze pro Visual Studio 2010 můžete nainstalovat pomocí [domovskou stránku ASP.NET MVC 4](../mvc/mvc4.md) pomocí instalačního programu webové platformy.

Je nutné odinstalovat všechny dříve nainstalované verze Preview architektury ASP.NET MVC 4 před instalací beta verze technologie ASP.NET MVC 4.

Tato verze není kompatibilní s náhled pro vývojáře rozhraní .NET Framework 4.5. Náhled pro vývojáře .NET 4.5 je nutné odinstalovat před instalací beta verze technologie ASP.NET MVC 4.

ASP.NET MVC 4 je možné nainstalovat a spustit vedle sebe v architektuře ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentace

Dokumentace k ASP.NET MVC je k dispozici na webu MSDN na následující adrese URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Kurzy a další informace o architektuře ASP.NET MVC jsou k dispozici na stránce na webu technologie ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Podpora

Toto je verze preview a není oficiálně podporován. Pokud máte dotazy týkající se práce v této vydané verzi, příspěvek do fóra ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), kde jsou často schopni poskytovat podporu neformální členové komunity technologie ASP.NET.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Požadavky na software

Komponenty architektury ASP.NET MVC 4 pro Visual Studio vyžadují prostředí PowerShell 2.0 a Visual Studio 2010 s aktualizací Service Pack 1 nebo Visual Web Developer Express 2010 s aktualizací Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Upgrade projektu aplikace ASP.NET MVC 3 na ASP.NET MVC 4

ASP.NET MVC 4 je nainstalovat souběžně s ASP.NET MVC 3 na stejném počítači, který poskytuje flexibilitu při výběru, kdy se má upgradovat aplikaci ASP.NET MVC 3 na ASP.NET MVC 4.

Nejjednodušší způsob, jak upgradovat je k vytvoření nového projektu ASP.NET MVC 4 a zkopírujte všechny zobrazení, kontrolery, kódu a obsah souborů z existující projekt MVC 3 na nový projekt a potom aktualizovat sestavení odkazuje v novém projektu tak, aby odpovídaly původním projektem. Pokud jste provedli změny v souboru Web.config v projektu MVC 3, musíte také sloučit tyto změny do souboru Web.config v projekt MVC 4.

Ručně upgradovat existující aplikaci ASP.NET MVC 3 na verzi 4, postupujte takto:

1. Ve všech souborech Web.config v projektu (je v kořenovém adresáři projektu, jeden v zobrazení složky a druhý ve složce zobrazení pro každou oblast, ve vašem projektu) nahraďte každou instanci následující text:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    s odpovídající následujícím textem:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. V kořenovém souboru Web.config aktualizujte *webPages:Version* element na "2.0.0.0" a přidejte nový *PreserveLoginUrl* klíč, který má hodnotu "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. V Průzkumníku řešení, odstraňte odkaz na *System.Web.Mvc* (která odkazuje na verzi 3 knihovny DLL). Pak přidejte odkaz na *System.Web.Mvc* (v4.0.0.0). Zejména proveďte následující změny k aktualizaci odkazy na sestavení. Tady jsou podrobnosti:

    1. V Průzkumníku řešení odstraňte odkazy na následující sestavení: 

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
4. V Průzkumníku řešení klikněte pravým tlačítkem myši na název projektu a pak vyberte Uvolnit projekt. Klikněte pravým tlačítkem na název a vyberte Upravit *ProjectName*csproj.
5. Vyhledejte *ProjectTypeGuids* prvku a nahraďte {E53F8FEA-EAE0-44A6-8774-FFD645390401} s {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Změny uložte, zavřete soubor projektu (.csproj), který jste dříve upravovali, klikněte pravým tlačítkem na projekt a poté vyberte znovu načíst projekt.
7. Pokud projekt odkazuje na žádné knihovny třetích stran, které jsou kompilovány pomocí předchozí verze technologie ASP.NET MVC, otevřete v kořenovém souboru Web.config a přidat následující tři *bindingRedirect* prvků  *konfigurace* části: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nové funkce ve verzi Beta architektury ASP.NET MVC 4

Tato část popisuje funkce, které byly zavedeny v ASP.NET MVC 4 Beta verzi.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>Rozhraní API pro ASP.NET Web

Teď obsahuje rozhraní ASP.NET Web API ASP.NET MVC 4, nové rozhraní pro vytváření služeb HTTP, který můžete oslovit širokou škálou klientů včetně prohlížečů a mobilních zařízení. ASP.NET Web API je také ideální platformu pro vytváření služby typu REST.

Rozhraní ASP.NET Web API zahrnuje podporu pro následující funkce:

- **Moderní programovací model HTTP:** přímo přistupovat k a manipulaci s žádostí HTTP a odpovědí v rozhraní Web API s využitím nové, silného typu HTTP objektový model. Stejné programovací model a kanálu protokolu HTTP je symetricky k dispozici na straně klienta pomocí nového typu HttpClient.
- **Plnou podporu pro trasy**: webová rozhraní API teď podporují kompletní sadu funkcí trasy, které byly vždy součástí webové problematiku, včetně parametry trasy a omezení. Kromě toho mapování akcí má plnou podporu pro vytváření názvů, takže už nemusíte použít atributy, jako je například [HttpPost] do vaší třídy a metody.
- **Vyjednávání obsahu**: klient a server můžou spolupracovat a určit správný formát pro data se vrací z rozhraní API. Výchozí podporu pro kódovaná adresou URL formuláře formáty XML, JSON a a mohou rozšířit tuto podporu tak, že přidáte vlastní formátovací moduly, nebo dokonce nahrazujte výchozí strategie vyjednávání obsahu.
- **Vazby modelu a ověření:** vazače modelů poskytují snadný způsob, jak extrahovat data z různých částí požadavek HTTP a převést tyto části zprávy do objektů .NET, které mohou být využívána akcí webového rozhraní API.
- **Filtry:** webová rozhraní API teď podporuje filtry, včetně dobře známých filtry, např. atribut [Authorize]. Můžete vytvářet a zapojte vlastní filtry pro akce a povolení zpracování výjimek.
- **Vytváření dotazu:** jednoduše vrácením IQueryable&lt;T&gt;, webové rozhraní API podporuje dotazování pomocí konvencí OData pro adresy URL.
- **Zlepšení testovatelnosti podrobnosti protokolu HTTP:** místo nastavení podrobností protokolu HTTP v objektech statického kontextu, akcí webového rozhraní API teď můžete pracovat s instancemi HttpRequestMessage a objekt HttpResponseMessage. Obecný verzích tyto objekty existují také umožňuje pracovat s vaší vlastní typy kromě typů protokolu HTTP.
- **Vylepšené inverzi ovládacího prvku (Inversion) prostřednictvím překladače závislostí:** webového rozhraní API teď používá vzor Lokátor služby implementované překladač závislostí MVC k získání instance pro mnoho různých zařízení.
- **Konfigurace založená na kódu:** konfiguraci webového rozhraní API se provádí pouze pomocí kódu, vyčistit byste museli opustit váš konfigurační soubory.
- **Hostování na vlastním serveru:** webová rozhraní API je možné hostovat ve vašem vlastním procesu kromě IIS stále přitom výkon trasy a dalších funkcích služby webového rozhraní API.

Další informace o rozhraní ASP.NET Web API najdete [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET jedním jednostránková aplikace

ASP.NET MVC 4 teď zahrnuje velmi předběžnou verzi prostředí pro sestavení jednostránkové aplikace s významnou interakce na straně klienta pomocí JavaScriptu a webových rozhraní API. Tato podpora zahrnuje:

- Sada knihoven jazyka JavaScript pro bohatší místní interakce se data uložená v mezipaměti
- Další součásti webového rozhraní API pro jednotku práce a DAL podpory
- Šablona projektu MVC pomocí generování uživatelského rozhraní, abyste mohli rychle začít

Další podrobnosti o jednu stránku aplikaci podpory v architektuře ASP.NET MVC 4, navštivte [ https://www.asp.net/single-page-application ](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Vylepšení pro výchozí šablony projektu

Jak vytvořit web více moderního vzhledu byl aktualizován šablonu, která se používá k vytvoření nových projektech ASP.NET MVC 4:

![](mvc4-beta-release-notes/_static/image1.png)

Kromě kosmetické vylepšení obsahuje vylepšené funkce v nové šablony. Šablona využívá techniky označované jako adaptivního vykreslování do vypadat dobře v počítačových prohlížečů a mobilní prohlížeče bez jakéhokoli přizpůsobení.

![](mvc4-beta-release-notes/_static/image2.png)

Zobrazíte adaptivního vykreslování v akci, můžete použít mobilní emulátor nebo zkuste použít mění velikost okna prohlížeči v počítači. Chcete-li být menší. Pokud okno prohlížeče získá dostatečně malá, se změní rozložení stránky.

Mezi další vylepšení patří na výchozí šabloně projektu je použití JavaScriptu k poskytování bohatších uživatelského rozhraní. Přihlášení a zaregistrujte odkazy, které se používají v šabloně jsou příklady, jak používat dialogové okno uživatelského rozhraní jQuery prezentovat bohaté přihlašovací obrazovky:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Šablona projektu mobilní

Pokud začíná nový projekt a chcete vytvořit web speciálně pro mobilní zařízení a prohlížečů tablet, můžete použít novou šablonu projektu mobilní aplikace. To je založené na technologii jQuery Mobile, knihovny open source pro vytváření optimalizovaných pro dotykové ovládání uživatelského rozhraní:

![](mvc4-beta-release-notes/_static/image4.png)

Tato šablona obsahuje stejnou strukturu aplikace jako šablony internetovou aplikaci (a kódu kontroleru je prakticky totožný), ale je ve stylu pomocí jQuery Mobile vypadat dobře a chovat i na mobilních zařízeních s dotykovým ovládáním. Další informace o tom, jak strukturovat a stylu mobilní uživatelského rozhraní, najdete v článku [jQuery Mobile projektu webu](http://jquerymobile.com/).

Pokud už máte orientované na ploše lokality, které chcete přidat zobrazení optimalizovaných pro mobilní zařízení nebo pokud chcete vytvořit jeden web, který slouží jinak upravený zobrazení pro stolní počítače a mobilní prohlížeče, můžete novou funkci režimů zobrazení. (Viz další části.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Režimy zobrazení

Nová funkce režimů zobrazení umožní aplikaci vyberte zobrazení v závislosti na prohlížeči, který provádí požadavek. Například pokud prohlížeč pro stolní počítač požadavků na domovskou stránku, aplikaci použít Views\Home\Index.cshtml šablony. Pokud mobilního prohlížeče požadavků na domovskou stránku, aplikace může vrátit Views\Home\Index.mobile.cshtml šablony.

Rozložení a částečných zobrazení lze také přepsat u určitého prohlížeče typů. Příklad:

- Pokud obsahuje složku Views\Shared \_Layout.cshtml a \_Layout.mobile.cshtml šablony, ve výchozím nastavení bude aplikace používat \_Layout.mobile.cshtml během žádosti od mobilních prohlížečů a \_Layout.cshtml během dalších požadavků.
- Pokud složka obsahuje oba \_MyPartial.cshtml a \_MyPartial.mobile.cshtml instrukce @Html.Partial("\_MyPartial") bude vykreslení \_MyPartial.mobile.cshtml během požadavků z mobile prohlížeče, a \_MyPartial.cshtml během dalších požadavků.

Pokud chcete vytvořit podrobnější zobrazení, rozložení nebo částečná zobrazení pro další zařízení, můžete zaregistrovat nový *DefaultDisplayMode* instance k určení, které název vyhledávat požadavek splňuje určité podmínky. Můžete například přidat následující kód, který *aplikace\_Start* metody v souboru Global.asax pro registraci řetězec "iPhone" jako režim zobrazení, která se použije, když prohlížeč Apple iPhone odešle požadavek:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Po spuštění tohoto kódu, pokud prohlížeče pro iPhone Apple odešle požadavek, bude aplikace používat Views\Shared\\_Layout.iPhone.cshtml rozložení (pokud existuje).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, přepínání zobrazení a přepsání prohlížeče

jQuery Mobile je open source knihovna pro vytváření dotykového webového uživatelského rozhraní. Pokud chcete použít jQuery Mobile se aplikaci ASP.NET MVC 4, můžete stáhnout a nainstalovat balíček NuGet, který vám pomůže začít pracovat. Chcete-li ji nainstalovat z konzoly Správce balíčků Visual Studio, zadejte následující příkaz:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Tím se nainstaluje jQuery Mobile a některé soubory pomocné rutiny, včetně následujících:

- Zobrazení/Shared/\_Layout.Mobile.cshtml, což je jQuery Mobile podle rozložení.
- Součásti přepínači zobrazení, která se skládá zezobrazení/Shared/\_ViewSwitcher.cshtml částečné zobrazení a kontroler ViewSwitcherController.cs.

Po instalaci balíčku, spusťte aplikaci v mobilním prohlížeči (nebo ekvivalentní, jako Firefox [uživatelského agenta přepínání](http://chrispederick.com/work/user-agent-switcher/) doplňku). Uvidíte, že se vaše stránky podívat značně odlišná, protože jQuery Mobile zpracovává rozložení a práce se styly. Abyste mohli využívat tohoto objektu, vám pomůžou následující:

- Vytvoření mobilní konkrétní zobrazení přepsání, jak je popsáno v části [režimů zobrazení](#_Toc303253810) dříve (například vytvořit Views\Home\Index.mobile.cshtml k přepsání Views\Home\Index.cshtml pro mobilní prohlížeče).
- Přečtěte si [jQuery Mobile dokumentaci](http://jquerymobile.com/) Další informace o tom, jak přidat dotykového prvky uživatelského rozhraní v mobilních zařízeních.

Konvence pro webové stránky optimalizovaných pro mobilní zařízení se přidat odkaz, jejichž text je něco jako zobrazení plochy nebo režim celý web, který umožňuje uživatelům přepínat desktopová verze stránky. Balíček jQuery.Mobile.MVC obsahuje komponentu přepínači zobrazení vzorku pro tento účel. Používá se ve výchozím nastavení Views\Shared\\_Layout.Mobile.cshtml zobrazení, a to vypadá to při vykreslování stránky:

![](mvc4-beta-release-notes/_static/image5.png)

Pokud uživatelé kliknou na odkaz, jejich jste přešli na desktopové verzi na stejné stránce.

Protože rozložení pro počítače se ve výchozím nastavení zahrnují přepínači zobrazení, nebudou mít návštěvníků způsob, jak získat na mobilní režim. Chcete-li povolit, přidejte následující odkaz na  *\_ViewSwitcher* na vaše rozložení pro počítače, jenom uvnitř *&lt;tělo&gt;* element:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Přepínání zobrazení používá novou funkci označovanou jako přepsání prohlížeče. Tato funkce umožňuje aplikaci zacházet se žádostmi, jako kdyby kdyby přicházely z jiného prohlížeče (uživatelského agenta) než ten, že jsou ve skutečnosti z. Následující tabulka shrnuje metody, které poskytuje přepsání prohlížeče.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Přepíše hodnotu skutečného uživatelského agenta žádosti pomocí zadaného uživatelského agenta. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Vrátí hodnotu přepsání uživatelského agenta žádosti nebo řetězec skutečného uživatelského agenta, pokud přepsání není určeno. |
| `HttpContext.GetOverriddenBrowser()` | Vrátí *HttpBrowserCapabilitiesBase* instanci, která odpovídá pro uživatelského agenta, který je aktuálně nastavený pro žádost (skutečné nebo přepsané). Tato hodnota slouží k získání vlastností, jako *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Odebere každého přepsaného uživatelského agenta pro aktuální požadavek. |

Přepíše prohlížeče je funkce jádra ASP.NET MVC 4 a je k dispozici i v případě, že nechcete nainstalovat balíček jQuery.Mobile.MVC. To ovlivní pouze zobrazení, rozložení a výběr částečného zobrazení – nemá vliv na jakékoli jiné funkce technologie ASP.NET, na kterém závisí *Request.Browser* objektu.

Ve výchozím nastavení je uložen přepsání uživatelského agenta pomocí souboru cookie. Pokud chcete ukládat přepsání jinde (například v databázi), můžete nahradit výchozí zprostředkovatel (*BrowserOverrideStores.Current*). Dokumentace ke službě pro tohoto zprostředkovatele bude k dispozici novější verze technologie ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Recepty pro generování kódu v sadě Visual Studio

Nová funkce recepty umožňuje sadě Visual Studio ke generování kódu pro konkrétní řešení založené na balíčky, které můžete nainstalovat pomocí nástroje NuGet. Rozhraní recepty usnadňuje vývojářům umožňuje psát moduly plug-in generování kódu, které lze také použít k nahrazení generátory kódu integrované přidat oblast, přidat kontroler a přidat zobrazení. Protože recepty jsou nasazeny jako balíčky NuGet, můžou být snadno zapsány do správy zdrojového kódu a sdílené s všichni vývojáři v projektu automaticky. Jsou k dispozici na základě každé řešení.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Podpora úkol pro asynchronní řadiče

Teď můžete psát asynchronní akce metody jako jedné metody, které vracejí objekt typu *úloh* nebo *úloh&lt;ActionResult&gt;*.

Například, pokud používáte Visual C# 5 (nebo pomocí [asynchronní CTP](https://msdn.microsoft.com/vstudio/async.aspx)), můžete vytvořit asynchronní metodě akce, která vypadá nějak takto:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

V předchozím metodě akce, volání *newsService.GetHeadlinesAsync* a *sportsService.GetScoresAsync* jsou volány asynchronně a nedochází k blokování vlákna z fondu podprocesů.

Metody asynchronní akce, které vracejí *úloh* instance může také podporovat vypršení časového limitu. Chcete-li metodu akce zrušitelný, přidejte parametr typu *CancellationToken* do podpisu metody akce. Následující příklad ukazuje asynchronní metodě akce, který má časový limit 2 500 milisekund a, který se zobrazí *vypršel časový limit* zobrazit klientovi, pokud dojde k vypršení časového limitu.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta podporuje září 2011 1.5 vydání sady Windows Azure SDK.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a změny způsobující chyby

- **Po instalaci aplikace ASP.NET MVC 4 Beta se může pozastavit CSHTML a VBHTML editoru v editoru Visual Studio 2010 Service Pack 1 CSHTML a VBHTML dlouhou dobu, po zadání fragment kódu nebo JavaScriptu uvnitř cshtml nebo vbhtml souborů.** K tomu dojde pouze v aplikacích ASP.NET MVC 4, které jste právě vytvořili a ještě nebyla kompilována.

    Alternativním řešením je ke kompilaci projektu zobrazíte sestavení ve složce bin. Mějte na paměti, pokud je vyčistit projekt, který odstraní sestavení ze složky koše, editor problém bude vracet.

    To bude opraven v další vydané verzi.
- **Šablony projektů C# pro Visual Studio 11 Beta obsahují nesprávnému připojovacímu řetězci v Global.asax.cs.** Výchozí připojení zadaný v aplikaci\_začátek metody pro projekty vytvořené v aplikaci Visual Studio 11 Beta obsahují LocalDB připojovací řetězec, který obsahuje znakem zpětného lomítka (\) znak. Výsledkem je chyba připojení při pokusy o přístup k DbContext v Entity Framework, která generuje výjimkou SQL.

    Chcete-li tento problém, řídicí znak zpětného lomítka v aplikaci\_Start, metoda Global.asax.cs tak, aby vypadal takto:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Aplikace ASP.NET MVC 4, které cílovou verzi .NET 4.5, vyvolá výjimku FileLoadException – při pokusu o přístup k sestavení System.Net.Http.dll při spuštění v rozhraní .NET 4.0.** ASP.NET MVC 4 aplikace vytvořené v rámci rozhraní .NET 4.5 obsahují vazby přesměrování, který bude mít za následek FileLoadException –, který uvádí, které "nelze načíst soubor nebo sestavení 'System.Net.Http' nebo některou z jeho závislostí." Pokud aplikace je spuštěn v systému nainstalované rozhraní .NET 4.0. Chcete-li tento problém vyřešit, odeberte ze souboru web.config následující přesměrování vazby:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    Element vazby sestavení v upravený soubor web.config by měl vypadat takto:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Šablona položky "Přidat kontroler" v projektech Visual Basic generuje nesprávný obor názvů při vyvolání</strong><strong>z uvnitř oblasti.</strong> Když přidáte řadič na oblast v projektu ASP.NET MVC, která používá jazyka Visual Basic, šablony položky vloží nesprávný obor názvů do kontroleru. Výsledkem je chyba "soubor nebyl nalezen", když přejdete na všechny akce v kontroleru.  
  
  Vygenerovaný obor názvů vynechá všechno, co po kořenového oboru názvů. Například je obor názvů generované *RootNamespace* ale měl by být *RootNamespace.Areas.AreaName.Controllers* .
- **Rozbíjející změny v zobrazovací modul Razor.** Jako součást přepisování analyzátor Razor, byly odebrány následující typy *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Také byly odstraněny následující metody: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **WebMatrix.WebData.dll je obsažen v adresáři/Bin aplikací technologie ASP.NET MVC 4, převezme adresu URL pro ověřování pomocí formulářů.** Přidání WebMatrix.WebData.dll sestavení pro vaši aplikaci (například tak, že vyberete "ASP.NET Web Pages se syntaxí Razor" při použití dialogového okna Přidat Nasaditelných závislostí) se přepíše přihlášení přesměrování ověřování/účet/Logon spíše než / / přihlášení k účtu očekávaným ve výchozím nastavení kontroler ASP.NET MVC účtu. K tomuto chování zabráníte a použijte adresu URL již definováno v sekci ověřování souboru Web.config, můžete přidat nastavení aplikace volá PreserveLoginUrl a nastavte ho na hodnotu true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Správce balíčků NuGet se nepodařilo nainstalovat při pokusu o instalaci technologie ASP.NET MVC 4 pro souběžné instalace sady Visual Studio 2010 a Visual Web Developer 2010.** Ke spuštění sady Visual Studio 2010 a Visual Web Developer 2010 souběžně s ASP.NET MVC 4 je nutné nainstalovat technologie ASP.NET MVC 4 po již byly nainstalovány obě verze systému Visual Studio.
- **Odinstalování technologie ASP.NET MVC 4 selže, pokud již byly odinstalovány požadavky.** Čistě odinstalování technologie ASP.NET MVC 4you odinstalujte před odinstalací sady Visual Studio ASP.NET MVC 4.
- **Spuštěný výchozí projekt webového rozhraní API se zobrazí pokyny, které se nesprávně nasměrovat uživatele k přidání tras pomocí RegisterApis metodu, která neexistuje.** V metodě RegisterRoutes pomocí technologie ASP.NET směrovací tabulka, měli byste přidat trasy.
- **Instalace technologie ASP.NET MVC 4 Beta přestane fungovat aplikace ASP.NET MVC 3 RTM.** Aplikace ASP.NET MVC 3, které byly vytvořeny s RTM verze (ne verze technologie ASP.NET MVC 3 nástroje Update) vyžaduje následující změny fungování souběžně s ASP.NET MVC 4 Beta. Vytváření projektu bez provedení těchto aktualizací výsledky v chyby kompilace. 

    **Požadované aktualizace**

  1. V kořenovém souboru Web.config přidejte nový *&lt;appSettings&gt;* položka s klíčem *webPages:Version* a hodnota *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. V Průzkumníku řešení klikněte pravým tlačítkem myši na název projektu a pak vyberte Uvolnit projekt. Klikněte pravým tlačítkem na název a vyberte Upravit *ProjectName*csproj.
  3. Vyhledejte následující odkazy na sestavení: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Nahraďte je následující:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Změny uložte, zavřete soubor projektu (.csproj) dříve upravovali a pak klikněte pravým tlačítkem na projekt a vyberte znovu načíst.
