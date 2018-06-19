---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 0bfe9cdc215226457ccfafff2b85ace87325b91b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
ms.locfileid: "30899100"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [Přehled](#overview)
- [Poznámky k instalaci](#installation-notes)
- [Požadavky na software](#software-requirements)
- [Dokumentace](#documentation)
- [Podpora](#support)
- [Upgrade projektu aplikace ASP.NET MVC 2 do architektury ASP.NET MVC 3 nástroje aktualizace](#upgrading)
- [ASP.NET MVC 3 Tools Update (April 12, 2011)](#tu-changes)

    - [Dialogové okno "Přidat řadič" teď můžete vygenerovat řadiče kódem přístup k zobrazení a data](#tu-AddControllerDialog)
    - [Vylepšení "rozhraní ASP.NET MVC 3 nový projekt" dialogové okno](#tu-ImprovementsNewDialogBox)
    - [Šablony projektů nyní zahrnují Modernizr 1.7](#tu-Modernizr)
    - [Šablony projektů zahrnují aktualizované verze jQuery, jQuery uživatelského rozhraní a jQuery ověření](#tu-UpdatedJQuery)
    - [Šablony projektů nyní zahrnují ADO.NET Entity Framework 4.1 jako balíčku NuGet, předem nainstalovaná](#tu-EF)
    - [Šablony projektů zahrnují knihovny JavaScript jako předem nainstalované balíčky NuGet](#tu-JavaScriptLibsNuget)
    - [Známé problémy](#tu-KI)
- [ASP.NET MVC 3 RTM (January 13, 2011)](#MVC3RTM)

    - [Změna: Aktualizovat verzi jQuery uživatelského rozhraní na 1.8.7](#RTM-1)
    - [Změny: Změnit výchozí ModelMetadataProvider zpět do DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Opravené: Vkládání část výrazu Razor, který obsahuje výsledky prázdných znaků v ní probíhá vrácení zpět](#RTM-3)
    - [Opravené: Přejmenování souboru Razor, který je otevřen v editoru zakáže zabarvení syntaxe a IntelliSense](#RTM-4)
    - [Známé problémy](#RTM-KI)
    - [Nejnovější změny](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 (10. prosince 2010)](#_Toc2)

    - [Projekt šablony změnit, například jQuery 1.4.4, jQuery 1.7 ověření a jQuery UI 1.8.6y 1.8.6 uživatelského rozhraní](#_Toc2_1)
    - [Třídy přidané "AdditionalMetadataAttribute"](#_Toc2_2)
    - [Vylepšené zobrazení generování uživatelského rozhraní](#_Toc2_3)
    - [Přidání Html.Raw – metoda](#_Toc2_3)
    - [Vlastnost přejmenovat "Controller.ViewModel" a vlastnost "Zobrazit" na "ViewBag"](#_Toc2_4)
    - [Třída přejmenovat "ControllerSessionStateAttribute" na "SessionStateAttribute"](#_Toc2_5)
    - [Vlastnost přejmenovat RemoteAttribute "Pole" na "AdditionalFields"](#_Toc2_6)
    - [Přejmenovat "SkipRequestValidationAttribute" na "AllowHtmlAttribute"](#_Toc2_7)
    - [Změněné "Html.ValidationMessage" metodu pro zobrazení první užitečné chybovou zprávu](#_Toc2_8)
    - [Opravené @model deklarace není prázdné přidat do dokumentu](#_Toc2_9)
    - [Vlastnost přidané "FileExtensions" pro moduly zobrazení pro podporu názvy souborů specifické pro modul](#_Toc2_10)
    - [Opravené "LabelFor" Pomocníka pro vydávání správnou hodnotu pro atribut "Pro"](#_Toc2_11)
    - [Metoda dlouhodobého "RenderAction" má přednost explicitní hodnoty během vazby modelu](#_Toc2_12)
    - [Nejnovější změny](#_Toc2_BC)
    - [Známé problémy](#_Toc2_KI)
- [ASP.NET MVC 3 Release Candidate (9 listopadu 2010)](#TOC_ASP_NET_3_RC)

    - [Nové funkce v rozhraní ASP.NET MVC 3 RC](#_Toc276711785)
    - [Správce balíčků NuGet](#_Toc276711786)
    - [Vylepšené "Nový projekt" dialogové okno](#_Toc276711787)
    - [Nerelační řadiče](#_Toc276711788)
    - [Nové atributy ověření](#_Toc276711789)
    - [Nové přetížení metod "LabelForModel" a "LabelFor"](#_Toc276711790)
    - [Podřízená akce ukládání výstupu do mezipaměti](#_Toc276711791)
    - ["Přidat zobrazení" Vylepšená dialogová okna](#_Toc276711792)
    - [Ověření granulární žádosti](#_Toc276711793)
    - [Nejnovější změny](#_Toc276711794)
    - [Známé problémy](#_Toc276711795)
- [SOUBOR ASP. MVC 3 Beta zaznamená (6 Oct 2010)](#TOC_ASP_NET_3_Beta)

    - [Nové funkce ve verzi Beta rozhraní ASP.NET MVC 3](#0.1__Toc274034215)
    - [Správce balíčků NuPack](#0.1__Toc274034216)
    - [Vylepšené dialogové okno Nový projekt](#0.1__Toc274034217)
    - [Jednodušší způsob, jak určit silně typované modely v zobrazení syntaxe Razor](#0.1__Toc274034218)
    - [Podporu pro nové webové stránky ASP.NET pomocné metody](#0.1__Toc274034219)
    - [Podpora vkládání Další závislosti](#0.1__Toc274034220)
    - [Nová podpora Nerušivý na základě jQuery Ajax](#0.1__Toc274034221)
    - [Nová podpora Nerušivý jQuery ověření](#0.1__Toc274034222)
    - [Nové příznaky celou aplikaci pro ověření klienta a Nerušivý JavaScript](#0.1__Toc274034223)
    - [Nová podpora kód, který běží před spuštěním zobrazení](#0.1__Toc274034224)
    - [Nová podpora pro syntaxi Razor VBHTML](#0.1__Toc274034225)
    - [Podrobnější kontrolu nad atribut ValidateInputAttribute](#0.1__Toc274034226)
    - [Pomocníci převádějí podtržítka pomlčky pro názvy atributů HTML zadán pomocí anonymní objekty](#0.1__Toc274034227)
    - [Opravy chyb](#0.1__Toc274034228)
    - [Nejnovější změny](#0.1__Toc274034229)
    - [Známé problémy](#0.1__Toc274034230)
- [Právní omezení](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Přehled

Tento dokument popisuje verzi RTM, ASP.NET MVC 3 pro Visual Studio 2010. ASP.NET MVC je rozhraní pro vývoj webových aplikací, která používá vzor Model-View-Controller (MVC). Instalační program ASP.NET MVC 3 zahrnuje následující součásti:

- Součásti režimu runtime ASP.NET MVC 3
- ASP.NET MVC 3 Visual Studio 2010 tools
- Součásti spuštění webové stránky ASP.NET
- Nástroje sady Visual Studio 2010 webové stránky ASP.NET
- Správce balíčků Microsoft pro platformu .NET (NuGet)
- Aktualizace pro sadu Visual Studio 2010, která umožňuje podporu pro syntaxi Razor. (Podrobnosti najdete v tématu Knowledge Base, článek 2483190.)

Kompletní poznámky k verzi pro každou předběžné verzi ASP.NET MVC 3 najdete na webu ASP.NET na následující adrese URL:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Poznámky k instalaci

Pokud chcete nainstalovat, ASP.NET MVC 3 RTM pomocí instalačního programu webové platformy (instalace webové platformy), naleznete na následující stránce:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Alternativně můžete stáhnout instalační program pro ASP.NET MVC 3 RTM pro Visual Studio 2010 na následující stránce:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 je možné nainstalovat a spustit souběžně sdílená s ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Požadavky na software

Komponenty ASP.NET MVC 3 spuštění vyžadovat následující software:

- Rozhraní .NET framework verze 4. 

    ASP.NET MVC 3 Visual Studio 2010 tools vyžadují následující software:
- Visual Studio 2010 a Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentace

Dokumentace k rozhraní ASP.NET MVC je k dispozici na webu MSDN na následující adrese URL:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Kurzy a další informace o architektuře ASP.NET MVC jsou k dispozici na stránce MVC webu ASP.NET na následující adrese URL:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Podpora

Toto je plně podporované verze. Informace o získání technické podpory naleznete na [web Microsoft Support](https://support.microsoft.com/).

Klidně také odeslání dotazy týkající se této verze na fóru ASP.NET MVC, kde jsou často schopen poskytnout neformální podporu členové komunity služby ASP.NET:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Upgrade projektu aplikace ASP.NET MVC 2 do architektury ASP.NET MVC 3 nástroje aktualizace

ASP.NET MVC 3 lze nainstalovat node souběžně s ASP.NET MVC 2 na stejném počítači, který poskytuje flexibilitu při volbě, když chcete upgradovat aplikaci ASP.NET MVC 2 ASP.NET MVC 3.

Pokud chcete ručně upgradovat stávající aplikace ASP.NET MVC 2 do verze 3, postupujte takto:

1. Vytvoření nového projektu ASP.NET MVC 3 prázdný ve vašem počítači. Tento projekt bude obsahovat některé soubory, které jsou požadovány pro upgrade.
2. Zkopírujte následující soubory z projektu ASP.NET MVC 3 do odpovídající umístění projektu ASP.NET MVC 2. Budete muset aktualizovat všechny odkazy na knihovny jQuery, aby se zohlednily nový název souboru (jQuery-1.5.1.js): 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*.js
    - / Obsah nebomotivy/\*.\*
3. Kopírování *balíčky* složku v kořenovém prázdný projekt ASP.NET MVC 3 řešení do kořenové řešení, která je v adresáři, kde je umístěn soubor .sln na řešení.
4. Pokud projektu ASP.NET MVC 2 obsahuje všechny oblasti, zkopírujte soubor /Views/Web.config *zobrazení* složky každou oblast.
5. V obou souborech Web.config v projektu ASP.NET MVC 2 globálně vyhledávání a nahrazení verze ASP.NET MVC. Najděte následující: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Nahraďte ho následujícím textem:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. V Průzkumníku řešení, odstraňte odkaz na *System.Web.Mvc* (která poukazuje na knihovnu DLL z verze 2), pak přidejte odkaz na *System.Web.Mvc* (v3.0.0.0).
7. Přidáte odkaz na System.Web.WebPages.dll a System.Web.Helpers.dll. Tyto sestavení jsou umístěny v následující složky: 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET 3\Assemblies MVC
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET webové Pages\v1.0\Assemblies
8. V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a vyberte Uvolnit projekt. Potom znovu klikněte pravým tlačítkem na název projektu a vyberte Upravit *ProjectName*.csproj.
9. Vyhledejte *ProjectTypeGuids* elementu a nahraďte {E53F8FEA-EAE0-44A6-8774-FFD645390401} {F85E285D-A4E0-4152-9332-AB1D724D3325}.
10. Uložte změny, klikněte pravým tlačítkem na projekt a potom vyberte znovu načíst projekt.
11. V kořenovém souboru Web.config aplikace, přidejte následující nastavení, která *sestavení* části. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Pokud projekt odkazuje na knihovny jakékoli třetí strany, kompilovaná pomocí ASP.NET MVC 2, přidejte následující zvýrazněnou *bindingRedirect* element v souboru Web.config v kořenovém adresáři aplikace v rámci * konfigurace* části: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Změny v architektuře ASP.NET MVC 3 aktualizace nástroje

Tato část popisuje změny ve verzi ASP.NET MVC 3 nástroje pro aktualizace od vydání ASP.NET MVC 3 RTM.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Dialogové okno "Přidat řadič" teď můžete vygenerovat řadiče kódem přístup k zobrazení a data

Generování uživatelského rozhraní je způsob, jak rychle generování řadič a zobrazení pro vaši aplikaci. Po vygenerování kódu, můžete upravit tak, aby vyhovovala požadavkům vašeho projektu.

Ke spuštění *přidat kontroler* dialogové okno technologie ASP.NET MVC 3, klikněte pravým tlačítkem myši *řadiče* složky v *Průzkumníku řešení*, klikněte na tlačítko *přidat*a potom klikněte na *řadič*. Dialogové okno je vylepšená nabídka možnosti Další generování uživatelského rozhraní.

![](mvc3-release-notes/_static/image1.png)

Ve výchozím nastavení jsou k dispozici tři šablony generování uživatelského rozhraní.

#### <a name="empty-controller"></a>Prázdný kontroler

Tato šablona generuje soubor prázdný kontroler. Tato šablona je ekvivalentní není kontrola *akce pro vytvořit, upravit, podrobnosti přidávat, odstraňovat scénáře* v předchozích verzích rozhraní ASP.NET MVC. Pokud si zvolíte, jsou k dispozici žádné další možnosti.

#### <a name="controller-with-empty-readwrite-actions"></a>Kontroler s akcemi čtení/zápisu prázdný

Tato šablona generuje soubor kontroleru, který má všechny metody požadované akce, ale žádný kód implementace v metody. Tato šablona je ekvivalentní kontrola *akce pro vytvořit, upravit, podrobnosti přidávat, odstraňovat scénáře* v předchozích verzích rozhraní ASP.NET MVC. Pokud si zvolíte, jsou k dispozici žádné další možnosti.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Kontroler s akcemi čtení/zápisu a zobrazeními, s využitím nástroje Entity Framework

Tato šablona umožňuje rychle vytvořit pracovní zadávání dat uživatelské rozhraní. Vygeneruje kód, který zpracovává řadu běžných požadavky a například následující scénáře:

- *Přístup k datům*. Generovaný kód čte a zapisuje entity v databázi. Funguje s Entity Framework Code First přístup, pokud si zvolíte existující třídy kontextu dat, nebo pokud nechat vygenerovat novou šablonu *DbContext* třídy. Spolupracuje také s přístupem Entity Framework Database First nebo Model First Pokud zvolíte existující *ObjectContext* třídy.
- *Ověření*. Generovaný kód používá vazby modelu ASP.NET MVC a metadata funkce tak, aby odeslání formuláře se ověřují podle pravidel deklarovaný na vaší třídě modelu. To zahrnuje předdefinovaná pravidla ověřování, například *požadované* a *StringLength* atributy a vlastní pravidla ověřování.
- *Vztahy na více*. Pokud definujete relace cizího klíče na více mezi tříd modelu, generovaný kód vytvoří rozevíracího seznamu pro výběr entit v relaci. Například může definovat následující třídy modelu Entity Framework Code First konvencemi: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Když pak generujete řadič pro *produktu* třída, jeho zobrazení umožní uživatelům vybrat *kategorie* objekt pro každou *produktu* instance.

    Tato šablona umožňuje další možnosti v *přidat kontroler* dialogové okno. Pro *třída modelu*, můžete zvolit všechny třídy modelu ve vašem řešení, která určuje typ dat, které budou uživatelé moci vytvořit nebo upravit:
- Pokud chcete použít Entity Framework Code First, můžete vybrat všechny třídy modelu.
- Pokud používáte Entity Framework Database First nebo model Entity Framework Model First, je nutné zvolit třídu entity definovanou v konceptuálním modelu.

Pro *třída kontextu dat*, můžete použít tyto možnosti:

- Pokud chcete použít Code First a mít žádné existující data kontextu třídy, vyberte ** nový kontext dat **. Třída kontextu dat se budou generovat pak za vás.
- Pokud chcete pomocí Code First a máte existující třídy kontextu dat, vyberte ho sem. Aktualizují se zachovat třídu modelu, který jste zvolili.
- Pokud používáte Database First nebo Model First, zvolte třídě kontextu objektu sem.

Pro zobrazení zvolte zobrazovací modul, který chcete použít, nebo zvolte None, pokud nechcete, aby chcete vygenerovat žádné zobrazení.

Můžete vybrat rozšířené Optionsto zadat další možnosti pro vygenerovaných zobrazení. Můžete například rozložení nebo stránku předlohy k použití.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Vylepšení "rozhraní ASP.NET MVC 3 nový projekt" dialogové okno

Dialogové okno, které použijete k vytvoření nové projekty ASP.NET MVC 3 obsahuje několik vylepšení, jak je uvedeno dále.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Nové šablony "Intranetový projekt"

Šablona projektu seznam obsahuje novou šablonu aplikace intranetu. Tato šablona obsahuje nastavení pro vytváření webovou aplikaci pomocí ověřování systému Windows místo ověřování pomocí formulářů. Protože aplikace sítě intranet vyžaduje některá nastavení služby IIS, která nemůže být zapouzdřené v šabloně projektu, šablona obsahuje soubor readme s pokyny, jak zajistit, že šablona projektu fungovat ve službě IIS. Dokumentace pro novou šablonu intranetu aplikace je k dispozici na webu MSDN na následující adrese URL:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Šablony projektů jsou nyní HTML5 povoleno

Dialogové okno Nový projekt nyní obsahuje možnost Přidat funkce specifické pro HTML5 šablon projektu. Když vyberete možnost, budou zobrazení má být vygenerován, které obsahují novou HTML5 `<header>`, `<footer>`, a `<navigation>` elementy.

Všimněte si, že starší verze prohlížečů nepodporují značky specifické pro HTML5. Chcete-li toto omezení vyřešit, šablony projektů jazyka HTML5 obsahovat odkaz na knihovnu Modernizr. (Viz další část).

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Šablony projektů nyní zahrnují Modernizr 1.7

Modernizr je knihovna JavaScript, která umožňuje podporu šablon stylů CSS 3 a HTML5 v prohlížečích, které zatím nepodporují tyto funkce. Tato knihovna je zahrnuta jako předem nainstalovaná balíček NuGet v rámci šablon pro projekty ASP.NET MVC 3. Další informace o Modernizr najdete v tématu [ http://www.modernizr.com/ ](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Šablony projektů zahrnují aktualizované verze jQuery, jQuery uživatelského rozhraní a jQuery ověření

Šablony projektů nyní zahrnují následující verze technologie jQuery skriptů:

- jQuery 1.5.1
- jQuery 1.8 ověření
- jQuery UI 1.8.11

Tyto knihovny jsou k dispozici jako předem nainstalované balíčky NuGet.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Šablony projektů nyní zahrnují ADO.NET Entity Framework 4.1 jako balíčku NuGet, předem nainstalovaná

Zahrnuje ADO.NET Entity Framework 4.1 Code First funkce. Kód je nejdřív nový vývoj vzor ADO.NET Entity Framework, který představuje alternativu ke stávající vzory první databáze a Model First.

Kód nejprve se zaměřuje kolem definování model pomocí třídy objektů POCO ("prostý staré CLR objektů") napsané v jazyce Visual Basic nebo C#. Tyto třídy lze mapovat k existující databázi nebo použít ke generování schématu databáze. Další konfigurace můžete zadat pomocí *DataAnnotations* atributy nebo pomocí rozhraní fluent API.

Dokumentace pro použití kódu Firstwith ASP.NET MVC je k dispozici na webu ASP.NET na následující adresy URL:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Šablony projektů zahrnují knihovny JavaScript jako předem nainstalované balíčky NuGet

Když vytvoříte nový projekt ASP.NET MVC 3, tento projekt zahrnuje soubory JavaScript již bylo zmíněno dříve (například knihovna Modernizr) pomocí jejich instalace pomocí nástroje NuGet přímo nepřidávat skriptů do složky skriptů v šabloně projektů obsah. To umožňuje použití balíčku NuGet k skripty aktualizovat na nejnovější verzi, když jsou vydávány nové verze skriptů.

Například zadána četnost nové verze jQuery verzi jQuery obsažené v šabloně projektů v určitém okamžiku bude zastaralá. Ale protože jQuery je uveden jako nainstalovaným balíčkem NuGet, vám bude upozorněn v dialogovém okně NuGet jsou k dispozici novější verze jQuery.

JQuery zahrnuje číslo verze v názvu souboru, a proto jQuery aktualizaci na nejnovější verzi také vyžaduje aktualizaci `<script>` značky, který odkazuje na soubor jQuery používat nový název souboru. Další zahrnuté skriptu knihovny neobsahují číslo verze v názvu skriptu, takže může být snadněji aktualizovat na jejich nejnovější verze.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Známé problémy

- V některých případech mohou selhat instalace chyba zpráva "instalace se nezdařila s kódem chyby (0x80070643)". Informace o tom, jak tento problém obejít, najdete v článku [Knowledge Base, článek 2531566](https://support.microsoft.com/kb/2531566).
- Generování uživatelského rozhraní pro přidávání řadiče není vygenerovat entit, které využít výhod podpory dědičnosti entity v rámci Entity Framework. Například uděleno na základní *osoba* třídu, která zdědí *Student* třídy generování uživatelského rozhraní *Student* třída bude mít za následek generovaného kódu, který nelze kompilovat.
- Vytvoření nového projektu ASP.NET MVC 3 ve složce řešení příčiny *NullReferenceException* chyby. Alternativní řešení je vytvoření projektu ASP.NET MVC 3 v kořenové složky řešení a pak přesuňte jej do složky řešení.
- IntelliSense pro syntaxi Razor nefunguje, pokud je nainstalován ReSharper. Pokud máte nainstalovaný ReSharper a chcete využít výhod podpory technologie IntelliSense pro Razor v architektuře ASP.NET MVC 3, naleznete v příspěvku [Razor Intellisense a ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri níž se probírá, jak je používat společně ještě dnes.
- Během instalace se zobrazí dialogové okno přijetí smlouvy EULA licenční podmínky v okně, které je menší než určená.
- Při úpravách zobrazení syntaxe Razor (cshtml nebo.* vbhtml* soubor), zobrazení. ASP.NET MVC 3 nezahrnuje všechny fragmenty pro zobrazení syntaxe Razor... aspxselecting fragmentu kódu pro architekturu ASP.NET MVC zobrazí fragmenty
- Pokud nainstalujete ASP.NET MVC 3 pro aplikaci Visual Web Developer Express na počítači, kde není nainstalovaná sada Visual Studio a pak instalaci sady Visual Studio, je třeba přeinstalovat ASP.NET MVC 3. Visual Studio a Visual Web Developer Express sdílet součásti, které jsou aktualizovány pomocí ASP.NET MVC 3 Instalační služby. Stejný problém platí při instalaci ASP.NET MVC 3 pro sadu Visual Studio v počítači, který nepodporuje mít Visual Web Developer Express a pak později nainstalujete Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Změny ve verzi RTM technologie ASP.NET MVC 3

Tato část popisuje změny a opravy chyb provedené ve verzi ASP.NET MVC 3 RTM od vydání RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Změna: Aktualizovat verzi jQuery uživatelského rozhraní na 1.8.7

Zahrnout nejnovější verzi knihovny jQuery uživatelského rozhraní byly aktualizovány šablon projektu ASP.NET MVC pro Visual Studio. Tyto šablony také obsahovat minimální sadu zdrojové soubory vyžadované jQuery uživatelského rozhraní, například související šablon stylů CSS a soubory obrázků.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Změny: Změnit výchozí ModelMetadataProvider zpět do DataAnnotationsModelMetadataProvider

RC2 verzi ASP.NET MVC 3 zavedená *CachedDataAnnotationsMetadataProvider* třídy, které poskytuje ukládání do mezipaměti na základě existující *DataAnnotationsModelMetadataProvider* třídy zlepšení výkonu. Však byly některé chyby nahlášené s touto implementací tak, aby tato změna byla vrátit zpět a přesunout do projektu MVC Futures, která je k dispozici v [skvělá webová sada ASP.NET](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Opravené: Vkládání část výrazu Razor, který obsahuje výsledky prázdných znaků v ní probíhá vrácení zpět

Předprodejní verze ASP.NET MVC 3 Když vložíte součástí výrazu Razor, který obsahuje prázdné znaky v souboru nástroje Razor, výsledný výraz, je obrácený. Zvažte například následující blok kódu Razor:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Pokud vyberete text "první param" v metodě první a vložte jej jako argument do druhé metody, výsledek je následující:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Správné chování je, že operace vložení by měl mít za následek následující:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Tento problém byl opraven ve verzi RTM tak, aby během operace vložení je správně zachovají výraz.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Opravené: Přejmenování souboru Razor, který je otevřen v editoru zakáže zabarvení syntaxe a IntelliSense

Přejmenování souboru nástroje Razor pomocí Průzkumníku řešení při otevření souboru v okně editoru způsobí, že zvýraznění syntaxe a IntelliSense přestane fungovat pro tento soubor. To bylo opraveno tak, aby zvýraznění a IntelliSense udržované po přejmenovat.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Známé problémy

- Pokud zavřete Visual Studio 2010 SP1 Beta při otevřené konzole Správce balíčků NuGet, Visual Studio dojde k chybě a pokusí o restartování. Tento problém bude vyřešený ve verzi RTM sady Visual Studio 2010 SP1.
- Instalační program ASP.NET MVC 3 je pouze moct instalovat původní verzi správce balíčků NuGet. Po instalaci počáteční verze NuGet je možné nainstalovat a aktualizovat pomocí Správce rozšíření Visual Studio. Pokud už máte nainstalovaný NuGet, přejděte do Galerie rozšíření Visual Studio k aktualizaci na nejnovější verzi balíčku nuget.
- Vytvoření nového projektu ASP.NET MVC 3 ve složce řešení příčiny *NullReferenceException* chyby. Alternativní řešení je vytvoření projektu ASP.NET MVC 3 v kořenové složky řešení a pak přesuňte jej do složky řešení.
- Instalační program může trvat déle, než v předchozích verzích ASP.NET MVC pro dokončení. Je to proto, že se aktualizuje součásti sady Visual Studio 2010.
- IntelliSense pro syntaxi Razor nefunguje, pokud je nainstalován ReSharper. Pokud máte nainstalovaný ReSharper a chcete využít výhod podpory technologie IntelliSense pro Razor v architektuře ASP.NET MVC 3, naleznete v příspěvku [Razor Intellisense a ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri níž se probírá, jak je používat společně ještě dnes.
- Zobrazení CCSHTML a VBHTML vytvořen Beta verzi ASP.NET MVC 3 nemají jejich proces sestavení správně nastavené, s výsledkem, že tyto zobrazit typy byly vynechány při publikování projektu. Hodnota akce sestavení pro tyto soubory by měla být nastavena na "Obsah". ASP.NET MVC 3 RTM řeší tento problém pro nové soubory, ale nemá správné nastavení pro existující soubory pro projekt vytvořené pomocí předběžných verzí.
- ![](mvc3-release-notes/_static/image3.png)
- Během instalace se zobrazí dialogové okno přijetí smlouvy EULA licenční podmínky v okně, které je menší než určená.
- Při úpravách zobrazení syntaxe Razor (cshtml soubor), přejděte do řadiče položky nabídky v sadě Visual Studio nebude k dispozici, a neexistují žádné fragmenty kódu.
- Pokud nainstalujete ASP.NET MVC 3 pro aplikaci Visual Web Developer Express na počítači, kde není nainstalovaná sada Visual Studio a pak instalaci sady Visual Studio, je třeba přeinstalovat ASP.NET MVC 3. Visual Studio a Visual Web Developer Express sdílet součásti, které jsou aktualizovány pomocí ASP.NET MVC 3 Instalační služby. Stejný problém platí při instalaci ASP.NET MVC 3 pro sadu Visual Studio v počítači, který nepodporuje mít Visual Web Developer Express a pak později nainstalujete Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Nejnovější změny

- V předchozích verzích rozhraní ASP.NET MVC akci, kterou jsou filtry vytvořit jednotlivé žádosti s výjimkou v několika případech. Toto chování bylo nikdy zaručenou chování, ale jenom podrobností implementace a smlouvu pro filtry vzít v úvahu je bezstavové. V architektuře ASP.NET MVC 3 filtry jsou uložené v mezipaměti důkladnějšímu. Proto všechny filtry vlastní akce, které nesprávně ukládají stav instance může být poškozený.
- Došlo ke změně pořadí zpracování pro filtry výjimek pro filtry výjimek, které mají stejnou *pořadí* hodnotu. ASP.NET MVC 2 a starší, filtry výjimky na řadiči, které mají stejný *pořadí* hodnoty jako na metodu akce jsou spuštěny před filtry výjimek na metodu akce. To by obvykle být případ, kdy se mají použít filtry výjimek bez zadané *pořadí* hodnotu. V architektuře ASP.NET MVC 3 Tento pořadí změněno tak, aby se nejprve provede nejvíce konkrétní obslužná rutina výjimky. Jako v předchozích verzích Pokud *pořadí* explicitně zadána vlastnost, filtry jsou spuštěny v uvedeném pořadí.
- Novou vlastnost s názvem *FileExtensions* byl přidán do *VirtualPathProviderViewEngine* základní třídy. Pokud technologie ASP.NET vyhledá zobrazení cestou (ne podle názvu), jsou považovány za jenom zobrazení s příponou souboru obsažené v této nové vlastnosti seznamu. Toto je narušující změně v aplikacích, kde vlastní sestavovací zprostředkovatel registroval Chcete-li povolit vlastní příponu souboru pro zobrazení webové formuláře a kde zprostředkovatele odkazuje na tato zobrazeními pomocí úplnou cestu, nikoli název. Řešením je změnit hodnotu *FileExtensions* vlastnost, aby zahrnovala vlastního souboru rozšíření.
- Implementace objektu pro vytváření vlastní zařízení, které přímo implementovat *IControllerFactory* rozhraní musí poskytnout implementaci nového *GetControllerSessionBehavior* metoda, která byla Přidat do rozhraní v této verzi. Obecně je doporučeno, není toto rozhraní implementovat přímo a místo toho jsou odvozeny třídě z *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Změny v architektuře ASP.NET MVC 3 RC2

Tato část popisuje změny (nové funkce a opravy chyb) ve verzi ASP.NET MVC 3 RC2 od vydání verze RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Projekt šablony změnit, například jQuery 1.4.4, jQuery 1.7 ověření a jQuery UI 1.8.6

Šablony projektů pro ASP.NET MVC 3 teď obsahuje nejnovější verze jQuery, jQuery ověření a jQuery uživatelského rozhraní. jQuery uživatelského rozhraní je nové přidání do šablony projektů a poskytuje užitečný uživatelské rozhraní pomůcek. Další informace o jQuery uživatelského rozhraní, navštivte jejich domovské stránky: [ http://jqueryui.com/ ](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Třídy přidané "AdditionalMetadataAttribute"

Můžete použít *AdditionalMetadataAttribute* třída k naplnění *ModelMetadata.AdditionalValues* slovník pro vlastnosti modelu.

Předpokládejme například, že model zobrazení obsahuje vlastnosti, které mají být zobrazeny pouze pro správce. Tohoto modelu může být označený poznámkou s nový atribut pomocí AdminOnly jako klíč a hodnotu true jako hodnotu, jako v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Tato metadata je k dispozici žádné šablony zobrazení nebo editoru při vykreslení zobrazení modelu produktu. Je na vás jako vývojář aplikace interpretovat informace metadat.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Vylepšené zobrazení generování uživatelského rozhraní

Vytvoření šablon T4 použita pro generování uživatelského rozhraní zobrazení nyní volání do metody helper šablony například *EditorFor* místo pomocné rutiny, jako *TextBoxFor*. Tato změna zlepšuje podporu pro metadata na modelu ve formě atributy datového poznámky při dialogové okno Přidat zobrazení generuje zobrazení.

Generování uživatelského rozhraní přidat zobrazení také zahrnuje vylepšenou detekční a používání primární klíče informací na modelu na základě. Například dialogové okno Přidat zobrazení používá tyto informace k zajištění, že hodnotu primárního klíče není vygeneroval jako pole upravitelné formuláře.

Výchozí upravit a vytvořit šablony nezahrnují odkazy na jQuery skripty potřebné pro ověření klienta.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Přidání Html.Raw – metoda

Ve výchozím nastavení zobrazit syntaxi Razor modul umístí kódování HTML všechny hodnoty. Například následující fragment kódu kóduje HTML uvnitř proměnnou pozdravu tak, aby se zobrazí na stránce jako `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

Nové *Html.Raw* metoda poskytuje jednoduchý způsob zobrazení nezakódovaný jazyk HTML, když obsah se označuje jako bezpečné. Tento příklad zobrazuje do jednoho řetězce, ale řetězec je vykresleno jako značka:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Vlastnost přejmenovat "Controller.ViewModel" a vlastnost "Zobrazit" na "ViewBag"

Dříve *ViewModel* vlastnost *řadič* korespondenční k *zobrazení* vlastnost zobrazení. Obě tyto vlastnosti umožňují přístup hodnoty *ViewDataDictionary* pomocí syntaxe dynamické přistupující objekt vlastnosti. Aby byla stejná, aby nedocházelo k záměně a aby byla konzistentní byla přejmenovaná obě vlastnosti.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Třída přejmenovat "ControllerSessionStateAttribute" na "SessionStateAttribute"

*ControllerSessionStateAttribute* třída byla zavedena v RC aplikace ASP.NET MVC 3. Vlastnost byla přejmenována na být více stručného.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Vlastnost přejmenovat RemoteAttribute "Pole" na "AdditionalFields"

*RemoteAttribute* třídy *pole* vlastnost způsobila úplně jasné mezi uživateli. Přejmenování této vlastnosti *AdditionalFields* vysvětluje jeho záměr.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Přejmenovat "SkipRequestValidationAttribute" na "AllowHtmlAttribute"

*SkipRequestValidationAttribute* atribut byla přejmenována na *AllowHtmlAttribute* lépe představují jeho zamýšlené použití.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Změněné "Html.ValidationMessage" metodu pro zobrazení první užitečné chybovou zprávu

*Html.ValidationMessage* zobrazíte první užitečné chybovou zprávu místo jednoduše první chyba byla opravena metoda.

Během vazby modelu *ModelState* slovník je možné importovat z více zdrojů se chybové zprávy o vlastnosti, včetně přímo pro model (Pokud se implementuje *IValidatableObject* ), od ověření atributy použité pro vlastnost a od výjimky vydané při přístupu k vlastnosti.

Když *Html.ValidationMessage* metoda zobrazí ověřovací zprávu, přeskočí stav modelu položky, které patří k výjimce, protože tyto obecně nejsou určeny pro koncového uživatele. Místo toho metodu vyhledá první zprávu ověření, který není spojen s výjimkou a zobrazí se zpráva. Pokud je nalezena žádná taková zpráva, bude výchozí obecnou chybovou zprávu, která souvisí s první výjimka.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Opravené @model deklarace není prázdné přidat do dokumentu

V dřívějších verzích <em> @model </em> deklarace v horní části zobrazení přidat prázdný řádek do vykreslené výstupu protokolu HTML. Tato chyba byla opravena tak, aby deklaraci nezavádí prázdný znak.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Vlastnost přidané "FileExtensions" pro moduly zobrazení pro podporu názvy souborů specifické pro modul

Modul zobrazení můžete vrátit zobrazení pomocí cesty explicitní zobrazení jako v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

První modul zobrazení se vždy pokusí k vykreslení zobrazení. Ve výchozím nastavení bude zobrazovací modul webových formulářů je první zobrazovací modul; vzhledem k tomu, že modul webových formulářů nelze vykreslit zobrazení syntaxe Razor, dojde k chybě. Teď máte moduly zobrazení *FileExtensions* vlastnost, která slouží k určení, které přípony souborů, které podporují. Tato vlastnost je zaškrtnuta možnost při ASP.NET určuje, zda modul zobrazení může vykreslit soubor. Toto je narušující změně a další podrobnosti jsou součástí [nejnovějších změn](#_Toc2_BC) část tohoto dokumentu.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Opravené "LabelFor" Pomocníka pro vydávání správnou hodnotu pro atribut "Pro"

Chyby byla opravena where *LabelFor* metoda vykresluje *pro* atribut, který odpovídá *vstupní* elementu *název* atribut místo jeho ID. Podle W3C *pro* atribut by měl odpovídat *vstupní* ID elementu.

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Metoda dlouhodobého "RenderAction" má přednost explicitní hodnoty během vazby modelu

V dřívějších verzích, explicitní hodnoty, které byly předány *RenderAction* metoda byly ignorován považuje aktuálních hodnot formuláře během vazby modelu uvnitř podřízená akce. Oprava zajistí, že explicitní hodnoty mají přednost před během vazby modelu.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Nejnovější změny

- V předchozích verzích rozhraní ASP.NET MVC byly filtry akce vytvořeny každý požadavek s výjimkou v několika případech. Toto chování bylo nikdy zaručenou chování, ale jenom podrobností implementace a smlouvu pro filtry vzít v úvahu je bezstavové. V architektuře ASP.NET MVC 3 filtry jsou uložené v mezipaměti důkladnějšímu. Proto všechny filtry vlastní akce, které nesprávně ukládají stav instance může být poškozený.
- Došlo ke změně pořadí zpracování pro filtry výjimek pro filtry výjimek, které mají stejnou *pořadí* hodnotu. ASP.NET MVC 2 a starší, filtry výjimky na řadiči, který měl stejné *pořadí* hodnoty jako na metodu akce byly spuštěny před filtry výjimek na metodu akce. To může obvykle být tento případ, kdy byly použity filtry výjimek bez zadané *pořadí* hodnotu. V architektuře ASP.NET MVC 3 Tento pořadí změněno tak, aby se nejprve provede nejvíce konkrétní obslužná rutina výjimky. Jako v předchozích verzích Pokud *pořadí* explicitně zadána vlastnost, filtry jsou spuštěny v uvedeném pořadí.
- Novou vlastnost s názvem *FileExtensions* byl přidán do *VirtualPathProviderViewEngine* základní třídy. Pokud technologie ASP.NET vyhledá zobrazení cestou (ne podle názvu), jsou považovány za jenom zobrazení s příponou souboru obsažené v této nové vlastnosti seznamu. Toto je narušující změně v aplikacích, kde vlastní sestavovací zprostředkovatel registroval Chcete-li povolit vlastní příponu souboru pro zobrazení webové formuláře a kde zprostředkovatele odkazuje na tato zobrazeními pomocí úplnou cestu, nikoli název. Řešením je změnit hodnotu *FileExtensions* vlastnost, aby zahrnovala vlastního souboru rozšíření.
- Implementace objektu pro vytváření vlastní zařízení, které přímo implementovat <em>IControllerFactory</em> rozhraní musí poskytnout implementaci nového <em>GetControllerSessionBehavior</em> <em> Metoda, která byla přidána do rozhraní v této verzi</em>. Obecně je doporučeno, není toto rozhraní implementovat přímo a místo toho jsou odvozeny třídě z <em>DefaultControllerFactory</em>.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Známé problémy

- Instalační program ASP.NET MVC 3 je pouze moct instalovat původní verzi správce balíčků NuGet. Po instalaci počáteční verze NuGet je možné nainstalovat a aktualizovat pomocí Správce rozšíření Visual Studio. Pokud už máte nainstalovaný NuGet, přejděte do Galerie rozšíření Visual Studio k aktualizaci na nejnovější verzi balíčku nuget.
- Vytvoření nového projektu ASP.NET MVC 3 ve složce řešení příčiny *NullReferenceException* chyby. Alternativní řešení je vytvoření projektu ASP.NET MVC 3 v kořenové složky řešení a pak přesuňte jej do složky řešení.
- Instalační program může trvat déle, než v předchozích verzích ASP.NET MVC pro dokončení. Je to proto, že se aktualizuje součásti sady Visual Studio 2010.
- IntelliSense pro syntaxi Razor nefunguje, pokud je nainstalován ReSharper. Pokud máte nainstalovaný ReSharper a chcete využít výhod podpory technologie IntelliSense pro Razor v ASP.NET MVC 3 RC2, naleznete v příspěvku [Razor Intellisense a ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri níž se probírá, jak je používat společně ještě dnes.
- Zobrazení CSHTML a VBHTML vytvořen Beta verzi ASP.NET MVC 3 nemají jejich proces sestavení správně nastavené, s výsledkem, že tyto zobrazit typy byly vynechány při publikování projektu. *Akce sestavení* hodnoty pro tyto soubory musí být nastavené na obsah ". ASP.NET MVC 3 RC2 řeší tento problém pro nové soubory, ale nemá správné nastavení pro existující soubory pro projekt vytvořen s Beta verzí.![](mvc3-release-notes/_static/image4.png)
- Během instalace se zobrazí dialogové okno přijetí smlouvy EULA licenční podmínky v okně, které je menší než určená.
- Při úpravách zobrazení syntaxe Razor (cshtml soubor), přejděte do řadiče položky nabídky v sadě Visual Studio nebude k dispozici, a neexistují žádné fragmenty kódu.
- Pokud nainstalujete ASP.NET MVC 3 pro aplikaci Visual Web Developer Express na počítači, kde není nainstalovaná sada Visual Studio a pak instalaci sady Visual Studio, je třeba přeinstalovat ASP.NET MVC 3. Visual Studio a Visual Web Developer Express sdílet součásti, které jsou aktualizovány pomocí ASP.NET MVC 3 Instalační služby. Stejný problém platí při instalaci ASP.NET MVC 3 pro sadu Visual Studio v počítači, který nepodporuje mít Visual Web Developer Express a pak později nainstalujete Visual Web Developer Express.
- Instalace technologie ASP.NET MVC 3 RC 2 neaktualizuje NuGet, pokud je již nainstalováno. Chcete upgradovat NuGet, přejděte do Správce rozšíření sady Visual Studio a měli zobrazují jako dostupné aktualizace. Můžete upgradovat NuGet na nejnovější verzi z ní.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 Release Candidate

ASP.NET MVC Release Candidate byla vydána 9 listopadu 2010.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Nové funkce v rozhraní ASP.NET MVC 3 RC

Tato část popisuje funkce, které byly zavedeny v od vydání Beta verzi ASP.NET MVC 3 RC.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>Správce balíčků NuGet

ASP.NET MVC 3 zahrnuje Správce balíčků NuGet (dříve označované jako NuPack), což je nástroj pro správu na integrované balíčku pro přidání do projektů sady Visual Studio knihovny a nástroje. Tento nástroj automatizuje kroky, které vývojáři provést ještě dnes k získání knihovny do stromu jejich zdroje.

Můžete pracovat s NuGet jako nástroj příkazového řádku, jako okno integrovaného konzoly v sadě Visual Studio 2010 v místní nabídce sady Visual Studio a jako sada rutin prostředí PowerShell.

Další informace o systému NuGet najdete [Nuget dokumentaci](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Vylepšené "Nový projekt" dialogové okno

Když vytvoříte nový projekt, dialogové okno Nový projekt teď umožňuje určit zobrazovací modul, jakož i typ projektu ASP.NET MVC.

![](mvc3-release-notes/_static/image5.png)

Podpora pro úpravy seznamu šablon a moduly uvedené v dialogovém okně zobrazení je součástí této verze.

Výchozí šablony jsou následující:

Prázdný. Obsahuje minimální sadu souborů pro projekt ASP.NET MVC, včetně výchozí strukturu adresáře pro projekty ASP.NET MVC Site.css soubor, který obsahuje výchozí styly ASP.NET MVC a skripty adresář, který obsahuje výchozí soubory JavaScript.

Internet Application. Obsahuje ukázkové funkce, které ukazuje, jak používat zprostředkovatel členství s architekturou ASP.NET MVC.

Seznam šablon projektu, který se zobrazí v dialogovém okně je uvedený v registru systému Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Nerelační řadiče

Nové *ControllerSessionStateAttribute* vám dává větší kontrolu nad chování stavu relace pro řadiče zadáním [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) hodnota výčtu.

Následující příklad ukazuje, jak chcete-li vypnout stavu relace pro všechny požadavky na řadič.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

Následující příklad ukazuje, jak nastavit stav relace jen pro čtení pro všechny požadavky na řadiči.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Nové atributy ověření

#### <a name="compareattribute"></a>CompareAttribute

Nové *CompareAttribute* ověřovací atribut vám umožňuje porovnat hodnoty dvě různé vlastnosti modelu. V následujícím příkladu *ComparePassword* vlastnost musí odpovídat *heslo* pole, aby byla platná.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

Nové *RemoteAttribute* ověřovací atribut využívá výhod vzdáleného validátoru jQuery ověření plug-in pro, který umožňuje ověření na straně klienta volat metodu na serveru, který provede logiku samotné ověření.

V následujícím příkladu *uživatelské jméno* vlastnost má *RemoteAttribute* použít. Při úpravě tuto vlastnost v zobrazení, ověření klienta zavolá akci s názvem *UserNameAvailable* na *UsersController* třída k ověřování v tomto poli.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

Následující příklad ukazuje odpovídající kontroler.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Ve výchozím nastavení je název vlastnosti, který se použije atribut pro odesílání na metodu akce, jako parametr řetězce dotazu.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Nové přetížení metod "LabelForModel" a "LabelFor"

Byly přidány nové přetížení pro *LabelFor* a *LabelForModel* metody, které vám umožní zadat text popisku. Následující příklad ukazuje, jak používat tato přetížení.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Podřízená akce ukládání výstupu do mezipaměti

*OutputCacheAttribute* podporuje ukládání výstupu do mezipaměti podřízených akcí, které se volají pomocí *Html.RenderAction* nebo *Html.Action* pomocné metody. Následující příklad ukazuje zobrazení, která volá jinou akci.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

*GetDate* akce je opatřen poznámkou *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Když tento kód běží, je výsledkem volání Html.Action("GetDate") uložená v mezipaměti pro 100 sekund.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>"Přidat zobrazení" Vylepšená dialogová okna

Když přidáte zobrazení se silnými typy, dialogové okno Přidat zobrazení nyní filtruje více nepoužitelné typy než v předchozích verzích, jako je například mnoho typů rozhraní .NET Framework core. Navíc tento seznam je řazen nyní, pomocí názvu třídy a ne plně kvalifikovaný název typu, takže je snazší najít typy. Název typu se nyní zobrazí jako v následujícím příkladu:

Název třídy (obor názvů)

V dřívějších verzích to by byla zobrazena jako následující:

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Ověření granulární žádosti

*Vyloučit* vlastnost *atribut ValidateInputAttribute* již existuje. Místo toho pokud chcete, aby ověření žádosti pro specifické vlastnosti modelu přeskočeno během vazby modelu, použijte nové *SkipRequestValidationAttribute*.

Předpokládejme například, že metoda akce slouží k úpravám příspěvku na blogu:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

Následující příklad ukazuje zobrazení modelu pro příspěvku na blogu.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Když uživatel odešle některé kód pro vlastnost Popis, vazby modelu se nezdaří z důvodu ověření žádosti. Zakázat ověření žádosti během vazby modelu pro v příspěvku blogu popis, použít *SkipRequpestValidationAttribute* k vlastnosti, jak ukazuje tento příklad:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Případně, chcete-li vypnout ověření žádosti pro každou vlastnost modelu, použít *atribut ValidateInputAttribute* s hodnotou *false* na metodu akce:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Nejnovější změny

- Došlo ke změně pořadí zpracování pro filtry výjimek pro filtry výjimek, které mají stejnou *pořadí* hodnotu. ASP.NET MVC 2 a starší, filtry výjimky na řadiči, který měl stejné *pořadí* jako na metodu akce byly spuštěny před filtry výjimek na metodu akce. To může obvykle být tento případ, kdy byly použity filtry výjimek bez zadané *pořadí* hodnotu. V architektuře ASP.NET MVC 3 Tento pořadí změněno tak, aby se nejprve provede nejvíce konkrétní obslužná rutina výjimky. Jako v předchozích verzích Pokud *pořadí* explicitně zadána vlastnost, filtry jsou spuštěny v uvedeném pořadí.
- Přidat novou vlastnost s názvem *FileExtensions* k *VirtualPathProviderViewEngine* základní třídy. Při vyhledávání zobrazení cesty (a ne název), pouze zobrazení s příponou souboru, který je součástí považuje za této nové vlastnosti seznamu. Toto je narušující změně pro ty, kteří zaregistrujte poskytovatele vlastní sestavení povolit rozšíření vlastního souboru pro zobrazení webové formuláře a odkazují tyto zobrazení pomocí úplnou cestu, nikoli název. Řešením je změnit hodnotu *FileExtensions* vlastnost, aby zahrnovala vlastního souboru rozšíření.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Známé problémy

- Instalační program může trvat déle, než v předchozích verzích rozhraní ASP.NET MVC dokončit, protože ho aktualizuje součásti sady Visual Studio 2010.
- Přidat zobrazení generování při výběru astrongly zadali vlastnosti jen pro zápis scaffold zobrazení. Třeba tyto vždy ignorovat pomocí generování uživatelského rozhraní. Dialogové okno Přidat zobrazení také scaffold vlastnosti jen pro čtení při generování zobrazení "Upravit" nebo "Vytvořit". Vlastnosti jen pro čtení by vygeneroval jenom pro zobrazení a seznamu zobrazení.
- Ladění nefunguje, pokud technologie ASP.NET MVC 3 je nainstalována spolu s modifikátorem Async CTP. ASP.NET MVC 3 nemůže být nainstalované-souběžného s modifikátorem Async CTP. Odinstalujte CTP asynchronní k opravě ladění. Další informace najdete v [tomto příspěvku na blogu](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) o odinstalovat všechny části ASP.NET MVC 3 RC.
- Intellisense pro Razor nefunguje, pokud je nainstalován Resharper. Pokud máte nainstalovaný ReSharper a chcete využít výhod podporu technologie intellisense Razor v ASP.NET MVC 3 RC, přečtěte si prosím [tomto příspěvku na blogu](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) z JetBrains, který popisuje, jak je používat společně ještě dnes.
- CSHTML a VBHTML zobrazení vytvořená s beta verzi ASP.NET MVC 3 nemají jejich akce sestavení správně které vynechá je z publikování. *Akce sestavení* pro tyto soubory musí být nastavené na "Obsah". ASP.NET MVC 3 RC řeší tento problém pro nové soubory, ale nemá správné nastavení pro existující soubory pro projekt vytvořen s Beta.
- Instalační program může trvat déle, než v předchozích verzích rozhraní ASP.NET MVC dokončit, protože ho aktualizuje součásti sady Visual Studio 2010.
- Generování přidat zobrazení, když vyberete "Upravit" silného typu scaffold zobrazení číst pouze vlastnosti. Vlastnosti jen pro zápis, se vygeneroval pro zobrazení "Zobrazit".
- Během instalace se zobrazí dialogové okno přijetí smlouvy EULA licenční podmínky v okně, které je menší než určená.
- Instalace Visual Studio asynchronní CTP způsobuje konflikt s vydáním Razor, který je součástí ASP.NET MVC 3 tooling instalace. Ujistěte se, že není pokusí nainstalovat Visual Studio asynchronní CTP a verze Razor na stejném počítači.
- Při úpravách zobrazení syntaxe Razor (cshtml soubor), přejděte do řadiče položky nabídky v sadě Visual Studio nebude k dispozici, a neexistují žádné fragmenty kódu.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 Beta

ASP.NET MVC 3 Beta byla vydána 6 říjen 2010. Následující poznámky jsou specifické pro betaverzi a podléhají všechny aktualizace nebo změny odkazuje v oddíle ASP.NET MVC 3 Release Candidate výše.

## <a id="0.1__Toc274034215"></a>  Nové Beta 2003V rozhraní ASP.NET MVC 3

<a id="0.1__Default_validation_system"></a>Tato část popisuje funkce, které byly zavedeny v betaverzi ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>  Správce balíčků NuGet

ASP.NET MVC 3 zahrnuje Správce balíčků NuGet, který je nástroj pro správu na integrované balíčku pro přidání knihovny a nástroje pro projektů sady Visual Studio. Ve většině případů automatizuje kroky, které vývojáři provést ještě dnes k získání knihovny do stromu jejich zdroje.

Můžete pracovat s NuGet jako nástroj příkazového řádku, jako okno integrovaného konzoly v sadě Visual Studio 2010 v místní nabídce sady Visual Studio a jako sadu rutin prostředí PowerShell.

Další informace o systému NuGet najdete [NuGet dokumentaci](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>  Vylepšené dialogové okno Nový projekt

Když vytvoříte nový projekt, dialogové okno Nový projekt teď umožňuje určit zobrazovací modul, jakož i typ projektu ASP.NET MVC.

![](mvc3-release-notes/_static/image6.png)

Podpora pro úpravy seznamu šablon a zobrazení moduly uvedené v dialogovém okně není součástí této verze.

Výchozí šablony jsou následující:

Prázdný. Obsahuje minimální sadu souborů pro projekt ASP.NET MVC, včetně výchozí strukturu adresáře pro projekty ASP.NET MVC, malé Site.css soubor, který obsahuje výchozí styly ASP.NET MVC a skripty adresář, který obsahuje výchozí soubory JavaScript.

Internet Application. Obsahuje ukázkové funkce, které ukazuje, jak používat zprostředkovatel členství v rámci rozhraní ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>  Jednodušší způsob, jak určit silně typované modely v zobrazení syntaxe Razor

Pomocí nové je jednodušší způsob, jak můžete zadat typ modelu pro zobrazení se silnými typy Razor @model direktivy pro zobrazení CSHTML a @ModelType direktivy pro VBHTML zobrazení. V dřívějších verzích rozhraní ASP.NET MVC zadali byste, že zobrazení silného typu modelu pro syntaxi Razor tímto způsobem:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

V této verzi můžete použít následující syntaxi:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  Podporu pro nové webové stránky ASP.NET pomocné metody

Nové technologie ASP.NET Web Pages zahrnuje sadu pomocné metody, které jsou užitečné pro přidání běžně používané funkce do řadiče a zobrazení. ASP.NET MVC 3 podporuje použití tyto pomocné metody v rámci kontrolery a zobrazení (podle potřeby). Tyto metody jsou obsažené v System.Web.Helpers sestavení. Následující tabulka uvádí několik pomocné metody rozhraní ASP.NET Web Pages.

| **Pomocné rutiny** | **Popis** |
| --- | --- |
| Graf | Vykreslí grafu v rámci zobrazení. Obsahuje metody, jako je například Chart.ToWebImage, Chart.Save a Chart.Write. |
| Crypto | Používá k vytvoření správně algoritmy hash řetězce Salt a použita hodnota hash hesla. |
| WebGrid | Vykreslí na kolekci objektů (většinou data z databáze) jako mřížka. Podporuje stránkování a řazení. |
| Objekt WebImage | Vykreslí bitovou kopii. |
| Webové pošty | Odešle e-mailovou zprávu. |

Stručná referenční příručka téma, které obsahuje pomocné rutiny a základní syntaxe je k dispozici jako součást v dokumentaci k syntaxi ASP.NET Razor na následující adresu URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  Podpora vkládání Další závislosti

Vytváření ve verzi ASP.NET MVC 3 Preview 1, aktuální verze zahrnuje byla přidána podpora pro dvě nové služby a čtyři stávající služby a vylepšenou podporu pro zjištění závislosti a Lokátor společných služeb.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nové IControllerActivator rozhraní pro podrobné řadiče vytváření instancí

Nové rozhraní IControllerActivator poskytuje jemně odstupňovanou kontrolu nad jak instance řadiče pomocí vkládání závislostí. Následující příklad ukazuje rozhraní:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Protějšek to k roli objektu factory kontroleru. Objekt factory kontroleru je implementací rozhraní IControllerFactory, která zajišťuje pro vyhledání typ řadiče i pro vytváření instancí instance tohoto typu kontroleru.

Aktivátory kontrolerů odpovídají pouze za vytváření instancí instance typu kontroleru. Neprovádějte vyhledávání typu kontroleru. Po vyhledání typ správné řadiče, objekty Factory by měly delegovat do instance IControllerActivator pro zpracování skutečné instance kontroleru.

Třída DefaultControllerFactory obsahuje nové konstruktor, který přijímá instanci IControllerFactory. To umožňuje aplikovat vkládání závislostí ke správě tento aspekt vytváření řadiče bez nutnosti přepsat výchozí chování vyhledávání typu kontroleru.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Rozhraní IServiceLocator nahradí IDependencyResolver

Na základě názorů komunity, nahradili betaverze ASP.NET MVC 3 použití rozhraní IServiceLocator s rozhraním IDependencyResolver slimmed rozbalovací specifické pro potřeby rozhraní ASP.NET MVC. Následující příklad ukazuje nové rozhraní:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

V rámci této změny byl nahrazený třídě ServiceLocator také třídě překladače závislostí. Registrace překladač závislostí je podobná dřívějších verzích rozhraní ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Implementace tohoto rozhraní by měly jednoduše delegovat do základního kontejneru pro vkládání závislosti k poskytnutí registrované služby pro požadovaný typ.

Pokud neexistují žádné registrované služby požadovaného typu, ASP.NET MVC očekává, že implementace tohoto rozhraní vrátí hodnotu null z metody GetService a vrátí prázdnou kolekci z metodu GetServices.

Nová třída překladače závislostí umožňuje zaregistrovat třídy, které implementují nové rozhraní IDependencyResolver nebo rozhraní lokátoru společných služeb (IServiceLocator). Další informace o lokátoru společných služeb najdete v tématu [CommonServiceLocator na Githubu](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nové IViewActivator rozhraní pro podrobné zobrazení stránky vytváření instancí

Nové rozhraní IViewPageActivator poskytuje jemně odstupňovanou kontrolu nad jak instance stránky zobrazení pomocí vkládání závislostí. To platí pro instance WebFormView a RazorView instance. Následující příklad ukazuje nové rozhraní:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Tyto třídy teď přijmout IViewPageActivator argument konstruktoru, které lze použít vkládání závislostí řídit, jak jsou vytvářeny instance typy ViewPage, ViewUserControl a WebViewPage.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Nová podpora překladač závislostí stávající služby

Nová verze zahrnuje podporu řešení závislostí pro následující služby:

- Zprostředkovatele ověření modelu. Třídy, které implementují ModelValidatorProvider může být registrováno v překladač závislostí a systém bude používat pro podporu ověřování na straně klienta a serveru.
- Zprostředkovatel metadat modelu. Jednu třídu, která implementuje ModelMetadataProvider lze zaregistrovat v překladač závislostí a systém bude používat k poskytnutí metadat pro systémy ukázka a ověření.
- Zprostředkovatele hodnot. Třídy, které implementují ValueProviderFactory může být registrováno v překladač závislostí a systém bude používat k vytvoření zprostředkovatele hodnot, které se spotřebovávají adaptérem a během vazby modelu.
- Vazače modelů. Třídy, které implementují IModelBinderProvider může být registrováno v překladač závislostí a systém bude používat k vytvoření vazače modelů, které se spotřebovávají systému vazby modelu.

### <a id="0.1__Toc274034221"></a>  Nová podpora Nerušivý na základě jQuery Ajax

ASP.NET MVC zahrnuje Ajax pomocné metody například následující:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Tyto metody vyvolání metody akce na serveru, nikoli pomocí úplný postback pomocí jazyka JavaScript. Tato funkce byla aktualizována využívat výhod jQuery nerušivý způsobem. Místo intrusively emitování vložené skripty klienta, tyto pomocné metody oddělte chování z kód generování atributů jazyka HTML5 pomocí *data-ajax* předponu. Chování se potom použije kód pod položkou příslušné soubory JavaScript. Ujistěte se, že následující soubory JavaScript se odkazuje:

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

Tato funkce je povoleno ve výchozím nastavení v souboru Web.config v nové šablony projektu ASP.NET MVC 3, ale je zakázán ve výchozím nastavení pro existující projekty. Další informace najdete v tématu [přidat příznaky celou aplikaci pro ověření klienta a nerušivý JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) dále v tomto dokumentu.

### <a id="0.1__Toc274034222"></a>  Nová podpora Nerušivý jQuery ověření

Ve výchozím nastavení používá technologie ASP.NET MVC 3 Beta k ověřování jQuery nerušivý způsobem aby bylo možné provést ověření na straně klienta. Pokud chcete povolit ověření nerušivého klienta, volání takto z v rámci zobrazení:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

To vyžaduje, aby ViewContext.UnobtrusiveJavaScriptEnabled vlastnost nastavena na hodnotu true, což lze provést tak, že toto volání:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Také zkontrolujte, zda že odkazují následující soubory JavaScript.

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Tato funkce je zapnuta ve výchozím nastavení v souboru Web.config v nové šablony projektu ASP.NET MVC 3, ale je zakázán ve výchozím nastavení pro existující projekty. Další informace najdete v tématu [nové příznaky celou aplikaci pro ověření klienta a nerušivý JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) dále v tomto dokumentu.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  Nové příznaky celou aplikaci pro ověření klienta a Nerušivý JavaScript

Můžete povolit nebo zakázat ověření klienta a nerušivý JavaScript globálně pomocí statické členy třídy HtmlHelper, jako v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Výchozí šablony projektů povolit nerušivý JavaScript ve výchozím nastavení. Můžete také povolit nebo zakázat tyto funkce v kořenovém souboru Web.config vaší aplikace s následujícím nastavením:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Protože tyto funkce můžete povolit ve výchozím nastavení, nové přetížení zavedených pro třídu HtmlHelper, která umožňují přepíší výchozí nastavení, jak je znázorněno v následujících příkladech:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Z důvodu zpětné kompatibility obou těchto funkcí jsou zakázané ve výchozím nastavení.

### <a id="0.1__Toc274034224"></a>  Nová podpora kód, který běží před spuštěním zobrazení

Nyní můžete umístit soubor s názvem \_viewstart.cshtml (nebo \_viewstart.vbhtml) v zobrazení adresáře a přidejte do ní, který bude sdílena mezi více zobrazení v tomto adresáři a jejích podadresářů kód. Například může vložte následující kód do \_viewstart.cshtml stránku ve složce ~/Views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Toto nastaví ke stránce rozložení pro každé zobrazení ve složce zobrazení a všechny její podsložky rekurzivně. Při zobrazení vykreslované, kód \_viewstart.cshtml soubor spouští před spuštěním kódu zobrazení. \_Viewstart.cshtml kódu se vztahuje na každé zobrazení v této složce.

Ve výchozím nastavení, kód \_viewstart.cshtml souboru platí také pro zobrazení v všechny podsložky. Ale jednotlivé podsložky může mít vlastní verzi \_viewstart.cshtml souboru; v tom případě místní verze přednost. Například pokud chcete spustit kód, který je společná pro všechna zobrazení pro HomeController, put \_viewstart.cshtml soubor ve složce ~/Views/Home.

### <a id="0.1__Toc274034225"></a>  Nová podpora pro syntaxi Razor VBHTML

Předchozí verze preview ASP.NET MVC zahrnuta podpora pro zobrazení pomocí syntaxe Razor podle jazyka C#. Tato zobrazení používat příponu souboru .cshtml. Jako součást probíhající práce na podporu Razor ASP.NET MVC 3 Beta zavádí podporu pro syntaxi Razor v jazyce Visual Basic, který používá přípona souboru .vbhtml.

Úvod do pomocí syntaxe jazyka Visual Basic v VBHTML stránky naleznete v kurzu na následující adrese URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  Podrobnější kontrolu nad atribut ValidateInputAttribute

ASP.NET MVC je vždy k dispozici atribut ValidateInputAttribute třídy, které vyvolá infrastruktury základní ASP.NET žádost o ověření a ujistěte se, že příchozí žádost neobsahuje potenciálně škodlivý vstup. Ve výchozím nastavení je povoleno ověření vstupu. Je možné zakázat žádosti o ověření pomocí atributu atribut ValidateInputAttribute, jako v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Mnoho webových aplikací mít však polí jednotlivé formuláře, které je potřeba povolit HTML, zatímco ostatní pole neměli. Třída atribut ValidateInputAttribute teď umožňuje určit seznam polí, které by neměly být obsažené v žádosti o ověření.

Pokud vyvíjíte modul blog, můžete chtít povolit značek v polích text a souhrn. Tato pole může být zobrazen dva vstupní element s názvem atributu odpovídající název vlastnosti ("Text" a "Souhrn"). Pokud chcete zakázat žádosti o ověření pro tato pole zadejte pouze názvy (oddělené čárkami) ve vlastnosti vyloučení ValidateInput třídy, jako v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  Pomocníci převádějí podtržítka pomlčky pro názvy atributů HTML zadán pomocí anonymní objekty

Pomocné metody umožňují zadat dvojice název/hodnota atributu pomocí anonymní objekt, jako v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Tento přístup není umožňují používat pomlčky v název atributu, protože pomlčka nelze použít pro název vlastnosti v technologii ASP.NET. Ale jsou důležité pro vlastní atributy HTML5; pomlčky například HTML5 používá předpona "data-".

Ve stejnou dobu podtržítka nelze použít pro názvy atributů v HTML, ale jsou platné v rámci názvů vlastností. Proto pokud zadáte atributů s použitím anonymní objekt, a pokud atribut názvy patří podtržítkem,, pomocné metody převede podtržítka pomlčky. Například používá následující syntaxe pomocné rutiny podtržítkem:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

Při spuštění pomocné rutiny, vykreslí předchozí příklad následující kód:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  Opravy chyb

Výchozí objekt šablonu pro šablony Pomocníci EditorFor a DisplayFor teď podporuje řazení zadaná ve vlastnosti DisplayAttribute.Order. (V předchozích verzích nebyl nastavení pořadí použit.)

Ověření klienta nyní podporuje ověřování potlačených vlastností, které mají atributy ověření použít.

Ve výchozím nastavení je nyní zaregistrovaná JsonValueProviderFactory.

## <a id="0.1__Toc274034229"></a>  Nejnovější změny

Došlo ke změně pořadí zpracování pro filtry výjimek pro filtry výjimek, které mají stejnou hodnotu pořadí. ASP.NET MVC 2 a starší filtry výjimky na řadiči se stejným pořadím jako na metodu akce byly spuštěny před filtry výjimek na metodu akce. Obvykle to může být tento případ, kdy byly použity filtry výjimek bez zadané hodnoty pořadí. V architektuře ASP.NET MVC 3 Tento pořadí změněno tak, aby se nejprve provede nejvíce konkrétní obslužná rutina výjimky. Jako v předchozích verzích Pokud vlastnost pořadí je explicitně určena, jsou filtry spustit v zadaném pořadí.

## <a id="0.1__Toc274034230"></a>  Známé problémy

Během instalace se zobrazí dialogové okno přijetí smlouvy EULA licenční podmínky v okně, které je menší než určená.

Zobrazení syntaxe Razor nemají podporu technologie IntelliSense ani zvýraznění syntaxe. Předpokládá se, že podpora pro syntaxi Razor v sadě Visual Studio bude součástí novější verzi.

Při úpravách zobrazení syntaxe Razor (CSHTML soubor), <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> položky nabídky přejít do řadiče v sadě Visual Studio nebude k dispozici, a neexistují žádné fragmenty kódu.

Při použití @model zobrazit syntaxe zadejte silného typu CSHTML, zkratky specifické pro jazyk pro typy nejsou rozpoznány. Například @model int nebude fungovat, ale @model Int32 bude fungovat. Alternativní řešení pro této chyby je pro použití názvu skutečný typ, když zadáte typ modelu.

Při použití @model syntaxe zadejte zobrazení se silnými typy CSHTML (nebo @ModelType k určení zobrazení se silnými typy VBHTML), nejsou podporované typy s možnou hodnotou Null a deklarace pole. Například @model int? není podporován. Místo toho použijte `@model Nullable<Int32>`. Syntaxe @model řetězec [] není podporováno také; místo toho použijte `@model IList<string>`.

Při upgradu projektu ASP.NET MVC 2 na ASP.NET MVC 3, ujistěte se, že jste přidejte oddíl appSettings souboru Web.config následující:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Je známý problém, který způsobuje, že ověřování pomocí formulářů pro vždy přesměrování neověřené uživatele na ~/Account nebo přihlášení, nastavení ověřování formulářů v souboru Web.config je ignorována. Alternativní řešení je přidejte následující nastavení aplikace.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  Právní omezení

© 2011 Microsoft Corporation. Všechna práva vyhrazena. Tento dokument je poskytován "jako-je." Informace a názory vyjádřené v tomto dokumentu včetně adres URL a dalších odkazů na internetové weby, mohou změnit bez předchozího upozornění. Můžete na sebe rizika spojená s jejím používáním.

Tento dokument neposkytuje jste žádná zákonná práva duševního vlastnictví produktů společnosti Microsoft. Můžete kopírovat a tento dokument použít pro interní referenční účely.
