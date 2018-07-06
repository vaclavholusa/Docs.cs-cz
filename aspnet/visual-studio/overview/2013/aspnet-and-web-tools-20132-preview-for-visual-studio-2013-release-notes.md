---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET a webové nástroje 2013.2 pro Visual Studio 2013 – poznámky k | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: fb36d9f469265be60a7e40ed7e317d8da6560bf9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824494"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET a webové nástroje 2013.2 pro Visual Studio 2013 – poznámky k
====================
podle [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Poznámky k instalaci

ASP.NET and Web Tools pro Visual Studio 2013.2 jsou spojeny v instalačním programu pro hlavní a je možné stáhnout jako součást [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET and Web Tools pro Visual Studio 2013.2 jsou k dispozici [Web ASP.NET s](https://www.asp.net/).

## <a name="software-requirements"></a>Požadavky na software

ASP.NET and Web Tools pro Visual Studio 2013.2 vyžaduje Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Novinky v ASP.NET and Web Tools pro Visual Studio 2013.2

Následující části popisují funkce, které byly zavedeny ve vydané verzi.

- [Šablony jeden projekt ASP.NET](#oneaspnet)
- [Podpora SSL při spuštění webové aplikace ve službě IIS Express](#ssl)
- [Vylepšení webového editoru sady Visual Studio](#vswebeditor)
- [Browser Link](#browserlink)
- [Podpora pro Azure App Service Web Apps v sadě Visual Studio](#waws)
- [Při vytváření nového webového projektu vytvořit vzdálené prostředky Azure](#AzureResources)
- [Vylepšení publikování webu](#webpublish)
- [ASP.NET generování uživatelského rozhraní](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Webové formuláře ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [Rozhraní ASP.NET Web API 2.1.2](#webapi)
- [Rozhraní ASP.NET Web Pages 3.1.2](#webpages)
- [Entity Framework 6.1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Komponenty Microsoft OWIN](#owin)
- [Funkce SignalR technologie ASP.NET bodu 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Šablony jeden projekt ASP.NET

- Aktualizace šablon projektů technologie ASP.NET, které podporují potvrzení účtu a resetování hesla.
- Aktualizace šablony ASP.NET Web API pro podporu ověřování pomocí na místní účty organizace.
- Šablony ASP.NET SPA nyní obsahuje ověřování, který je založen na straně zobrazení MVC a serveru. Šablona má řadiče WebAPI, který je přístupný pouze pomocí ověřených uživatelů.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Podpora SSL při spuštění webové aplikace ve službě IIS Express

Chcete-li odstranit upozornění zabezpečení při procházení a ladění HTTPS na localhost, přidali jsme dialogové okno tak, aby umožnily aplikace Internet Explorer a Chrome důvěřovat podepsaný svým držitelem služby IIS express certifikát SSL.

Například lze nastavit vlastnosti webového projektu pro použití protokolu SSL. Klikněte na tlačítko F4 zobrazíte dialogové okno Vlastnosti. Změna **povolený protokol SSL** na hodnotu true. Zkopírujte adresu URL protokolu SSL.

![Vlastnost povolený protokol SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Sada kartě webového projektu vlastnost stránky web používat HTTPS na základě adresy URL (adresa URL protokolu SSL budou `https://localhost:44300/` Pokud jste předtím vytvořili webů SSL.)

![Nastavení adresy URL projektu (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Stisknutím kláves CTRL + F5 spusťte aplikaci. Postupujte podle pokynů a důvěřovat certifikátu podepsaného svým držitelem, který vygenerovala služba IIS Express.

![Upozornění protokolu SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Čtení **upozornění zabezpečení** dialogového okna a pak klikněte na tlačítko **Ano** Pokud chcete nainstalovat certifikát představující localhost.

![Upozornění zabezpečení](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Web, bude se zobrazovat v aplikaci Internet Explorer nebo Chrome bez upozornění certifikátu v prohlížeči.

![Stránka HTTPS bez upozornění](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox používá své vlastní úložiště certifikátů, takže se zobrazí upozornění.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Vylepšení webového editoru sady Visual Studio

- **Novou položku projektu JSON a editor**: jsme přidali položky projektu JSON a editor sady Visual Studio. Aktuální funkce editoru JSON obsahovat zabarvení, ověření syntaxe, uzavírání hranatých závorek, osnovy, nástrojů možnost nastavení a další.

    ![JSON Editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    Teď podporuje technologie IntelliSense [schématu JSON](http://json-schema.org/) v3 a v4. Neexistuje pole se seznamem schématu chcete zvolit existující schémata, upravte schéma místní cestu nebo jednoduše přetáhněte vyřadit projektu soubor JSON k ní získat relativní cestu.

    ![Technologie Intellisense JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Editor schémat JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nový editor scss Sass (–)**: jsme přidali menší ve VS2013 RTM a nyní je k dispozici editor a Sass položky projektu. Editor sass funkce jsou srovnatelné s editoru LESS a zahrnují zabarvení, proměnné a technologii IntelliSense pro objekty Mixin, okomentovat/odkomentovat, rychlé informace, formátování, ověření syntaxe, sbalení, definice přejít, výběr barev, nástroje, možnosti nastavení atd.

    ![Přidat novou položku: Šablona stylů SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor stylů](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nový nástroj pro výběr adresy URL v HTML, Razor, šablon stylů CSS, LESS a Sass dokumentů:** VS 2013, kterou jste dostali se žádný výběr adresy URL mimo stránky webových formulářů. Nový nástroj pro výběr adresy URL pro HTML, Razor, šablon stylů CSS, LESS a Sass editory je bez dialogového okna, fluent psaní ovládacího prvku pro výběr rozumí '..' a filtry souboru odpovídajícím způsobem img značky a odkazy.

    ![Výběr adresy URL pro značky obrázku](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Výběr adresy URL pro zobrazení](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Výběr adresy URL pro šablony stylů CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Aktualizace pro editor LESS tak, že přidáte další funkce**
- **Knockout Intellisense Upgrade**: nestandardní syntaxe KnockOut pro VS intelliSense, přidali "viewModel ko vs editor:" syntaxi. Je možné vytvořit vazbu s více modely zobrazení na stránce pomocí komentářů ve tvaru:

    ![Technologie Intellisense knockout](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Také přidali jsme podporu pro vnořené ViewModel IntelliSense tak může přejít do hluboce vnořených objektů na ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    IntelilSense, zobrazí se plnou podporou technologie IntelliSense jazyka JavaScript objektu.

    ![Technologie IntelliSense zobrazuje úplný objekt jazyka JavaScript](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nový nástroj pro výběr adresy URL v HTML, Razor, šablon stylů CSS, LESS a Sass dokumenty**: VS 2013, kterou jste dostali se žádný výběr adresy URL mimo stránky webových formulářů. Nový nástroj pro výběr adresy URL pro HTML, Razor, šablon stylů CSS, LESS a Sass editory je bez dialogového okna, fluent psaní ovládacího prvku pro výběr rozumí '..' a filtry souboru odpovídajícím způsobem img značky a odkazy.

    ![Výběr adresy URL pro značky obrázku](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Výběr adresy URL pro zobrazení](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Výběr adresy URL pro šablony stylů CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Browser Link

- Browser Link nyní podporuje připojení prostřednictvím protokolu HTTPS a zobrazí seznam, který na řídicím panelu s další připojení tak dlouho, dokud je certifikát důvěryhodný pro prohlížeč.
- Statické mapování zdroje HTML
- Jednostránková aplikace podporu pro mapování dat
- Automatické aktualizace mapovacích dat

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Podpora pro Azure App Service Web Apps v sadě Visual Studio

- **Přihlaste se podpory Azure.**
- **Vzdálené ladění a zobrazení pro web apps vzdálený**: teď podporujeme [vzdálené ladění webových aplikací ve službě Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) a vzdálené zobrazení souborů obsahu webové aplikace v Průzkumníku serveru.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Při vytváření nového webového projektu vytvořit vzdálené prostředky Azure

Přidali jsme Azure ["Vytvořit vzdálené prostředky"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) zaškrtávací políčko na dialogové okno nové webové aplikace. Kliknutím na něj, bude možné integrovat funkce vytvoření nové webové aplikace, nastavení publikování webu Azure pro účely testování a vytváří se profil publikování v pár jednoduchých kroků.

![Nový projekt s prostředky Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publikování do Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Vylepšení publikování webu

- Vylepšete uživatelské prostředí pro publikování.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET generování uživatelského rozhraní

- **Podpora výčtu:** Pokud váš model používá výčty pak generátor MVC vygeneruje rozevírací seznam pro výčet. Tato funkce využívá pomocné rutiny výčtu v aplikaci MVC.
- **Podpora Bootstrap**: aktualizované šablony EditorFor v generování uživatelského rozhraní MVC, tak, aby používaly Bootstrap třídy.
- **Balíček podpory**: MVC a webového rozhraní API podpůrné přidá 5.1 balíčky pro MVC a webového rozhraní API

Na následujících snímcích obrazovky ukazují modely generování uživatelského rozhraní.

- Model kódu:

     ![Model kódu](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Zkompilovat kód modelu, klikněte pravým tlačítkem a vyberte **přidat**, **novou vygenerovanou položku**.

     ![Přidat nová vygenerovaná položka](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Zvolte **MVC5 kontroler se zobrazeními, používá nástroj Entity Framework**:

     ![Přidat nový kontroler MVC5 se zobrazeními](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Přidat kontroler** pomocí modelu:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Zkontrolujte vygenerovaný kód, například obsahuje Views/WeekdayModels/Edit.cshtml `@Html.EnumDropDownListFor`: ![zobrazení obsahuje EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Spustíte na stránce chcete podívat na pole se seznamem výčtového typu vygeneruje, Všimněte si, že je-li hodnota může mít hodnotu null, prázdný řetězec může být vybrán pro pole se seznamem. Například **vytvořit** stránce se zobrazí následující:

    ![Pole se seznamem povolení prázdný řetězec](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1, které budou RTM vydané v duben 2014. Tady jsou důležité body z zpráva k vydání verze, ale Zkontrolujte prosím, [kompletní poznámky k verzi](http://docs.nuget.org/docs/release-notes/nuget-2.8) pro další informace o těchto změnách.

- **Cílení na Windows Phone 8.1 aplikace**: NuGet 2.8.1 teď podporuje cílení na Windows Phone 8.1 aplikací pomocí rozhraní framework monikery target "WindowsPhoneApp", "WPA", "WindowsPhoneApp81" a "WPA81".
- **Řešení opravy pro závislosti**: při překladu závislosti balíčků NuGet v minulosti implementoval strategie výběr nejnižší verze hlavní a dílčí balíček, který splňuje požadavky závislosti na balíčku. Na rozdíl od hlavní a dílčí verze ale verze opravy vždy přeložila na nejvyšší verzi. I když byla úmyslného chování, vytvoří nedostatku determinismus pro instalaci balíčků se závislostmi.
- **Přepínač DependencyVersion**: NuGet 2.8 i když se změní *výchozí* chování pro vyřešení závislostí, také přidá přesnější kontrolu nad procesem řešení závislostí prostřednictvím přepínače - DependencyVersion v konzole Správce balíčků. Přepínač umožňuje řešení závislostí na nejnižší možné verze (výchozí chování), nejvyšší možné verzi, nebo nejvyšší podverze nebo verzi opravy. Tento přepínač funguje jenom pro install-package v příkazu prostředí powershell.
- **Atribut DependencyVersion**: kromě přepínačem - DependencyVersion výše popsané, má také povoleny NuGet pro možnost nastavit v souboru nuget.config definující, co je výchozí hodnota, pokud DependencyVersion – nový atribut není zadán v vyvolání install-package. Tato hodnota se také budou dodržovat i dialogové okno Správce balíčků NuGet pro všechny operace instalace balíčku. Nastavení této hodnoty, přidejte do souboru nuget.config atribut níže:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Ve verzi Preview NuGet operací s - whatif**: balíčky NuGet některé může mít hloubkové závislosti grafy, a v důsledku toho může být užitečné při instalaci, odinstalovat nebo aktualizovat operace nejprve naleznete v tématu co se stane. NuGet 2.8 přidá standardní prostředí PowerShell – co když přepnout na příkazy install-package, odinstalace balíčku a balíček aktualizace umožňující vizualizaci celého uzavření balíčky, na které se použije příkaz.
- **Downgrade balíčku**: není, chcete-li nainstalovat zkušební verzi balíčku, aby prozkoumat nové funkce a potom se rozhodnete vrátit zpět poslední stabilní verzi. Před 2.8 NuGet to bylo vícefázový proces odinstalace předprodejní balíček a jeho závislosti a následná instalace předchozí verzi. S NuGet 2.8 však balíčku aktualizace bude nyní vrátit zpět uzavření celého balíčku (například strom závislostí balíčku) k předchozí verzi.
- **Vývoj závislosti**: mnoho různých typů funkcí můžete dodávány jako balíčky NuGet – včetně nástrojů, které se používají pro optimalizaci procesu vývoje. Tyto komponenty, zatímco mohou být přispěly k vývoji nového balíčku, by neměly být zahrnuté publikované závislost nového balíčku, pokud je novější. NuGet 2.8 umožňuje balíček identifikovat v souboru .nuspec souboru jako developmentDependency. Při instalaci, se tato metadata přidají také do souboru packages.config projektu, do kterého byl nainstalován balíček. Když tento soubor packages.config se později analyzuje závislostí NuGet během nuget.exe pack, vyloučí tyto závislosti označené jako vývoj závislosti.
- **Soubor packages.config jednotlivých souborů pro různé platformy**: při vývoji aplikací pro více cílových platforem, je běžné mít různé soubory projektu pro každé prostředí příslušné sestavení. Je také běžné využívání různých balíčků NuGet v různé soubory projektu, jak balíčky mají různé úrovně podpory pro různé platformy. NuGet 2.8 poskytuje vylepšenou podporu pro tento scénář vytvořením různých souboru packages.config souborů pro soubory projektu různých specifické pro platformu.
- **Do místní mezipaměti**: balíčky NuGet i když jsou obvykle spotřebované vzdálené galerie, jako [Galerie NuGet](http://www.nuget.org) pomocí připojení k síti, existuje mnoho scénářů, ve kterém není klient připojen. Bez připojení k síti nebylo možné úspěšně nainstalovat balíčky – i v případě, že již byly tyto balíčky na klientském počítači v místní mezipaměti NuGet klienta NuGet. NuGet 2.8 mezipaměť automatické použití náhradní lokality přidá do konzoly Správce balíčků.

    Náhradní funkce mezipaměti nevyžaduje žádné argumenty konkrétní příkaz. Kromě toho mezipaměti použití náhradní lokality v současné době používá jenom v konzole Správce balíčků – chování aktuálně nefunguje v dialogové okno Správce balíčků.
- **Opravy chyb**: byl jeden ze hlavní opravy provedli zlepšování výkonu v balíčku aktualizace-přeinstalujte příkazu.

    Kromě těchto funkcí a opravte výše uvedené výkonu tato verze NuGet obsahuje také mnoho ostatní opravy chyb. Došlo k 181 celkový problémy zákazníky a vyřešené ve verzi. Úplný seznam pracovní položky opravených NuGet 2.8 prosím zobrazení [sledování problémů NuGet](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) pro tuto verzi.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET – webové formuláře

- Šablony webových formulářů teď ukazují, jak provést potvrzení účtu a resetování hesla pro ASP.NET Identity.
- Ovládací prvek Entity Data Source a dynamické zprostředkovatel dat pro Entity Framework 6. Další podrobnosti najdete v následujícím blogu MSDN: [poskytovatele dynamických dat a ovládací prvek třídu EntityDataSource platí pro Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Atribut směrování vylepšení](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Zavedení podpory pro editor šablon](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Podpora výčtu v zobrazeních.](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Podpora Unobstrusive MinLength nebo MaxLength atributy](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Podpora 'this' kontextu v Nerušivého jazyka Ajax](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Různé [opravy chyb](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>Rozhraní ASP.NET Web API 2.1.2

- [Zpracování chyb globální](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Atribut směrování vylepšení](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Vylepšení stránky nápovědy](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Podpora IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formátovací modul typu média BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Lepší podporu pro asynchronní filtry](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Dotazování, analýza kódu pro formátování knihovna klienta](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Různé [opravy chyb](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>Rozhraní ASP.NET Web Pages 3.1.2

- Různé [opravy chyb](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework byla aktualizována na verze 6.1 pro modul runtime a nástroje. Entity Framework (EF) 6.1 je menší aktualizace Entity Framework 6 a zahrnuje celou řadu oprav chyb a nové funkce. Podrobné informace o EF6.1, včetně odkazů na dokumentaci k nové funkce, najdete v části [historie verzí Entity Framework](https://msdn.microsoft.com/data/jj574253). Mezi nové funkce v této verzi patří:

- **Nástroje konsolidace** poskytuje konzistentní způsob, jak vytvořit nový model EF. Tato funkce rozšiřuje průvodce datový Model Entity ADO.NET pro podporu vytváření Code First modelů, včetně zpětné analýzy z existující databáze. Tyto funkce byly dříve k dispozici v beta verzi kvality v EF Power Tools.
- **Zpracování chyb potvrzení transakce** poskytuje nové [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) která využívá nově zavedená možnost zachycení operace transakce. **CommitFailureHandler** umožňuje automatické obnovení v případě selhání připojení, zatímco potvrzení transakce.
- **IndexAttribute** umožňuje indexů možné zadat tak, že atribut na vlastnosti (nebo vlastnosti) v modelu Code First. Kód nejprve pak vytvoří odpovídající index v databázi.
- **Mapování veřejného rozhraní API** poskytuje přístup k informacím EF má na typy a vlastnosti zpřístupněných sloupců a tabulek v databázi. V minulých verzích tohoto rozhraní API byla interní.
- **Možnost konfigurace sběrače prostřednictvím souboru App/Web.config**(povoluje sběrače přidávané bez nutnosti znovu kompilovat aplikace).
- **DatabaseLogger** je nový sběrač, se kterou snadno protokolovat všechny operace databáze do souboru. V kombinaci s předchozí funkce díky tomu můžete snadno zapnout protokolování databázové operace pro nasazenou aplikaci, aniž by bylo nutné znovu zkompilovat.
- **Detekce změn modelu migrace** jsme vylepšili tak, aby byly přesnější; výkon samotného procesu zjišťování změn je také výrazně Vylepšená vygenerované migrace.
- **Vylepšení výkonu** včetně nižší databázové operace při inicializaci, optimalizace pro porovnání rovnosti null v dotazech LINQ, rychleji zobrazit. generace (vytvoření modelu) ve více scénářích, přehledné a efektivnější materializace sledované entit s několika přidružení.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **Dvoufaktorové ověřování**: ASP.NET Identity teď podporuje dvojúrovňového ověřování. Dvoufaktorové ověřování poskytuje další vrstvu zabezpečení s vašimi uživatelskými účty v případě, kdy narušení heslo. Je také ochranu pro útoky hrubou silou na dvoufaktorové kódy.
- **Uzamčení účtu:** poskytuje způsob, jak uzamknout uživatele, pokud uživatel zadá své heslo nebo dvoufaktorové kódy nesprávně. Je možné nakonfigurovat počet neplatných pokusů o zadání a časový interval pro uživatele je uzamčen. Vývojář můžete volitelně vypnout uzamčení účtu pro některé uživatelské účty by měly potřebují.
- **Potvrzení účtu:** ASP.NET Identity systém nyní podporuje potvrzení účtu. To je poměrně běžný scénář v současné době většina webů, kdy, když si zaregistrujete nový účet na webu, se vyžaduje k potvrzení vašeho e-mailu, než to můžete udělat něco na webu. Potvrzení e-mailu je užitečné, protože se zabraňuje vytváření fiktivní účty. To je velmi užitečné, pokud používáte e-mailu jako metoda komunikace s uživateli vašeho webu, jako jsou weby fórum, bankovnictví, elektronického obchodování nebo sociální weby.
- **Resetování hesla:** resetování hesla je funkce, kde uživatel můžete resetovat svá hesla, zapomenou své heslo.
- **Razítko zabezpečení (odhlášení všude):** podporuje způsob, jak znovu vygenerovat Token zabezpečení pro uživatele v případech, když uživatel změní heslo nebo jiné zabezpečení související informace, jako je odebrání přidružené přihlášení (například Facebook, Google Účet Microsoft a tak dále). To je potřeba zajistit, že nejsou zneplatněny žádné tokeny vygenerovat pomocí starého hesla. V ukázkového projektu a pokud změníte heslo uživatele potom se vygeneruje nový token pro uživatele a všechny předchozí tokeny, jež nejsou zneplatněny. Tato funkce poskytuje další úroveň zabezpečení pro vaši aplikaci, protože pokud změníte svoje heslo, které se odhlásíte z jakéhokoliv místa (všechny ostatní prohlížeče) Pokud jste přihlášení do této aplikace.
- **Učiňte typ primární klíč pro uživatele a role rozšiřitelnost**: V ASP.NET Identity 1.0 byl typ primárního klíče pro tabulku uživatelů a rolí řetězce. To znamená, když systém identit technologie ASP.NET byl trvale uložen v systému SQL Server s použitím Entity Framework, kdybychom používali nvarchar. Došlo k mnoha diskuse kolem tato výchozí implementace na webu Stack Overflow a na základě příchozí zpětné vazby. Uvádíme rozšiřitelnosti hook, kde můžete určit, co by měl být primární klíč tabulky uživatelů a rolí. Toto rozšíření zavěšení je zvlášť užitečné, že pokud provádíte migraci vaší aplikace a aplikace byla ukládání ID uživatelů jsou identifikátory GUID nebo čísla na hodnoty.
- **Podpora IQueryable uživatelů a rolí**: Přidání podpory pro typ IQueryable na UsersStore a RolesStore můžete snadno získat seznam uživatelů a rolí.
- **Operace odstranění podpory prostřednictvím objektu UserManager.**
- **Indexování na uživatelské jméno**: implementace v ASP.NET Identity Entity Framework, jsme přidali jedinečný index na uživatelské jméno pomocí nové IndexAttribute v EF 6.1.0. Tím je zajištěno, že jsou vždy jedinečná uživatelská jména a se žádné časování, ve kterém může skončit s duplicitní uživatelských jmen.
- **Validátor hesel pro rozšířené:** validátor hesel, který byl součástí technologie ASP.NET Identity 1.0 byl validátor vcelku hesel, která byla pouze ověřování minimální délku. Existuje nový validátor hesel, která vám dává větší kontrolu nad složitost hesla. Mějte prosím na paměti, že i v případě, že zapnete všechna nastavení v toto heslo, doporučujeme vám povolit dvoufaktorové ověřování uživatelských účtů.
- **IdentityFactory Middleware / CreatePerOwinContext**:

    - **Správce uživatelů**: implementace objektu Factory můžete použít k získání instance objektu UserManager. z kontextu OWIN. Tento model je podobný používáme k zařazení správce AuthenticationManager pro přihlášení a odhlášení z kontextu OWIN. Toto je doporučený způsob získání instance objektu UserManager. každý požadavek pro aplikaci.
    - **DbContextFactory**: ASP.NET Identity používá Entity Framework pro trvalé systém identit v systému SQL Server. K tomu systém identit obsahuje odkaz na ApplicationDbContext. DbContextFactory Middleware vrací instanci ApplicationDbContext každý požadavek, který vám pomůže v aplikaci.
- **Balíček NuGet ukázky identitu ASP.NET**: ukázky balíček můžete usnadnit instalaci a spuštění ukázek pro ASP.NET Identity a dodržujte doporučené postupy zabezpečení. Toto je ukázková aplikace ASP.NET MVC. Upravte kód tak, aby vyhovovala vaší aplikace předtím, než můžete nasadit v produkčním prostředí. Ukázka musí být nainstalován v prázdnou aplikaci ASP.NET. Další informace o balíčku, najdete v následujícím příspěvku blogu: [oznamujeme vydání RTM sady ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Komponenty Microsoft OWIN

Došlo k velké množství chyb, které byly opraveny v této verzi. Podrobnosti najdete [poznámky k verzi pro 2.1.0 vydání](https://katanaproject.codeplex.com/releases/view/113281) podrobnější informace.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>Funkce SignalR technologie ASP.NET bodu 2.0.2

Došlo k velké množství chyb, které byly opraveny v této verzi. Podrobnosti najdete [poznámky k verzi pro bodu 2.0.2 vydání](https://github.com/SignalR/SignalR/releases/tag/2.0.2) podrobnější informace.
