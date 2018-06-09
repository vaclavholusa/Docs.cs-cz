---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET a webové nástroje 2013.2 poznámky k verzi 2013 Visual Studio | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2014
ms.topic: article
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 0e7ad52662f7ceaa1f087d007d0b14b610f90bee
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036021"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>Technologie ASP.NET a webové nástroje 2013.2 poznámky k verzi 2013 Visual Studio
====================
podle [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Poznámky k instalaci

ASP.NET a nástroje Web Tools pro Visual Studio 2013.2 jsou seskupeny v hlavní instalační program a můžete stáhnout jako součást [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET a webové nástroje pro Visual Studio 2013.2 jsou dostupné na [webové stránky ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Požadavky na software

Technologie ASP.NET a nástroje Web Tools pro Visual Studio 2013.2 vyžaduje Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nové funkce v technologii ASP.NET a nástroje Web Tools pro Visual Studio 2013.2

Následující části popisují funkce, které byly zavedeny v verzi.

- [Šablony projektů jeden ASP.NET](#oneaspnet)
- [Podporovat protokol SSL při spuštění webové aplikace ve službě IIS Express](#ssl)
- [Vylepšení editoru webové Visual Studio](#vswebeditor)
- [Browser Link](#browserlink)
- [Podpora pro Azure App Service Web Apps v sadě Visual Studio](#waws)
- [Vytvořit vzdálené prostředky Azure při vytváření nového webového projektu](#AzureResources)
- [Vylepšení publikování webu](#webpublish)
- [ASP.NET generování uživatelského rozhraní](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.NET – webové formuláře](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [Rozhraní ASP.NET Web API 2.1.2](#webapi)
- [Rozhraní ASP.NET Web Pages 3.1.2](#webpages)
- [Rozhraní Entity Framework 6.1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Komponenty Microsoft OWIN](#owin)
- [Funkce SignalR technologie ASP.NET bodu 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Šablony projektů jeden ASP.NET

- Aktualizace šablon projektu ASP.NET pro podporu potvrzení účtu a resetování hesla.
- Aktualizace šablony ASP.NET Web API pro podporu ověřování pomocí na místní účty organizace.
- Šablona ASP.NET SPA teď obsahuje ověřování, který je založen na straně zobrazení MVC a serveru. Šablona má řadiči WebAPI, který lze přistupovat pouze ověřené uživatele.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Podporovat protokol SSL při spuštění webové aplikace ve službě IIS Express

K vyloučení upozornění zabezpečení při procházení a ladění HTTPS na místního hostitele, jsme přidali, zobrazí se dialogové okno povolit, že Internet Explorer a Chrome důvěřovat podepsaný svým držitelem služby IIS express certifikát SSL.

Vlastnosti projektu webového lze například nastavit na používání protokolu SSL. Klikněte na tlačítko F4 zprovoznit dialogovém okně Vlastnosti. Změna **povolen protokol SSL** na hodnotu true. Zkopírujte adresu URL protokolu SSL.

![Vlastnost povolen protokol SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Sada web kartu webového projektu vlastnost stránku pro použití protokolu HTTPS na základě adresy URL (adresa URL SSL bude `https://localhost:44300/` Pokud jste dříve vytvořili webů SSL.)

![Nastavení adresy URL projektu (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci. Postupujte podle pokynů důvěřovat certifikát podepsaný svým držitelem, který služba IIS Express vygenerovala.

![Upozornění SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Pro čtení **upozornění zabezpečení** dialogové okno a potom klikněte na **Ano** Pokud chcete nainstalovat certifikát představující localhost.

![Upozornění zabezpečení](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Web se zobrazí v aplikaci Internet Explorer nebo Chrome bez upozornění certifikátu v prohlížeči.

![Stránka HTTPS bez upozornění](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox používá vlastní úložiště certifikátů, proto se zobrazí upozornění.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Vylepšení editoru webové Visual Studio

- **Nová položka projektu JSON a editor**: jsme přidali položka projektu JSON a editor pro Visual Studio. Aktuální funkce editor JSON zahrnují zabarvení, ověření syntaxe, dokončení složené závorce, osnovy, nástrojů možnost nastavení a další.

    ![JSON Editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense nyní podporuje [schématu JSON](http://json-schema.org/) v3 a v4. Je schématu pole se seznamem vyberte existující schémata, upravte cestu místní schématu, nebo vyřaďte jednoduše přetáhněte soubor JSON projektu do ní relativní cesty.

    ![JSON Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Editor schématu JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nový editor Sass (SCSS)**: jsme přidali menší v VS2013 RTM a nyní je k dispozici položka projektu Sass a editor. Editor sass funkce srovnatelné s LESS editor a zahrnují zabarvení, proměnné a Mixins IntelliSense, komentáře nebo zrušte komentář u rychlé informace, formátování, ověření syntaxe, osnovy, definice goto, volby barev nástrojů možnost nastavení atd.

    ![Přidat novou položku: SCSS šablony stylů](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor stylů](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nový výběr adresy URL ve formátu HTML, Razor, CSS, LESS a Sass dokumentů:** VS 2013 součástí žádný výběr adresy URL mimo stránky webových formulářů. Nový výběr adresy URL pro HTML, Razor, CSS, LESS a Sass editory je dialog bez, fluent výběr zadáte, která funguje s technologií '..' a jsou uvedené filtry souborů správně img značky a odkazy.

    ![Výběr adresy URL pro značky obrázku](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Výběr adresy URL pro zobrazení](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Výběr adresy URL pro šablon stylů CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Aktualizace editor LESS přidáním další funkce**
- **Knockout Intellisense Upgrade**: jsme přidali nestandardní syntaxe KnockOut pro VS intelliSense "ko-vs-editor viewModel:" syntaxe. Slouží k vytvoření vazby více modelů zobrazení na stránce pomocí komentáře ve tvaru:

    ![Knockout Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Můžeme také přidána podpora pro vnořené ViewModel IntelliSense, tak může rozbalit hluboko vložené objekty v ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    IntelilSense, zobrazí se úplná IntelliSense objektu JavaScript.

    ![IntelliSense zobrazující úplné objekt jazyka JavaScript](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nový výběr adresy URL ve formátu HTML, Razor, CSS, LESS a Sass dokumentů**: VS 2013 součástí žádný výběr adresy URL mimo stránky webových formulářů. Nový výběr adresy URL pro HTML, Razor, CSS, LESS a Sass editory je dialog bez, fluent výběr zadáte, která funguje s technologií '..' a jsou uvedené filtry souborů správně img značky a odkazy.

    ![Výběr adresy URL pro značky obrázku](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Výběr adresy URL pro zobrazení](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Výběr adresy URL pro šablon stylů CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Browser Link

- Browser Link nyní podporuje připojení prostřednictvím protokolu HTTPS a bude seznam, který v řídicím panelu s další připojení, dokud je certifikát důvěryhodný prohlížeče.
- Statické mapování zdroje HTML
- Podpora SPA data mapování.
- Automatická aktualizace data mapování.

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Podpora pro Azure App Service Web Apps v sadě Visual Studio

- **Přihlaste se podpory Azure.**
- **Vzdálené ladění a vzdálené zobrazení pro webové aplikace**: teď podporujeme [vzdáleného ladění pro webové aplikace v Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) a vzdálené zobrazení souborů obsahu webové aplikace v Průzkumníku serveru.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Vytvořit vzdálené prostředky Azure při vytváření nového webového projektu

Jsme přidali Azure ["Vytvořit vzdálené prostředky"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) zaškrtávací políčko je na dialogové okno nové webové aplikace. Výběrem ho bude možné integrovat postupu při vytváření nové webové aplikace, nastavení publikování webu Azure pro účely testování a vytváření profil publikování v několika jednoduchých kroků.

![Nový projekt s prostředky Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publikování do služby Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Vylepšení publikování webu

- Zlepšení uživatelské prostředí pro publikování.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET generování uživatelského rozhraní

- **Podpora výčtu:** Pokud váš model používá výčty pak MVC Scaffolder vygeneruje rozevíracího seznamu pro příkaz Enum. Pomocníci výčtu se používá v MVC.
- **Bootstrap podporu**: aktualizované šablony EditorFor v generování uživatelského rozhraní MVC tak, aby používaly Bootstrap třídy.
- **Balíček podporu**: MVC a webových rozhraní API Scaffolders přidá 5.1 balíčky pro MVC a webového rozhraní API

Tyto snímky obrazovky ukazují modely generování uživatelského rozhraní.

- Model kódu:

     ![Model kódu](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Kompilace kódu model, klikněte pravým tlačítkem a vyberte **přidat**, **novou vygenerovanou položku**.

     ![Přidat novou položku vygenerované](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Zvolte **MVC5 řadiče se zobrazeními s využitím nástroje Entity Framework**:

     ![Přidat nový řadič MVC5 se zobrazeními](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Přidat řadič** pomocí modelu:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Zkontrolujte generovaného kódu, například obsahuje Views/WeekdayModels/Edit.cshtml `@Html.EnumDropDownListFor`: ![zobrazení obsahující EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Spuštění stránky najdete combobox výčet generuje, Všimněte si, zda hodnota může být null, prázdný řetězec může být zvolené pro pole se seznamem. Například **vytvořit** na stránce se zobrazí následující:

    ![Pole se seznamem povolení prázdný řetězec.](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1, které RTM zveřejníme. dubna 2014. Tady jsou nejdůležitějšími bodů z poznámky k verzi, ale zkontrolujte, zda [úplné poznámky](http://docs.nuget.org/docs/release-notes/nuget-2.8) Další informace o těchto změnách.

- **Cílové aplikace pro Windows Phone 8.1**: NuGet 2.8.1 teď podporuje cílení pomocí monikery cílový framework, 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' a 'WPA81' aplikace pro Windows Phone 8.1.
- **Řešení opravy pro závislosti**: při rozpoznávání závislosti balíčků, NuGet v minulosti implementovala strategie výběru nejnižší verze hlavní a podverze balíčku, který splňuje požadavky závislosti na balíčku. Na rozdíl od hlavní a vedlejší verzi ale oprav verze vždy vyřešila nejvyšší verze. I když byl úmyslného chování, vytvořit chybějících determinism pro instalaci balíčků se závislostmi.
- **Přepínač DependencyVersion**: NuGet 2.8 i když se změní *výchozí* chování pro řešení závislostí, přidá také přesnější kontrolu nad závislost procesu překladu, který přes přepínač - DependencyVersion na konzole Správce balíčků. Přepínač umožňuje řešení závislosti na nejnižší možné verze (výchozí nastavení), nejvyšší možné verze nejvyšší menší nebo verzi opravy. Tento přepínač funguje pouze pro instalaci balíčku v příkaz prostředí powershell.
- **Atribut DependencyVersion**: kromě přepínačem - DependencyVersion popsané výše, má také povolené NuGet pro možnost nastavit nový atribut v souboru nuget.config definovat, co je výchozí hodnota, pokud DependencyVersion – přepínač v k vyvolání instalace balíčku není určen. Tato hodnota musejí být dodržovány také pomocí dialogového okna Správce balíčků NuGet pro všechny operace instalace balíčku. Pokud chcete nastavit tuto hodnotu, do souboru nuget.config přidáte atribut níže:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Náhled NuGet operací s - whatif**: balíčky NuGet některá může mít grafy hloubkové závislostí a jako takový ho může být užitečné při instalaci, odinstalovat nebo aktualizovat operace nejprve zobrazit, co se stane. NuGet 2.8 přidá standardní prostředí PowerShell-co když přepnout na instalaci balíčku, odinstalace balíčku a balíček aktualizace příkazy Povolit vizualizace celý uzavření balíčky, pro které bude použito příkaz.
- **Starší verzi balíčku**: není, chcete-li prozkoumat nové funkce, nainstalujte předprodejní verze balíčku a rozhodněte se vrátit zpět poslední stabilní verze. Před NuGet 2.8 šlo vícekrokový proces odinstalace předprodejní balíček a jeho závislosti a následnou instalaci starší verze. S 2.8 NuGet ale balíčku aktualizace bude nyní vrátit zpět uzavření celého balíčku (například strom závislosti balíčku) na předchozí verzi.
- **Vývoj závislosti**: mnoho různých typů možnosti mohou být zajišťovány jako balíčky NuGet - včetně nástrojů, které se používají pro optimalizaci procesu vývoje. Tyto součásti, zatímco můžou být instrumentálního při vývoji nový balíček, by neměla považovat za publikován závislost nový balíček, pokud je novější. NuGet 2.8 umožňuje balíček identifikovat v souboru příponou .nuspec jako developmentDependency. Při instalaci, bude tato metadata přidat i do souboru packages.config v projektu, do kterého byl nainstalován balíček. Pokud tento soubor packages.config se později analyzují závislosti NuGet během nuget.exe pack, vyloučí těchto závislostí označen jako vývoj závislosti.
- **Soubor packages.config jednotlivé soubory pro různé platformy**: při vývoji aplikací pro více cílových platforem, se běžně používají různé soubory projektu pro každou z prostředí příslušných sestavení. Je také běžné využívají různé balíčků NuGet v souborech jiný projekt, jak balíčky mají různé úrovně podpory pro různé platformy. NuGet 2.8 poskytuje lepší podporu pro tento scénář vytvořením různých packages.config souborů pro soubory jiný projekt specifické pro platformu.
- **Záložní do místní mezipaměti**: z Galerie vzdálené balíčky NuGet když obvykle využívat, jako [Galerie NuGet](http://www.nuget.org) použití síťového připojení, existuje mnoho scénářů, kde klient není připojen. Bez připojení k síti nebylo možno k úspěšné instalaci balíčků - i v případě, že tyto balíčky již byly v počítači klienta v místní mezipaměti NuGet klienta NuGet. NuGet 2.8 mezipaměť automatické záložní přidá do konzoly Správce balíčků.

    Záložní funkci mezipaměti nevyžaduje žádné argumenty konkrétní příkaz. Kromě toho záložní ukládání do mezipaměti aktuálně funguje pouze v konzole Správce balíčků – chování momentálně nefunguje v dialogové okno Správce balíčků.
- **Opravy chyb**: jeden z hlavních opravy chyb provedené byl zlepšování výkonu v balíčku aktualizace-přeinstalujte příkaz.

    Kromě těchto funkcí a opravte zmíněnými výkonu tato verze NuGet také obsahuje mnoho ostatní opravy chyb. Nebyly 181 celkový problémy řešeny v verze. Úplný seznam pracovní položky pevná ve NuGet 2.8 prosím zobrazení [sledovací modul problém NuGet](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) pro tuto verzi.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET – webové formuláře

- Šablony webových formulářů nyní ukazují, jak provést potvrzení účtu a resetování hesla pro ASP.NET Identity.
- Ovládací prvek Entity zdroje dat a dynamické zprostředkovatele dat pro Entity Framework 6. Další podrobnosti naleznete v následujícím blogovém MSDN: [zprostředkovatele Dynamická Data a řízení EntityDataSource pro Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Atribut směrování vylepšení](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Zavedení podpory pro editor šablony](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Podpora výčtu v zobrazeních.](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Podpora Unobstrusive MinLength / MaxLength atributy](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Podpora kontext "this" v Nerušivý Ajax](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Různé [opravy chyb](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>Rozhraní ASP.NET Web API 2.1.2

- [Zpracování chyb globální](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Atribut směrování vylepšení](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Vylepšení stránky nápovědy](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Podpora IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formátovací modul typu média BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Lepší podporu pro asynchronní filtry](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Dotaz na Analýza pro klienta formátování knihovny](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Různé [opravy chyb](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>Rozhraní ASP.NET Web Pages 3.1.2

- Různé [opravy chyb](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Rozhraní Entity Framework 6.1

Rozhraní Entity Framework se aktualizovalo a verze 6.1 pro modul runtime a nástrojů. Entity Framework (EF) 6.1 je menší aktualizace Entity Framework 6 a zahrnuje několik oprav chyb a nové funkce. Podrobné informace o EF6.1, včetně odkazů na dokumentaci pro nové funkce, najdete v části [Entity Framework verze historie](https://msdn.microsoft.com/data/jj574253). Nové funkce v této verzi patří:

- **Tooling konsolidace** poskytuje konzistentní způsob, jak vytvořit nový model EF. Tato funkce rozšiřuje Průvodce ADO.NET Entity Data Model pro podporu vytváření Code First modely, včetně zpětnou analýzu z existující databáze. Tyto funkce byly dřív dostupné v Beta kvality v nástrojích Power EF.
- **Zpracování chyb potvrzení transakce** poskytuje nové [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) který využije nově přináší možnost zachytávat operace transakce. **CommitFailureHandler** umožňuje automatické obnovení připojení selhání při potvrzování transakce.
- **IndexAttribute** umožňuje indexy zadat umístění atribut na vlastnosti (nebo vlastnosti) v modelu Code First. Kód nejprve pak vytvoří odpovídající index v databázi.
- **Mapování veřejné rozhraní API** poskytuje přístup k informacím EF má na tom, jak vlastnosti a typy jsou namapované na sloupce a tabulky v databázi. V po vydání toto rozhraní API byla interní.
- **Možnost konfigurace sběrače přes soubor App/Web.config**(povolení sběrače přidávaného bez nutnosti rekompilace aplikace).
- **DatabaseLogger** je nové interceptoru, který usnadňuje do souboru protokolu všechny databázové operace. V kombinaci s předchozí funkce můžete tak snadno přepnout na protokolování databázových operací pro nasazené aplikace, bez nutnosti její kompilace.
- **Detekce změn modelu migrace** vylepšila se tak, aby byly přesnější; výkonu procesu zjišťování změnu je také výrazně Vylepšená vygenerované migrace.
- **Vylepšení výkonu** včetně snížené databázových operací během inicializace, optimalizace pro porovnání null rovnosti v dotazech LINQ rychleji zobrazit generace (vytvoření modelu) v další scénáře a efektivnější Vlastnost materialization sledovaných entit s více přidružení.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **Dvoufaktorové ověřování**: ASP.NET Identity teď podporuje dvoufaktorové ověřování. Dvoufaktorové ověřování poskytuje další vrstvu zabezpečení, které uživatelské účty v případě, kdy prozrazení vašeho hesla. Je také ochranu pro útoky hrubou silou na dvoufaktorové kódy.
- **Uzamčení účtu:** poskytuje způsob, jak zablokovat uživatele, pokud uživatel zadá své heslo nebo dvoufaktorové kódy nesprávně. Počet neplatných pokusů o zadání a časový interval pro se uživatelé dá nakonfigurovat. Vývojář může volitelně vypnout uzamčení účtu pro určité uživatelské účty musí potřebují k.
- **Potvrzení účtu:** ASP.NET Identity systém teď podporuje potvrzení účtu. Toto je docela běžný scénář v současné době většina webů, kde, když registrujete nový účet na webu, je nutné potvrdit e-mailu, než provedete nic na webu. Potvrzení e-mailu je užitečné, protože neplatnou účty brání vytváří. Toto je velmi užitečné, pokud používáte e-mailu jako metodu komunikaci s uživateli vašeho webu, jako jsou weby fórum, bankovnictví, elektronické obchodování nebo sociální weby.
- **Resetování hesla:** resetování hesla je funkce, kde uživatel mohou resetovat svá hesla budou zapomenou své heslo.
- **Razítko zabezpečení (odhlášení everywhere):** podporuje způsob, jak vygenerovat Token zabezpečení pro uživatele v případech, když uživatel změní své heslo nebo jiných související informace, jako je odebrání k přidružené přihlášení (jako je Facebook, Google, Účet Microsoft a tak dále). To je nutné zajistit, že jsou zneplatněny všechny tokeny vygenerovat pomocí staré heslo. V projektu vzorku Pokud změníte heslo uživatele pak se vygeneruje nový token pro uživatele a všechny předchozí tokeny, jež jsou zneplatněny. Tato funkce poskytuje další vrstvu zabezpečení do vaší aplikace, protože pokud změníte heslo, můžete se odhlásit z everywhere (všechny ostatní prohlížeče) Pokud jste přihlášení do této aplikace.
- **Ujistěte se, typ primární klíč pro uživatele a role rozšiřitelnost**: V aplikaci ASP.NET Identity 1.0 byl typ primární klíč pro tabulku uživatelů a rolí řetězce. To znamená při ASP.NET Identity systému byl v systému SQL Server pomocí rozhraní Entity Framework, jsme používali nvarchar. Došlo k mnoha diskusí kolem tato výchozí implementace na Stack Overflow a na základě názorů příchozí. Uvádíme rozšiřitelnosti háku, kde můžete určit, co by měl být primární klíč tabulky uživatelů a rolí. Toto rozšíření háku je obzvláště užitečné, že pokud provádíte migraci vaší aplikace a aplikace byla ukládání ID uživatelů jsou identifikátory GUID nebo čísla.
- **Podpora IQueryable uživatelů a rolí**: byla přidána podpora pro IQueryable na UsersStore a RolesStore, můžete snadno získat seznam uživatelů a rolí.
- **Operace odstranění podpory prostřednictvím objekt UserManager**
- **Indexování na uživatelské jméno**: implementace v ASP.NET Identity Entity Framework jsme přidali jedinečný index na uživatelské jméno pomocí nové IndexAttribute v EF 6.1.0. Tím je zajištěno, že uživatelská jména jsou vždy jedinečný a se žádné časování, ve kterém může skončili s duplicitní uživatelských jmen.
- **Validátor hesel pro rozšířené:** validátor hesel, který byl součástí ASP.NET Identity 1.0 byl validátor vcelku jednoduchá hesla, která byla pouze ověřování minimální délku. Není k dispozici nové validátor hesel, která vám dává větší kontrolu nad složitost hesla. Upozorňujeme, že i v případě, že zapnete všechna nastavení v toto heslo, doporučujeme vám povolit dvoufaktorové ověřování pro uživatelské účty.
- **IdentityFactory Middleware / CreatePerOwinContext**:

    - **Správce uživatelů**: implementace objektu Factory můžete získat instanci nastavení UserManager z kontextu OWIN. Tento vzor je podobná používáme pro získávání třídě z kontextu OWIN pro přihlášení a odhlášení. Toto je doporučený způsob, jak získat instanci nastavení UserManager každý požadavek pro aplikaci.
    - **DbContextFactory**: používá ASP.NET Identity Entity Framework pro zachování systém identit v systému SQL Server. K tomu systém identit obsahuje odkaz na ApplicationDbContext. DbContextFactory Middleware vrací instanci třídy ApplicationDbContext každý požadavek, který můžete použít v aplikaci.
- **Balíček NuGet ukázky identitu ASP.NET**: balíček NuGet ukázky můžete bylo snazší pro instalaci a spuštění ukázky pro ASP.NET Identity a použijte osvědčené postupy. Zde je ukázka aplikace ASP.NET MVC. Upravte kód tak, aby vyhovovala vaší aplikace předtím, než můžete nasadit v provozním prostředí. Ukázka by měly být nainstalovány v prázdnou aplikaci ASP.NET. Další informace o balíčku, najdete v tomto příspěvku blogu: [uvedení RTM o ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Komponenty Microsoft OWIN

Nebyly k dispozici velký počet chyb, které byly opraveny v této verzi. Najdete v tématu [poznámky k verzi pro 2.1.0 verze](https://katanaproject.codeplex.com/releases/view/113281) podrobnější informace.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>Funkce SignalR technologie ASP.NET bodu 2.0.2

Nebyly k dispozici velký počet chyb, které byly opraveny v této verzi. Podrobnosti najdete [poznámky k verzi pro bodu 2.0.2 verze](https://github.com/SignalR/SignalR/releases/tag/2.0.2) podrobnější informace.
