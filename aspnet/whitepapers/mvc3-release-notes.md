---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Dokumentace Microsoftu
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: ''
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: fe3536f3a40f3a74df47bcc1ca0d0b8416895169
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381675"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [Přehled](#overview)
- [Poznámky k instalaci](#installation-notes)
- [Požadavky na software](#software-requirements)
- [Dokumentace](#documentation)
- [Podpora](#support)
- [Upgrade projektu aplikace ASP.NET MVC 2 na ASP.NET MVC 3 nástroje Update](#upgrading)
- [ASP.NET MVC 3 nástroje Update (12. dubna 2011)](#tu-changes)

    - [Dialogové okno "Přidat kontroler" teď můžete vygenerovat řadiče s kódem přístupu k zobrazení a data](#tu-AddControllerDialog)
    - [Vylepšení "ASP.NET MVC 3 nový projekt" dialogové okno](#tu-ImprovementsNewDialogBox)
    - [Šablony projektů nyní obsahují Modernizr 1.7](#tu-Modernizr)
    - [Šablony projektů obsahují aktualizované verze jQuery, uživatelské rozhraní jQuery a jQuery ověření](#tu-UpdatedJQuery)
    - [Šablony projektů nyní obsahují ADO.NET Entity Framework 4.1 jako předinstalovaný balíček NuGet](#tu-EF)
    - [Šablony projektů zahrnout jako předinstalované balíčky NuGet knihoven jazyka JavaScript](#tu-JavaScriptLibsNuget)
    - [Známé problémy](#tu-KI)
- [Verze RTM technologie ASP.NET MVC 3 (13. ledna 2011)](#MVC3RTM)

    - [Změna: Aktualizován verzi uživatelské rozhraní jQuery 1.8.7](#RTM-1)
    - [Změny: Změnit výchozí ModelMetadataProvider zpět do DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Opraveno: Vkládání část výrazu Razor, která obsahuje výsledky prázdné znaky v ní se vrátit zpět](#RTM-3)
    - [Opraveno: Přejmenováním souboru Razor, který je otevřen v editoru zakáže barevné zvýrazňování syntaxe a IntelliSense](#RTM-4)
    - [Známé problémy](#RTM-KI)
    - [Rozbíjející změny v](#RTM-BC)
- [ASP.NET MVC 3 verze Release Candidate 2 (10 December, 2010)](#_Toc2)

    - [Projekt šablony změnit jQuery 1.4.4, jQuery ověření 1.7 a uživatelské rozhraní 1.8.6y 1.8.6 uživatelské rozhraní jQuery](#_Toc2_1)
    - [Třídy přidané "AdditionalMetadataAttribute"](#_Toc2_2)
    - [Vylepšené zobrazení generování uživatelského rozhraní](#_Toc2_3)
    - [Přidání Html.Raw – metoda](#_Toc2_3)
    - [Vlastnosti přejmenováno "Controller.ViewModel" a "Zobrazit" k "Objekt ViewBag"](#_Toc2_4)
    - [Přejmenované "ControllerSessionStateAttribute" třída "SessionStateAttribute"](#_Toc2_5)
    - [Přejmenované RemoteAttribute vlastnost "Pole" na "AdditionalFields"](#_Toc2_6)
    - [Přejmenovat "SkipRequestValidationAttribute" k "AllowHtmlAttribute"](#_Toc2_7)
    - [Metoda změněné "Html.ValidationMessage" k zobrazení první užitečné chybovou zprávu](#_Toc2_8)
    - [Oprava @model deklarace není přidávají prázdné znaky v dokumentu](#_Toc2_9)
    - [Vlastnost přidání "FileExtensions" pro moduly zobrazení pro podporu modulu názvy](#_Toc2_10)
    - [Pomocné rutiny opravené "LabelFor" a vygenerovat správnou hodnotu pro atribut "For"](#_Toc2_11)
    - [Metoda dlouhodobého "RenderAction" dát přednost explicitní hodnoty během vazby modelu](#_Toc2_12)
    - [Rozbíjející změny v](#_Toc2_BC)
    - [Známé problémy](#_Toc2_KI)
- [ASP.NET MVC 3 verze Release Candidate (9. listopadu 2010)](#TOC_ASP_NET_3_RC)

    - [Nové funkce ve verzi RC: technologie ASP.NET MVC 3](#_Toc276711785)
    - [Správce balíčků NuGet](#_Toc276711786)
    - [Vylepšené "Nový projekt" dialogové okno](#_Toc276711787)
    - [Nerelační řadiče](#_Toc276711788)
    - [Nové atributy ověření](#_Toc276711789)
    - [Nová přetížení pro metody "LabelForModel" a "LabelFor"](#_Toc276711790)
    - [Podřízená akce ukládání výstupu do mezipaměti](#_Toc276711791)
    - ["Přidat zobrazení" vylepšení pole dialogového okna](#_Toc276711792)
    - [Ověření detailní žádosti](#_Toc276711793)
    - [Rozbíjející změny v](#_Toc276711794)
    - [Známé problémy](#_Toc276711795)
- [ASP. MVC 3 Beta poznámky (6 říjnu 2010)](#TOC_ASP_NET_3_Beta)

    - [Nové funkce ve verzi Beta ASP.NET MVC 3](#0.1__Toc274034215)
    - [Správce balíčků NuPack](#0.1__Toc274034216)
    - [Vylepšené dialogové okno Nový projekt](#0.1__Toc274034217)
    - [Zjednodušený způsob, jak určit silného typu modelů v zobrazení Razor](#0.1__Toc274034218)
    - [Podpora pro nové rozhraní ASP.NET Web Pages pomocné metody](#0.1__Toc274034219)
    - [Další závislosti vkládání podpory](#0.1__Toc274034220)
    - [Nová podpora Nerušivého jazyka Ajax na základě jQuery](#0.1__Toc274034221)
    - [Nová podpora Nerušivý jQuery ověření](#0.1__Toc274034222)
    - [Nové příznaky celou aplikaci pro ověření klienta a Nerušivý JavaScript](#0.1__Toc274034223)
    - [Nová podpora pro kód, který se spustí před spuštěním zobrazení](#0.1__Toc274034224)
    - [Nová podpora pro syntaxi Razor VBHTML](#0.1__Toc274034225)
    - [Podrobnější kontrolu nad atribut ValidateInputAttribute](#0.1__Toc274034226)
    - [Pomocné rutiny převádějí podtržítka pomlčky pro zadané pomocí anonymních objektů názvy atributu HTML](#0.1__Toc274034227)
    - [Opravy chyb](#0.1__Toc274034228)
    - [Rozbíjející změny v](#0.1__Toc274034229)
    - [Známé problémy](#0.1__Toc274034230)
- [Právní omezení](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Přehled

Tento dokument popisuje verze RTM technologie ASP.NET MVC 3 pro sadu Visual Studio 2010. ASP.NET MVC je architektura pro vývoj webových aplikací, která používá vzor Model-View-Controller (MVC). ASP.NET MVC 3 instalační program obsahuje následující součásti:

- Komponenty modulu runtime ASP.NET MVC 3
- ASP.NET MVC 3 aplikace Visual Studio 2010 tools
- ASP.NET Web Pages běhové komponenty
- Nástroje Visual Studio 2010 webových stránek ASP.NET
- Microsoft Správce balíčků pro .NET (NuGet)
- Aktualizace pro sadu Visual Studio 2010, které přinášejí podporu pro syntaxi Razor. (Podrobnosti najdete v článku znalostní báze 2483190.)

Kompletní poznámky k verzi pro každý Předběžná verze rozhraní ASP.NET MVC 3 najdete na webu ASP.NET na následující adrese URL:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Poznámky k instalaci

Pokud chcete nainstalovat, ASP.NET MVC 3 RTM pomocí instalačního programu webové platformy (instalace webové platformy), naleznete na následující stránce:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Alternativně si můžete stáhnout instalační program pro RTM technologie ASP.NET MVC 3 pro sadu Visual Studio 2010 z následující stránky:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 je možné nainstalovat a spustit vedle sebe s ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Požadavky na software

Běhové komponenty ASP.NET MVC 3 vyžaduje následující software:

- Rozhraní .NET framework verze 4. 

    ASP.NET MVC 3 aplikace Visual Studio 2010 tools vyžaduje následující software:
- Visual Studio 2010 nebo Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentace

Dokumentace k ASP.NET MVC je k dispozici na webu MSDN na následující adrese URL:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Kurzy a další informace o architektuře ASP.NET MVC jsou k dispozici na stránce MVC na webu ASP.NET na následující adrese URL:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Podpora

Jedná se o plně podporovanou verzi. Informace o tom, jak technickou podporu najdete [webu Microsoft Support](https://support.microsoft.com/).

Neváhejte také přidat dotazy týkající se této verze technologie ASP.NET MVC fóra, kde jsou často schopni poskytovat podporu neformální členové komunity ASP.NET:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Upgrade projektu aplikace ASP.NET MVC 2 na ASP.NET MVC 3 nástroje Update

ASP.NET MVC 3 lze nainstalovat souběžně s ASP.NET MVC 2 na stejném počítači, který poskytuje flexibilitu při výběru, kdy se má upgradovat aplikaci ASP.NET MVC 2 na ASP.NET MVC 3.

Ručně upgradovat existující aplikaci ASP.NET MVC 2 na verzi 3, postupujte takto:

1. Vytvořte nový prázdný projekt ASP.NET MVC 3 na vašem počítači. Tento projekt bude obsahovat některé soubory, které jsou požadovány pro upgrade.
2. Zkopírujte následující soubory z projektu ASP.NET MVC 3 do odpovídajícího umístění v projektu ASP.NET MVC 2. Budete muset aktualizovat všechny odkazy na knihovny jQuery pro nový název souboru (jQuery 1.5.1.js): 

    - /Views/Web.config
    - /packages.config
    - adresu /scripts/\*js
    - /Obsahem/motivy/\*.\*
3. Kopírovat *balíčky* složku v kořenovém adresáři prázdné řešení projektu ASP.NET MVC 3 na kořenovém adresáři vašeho řešení, který se nachází v adresáři, kde je umístěn soubor .sln řešení.
4. Pokud váš projekt ASP.NET MVC 2 obsahuje všechny oblasti, /Views/Web.config soubor zkopírovat *zobrazení* složky každou oblast.
5. V obou souborech Web.config v projektu ASP.NET MVC 2 globálně Hledat a nahradit verzi technologie ASP.NET MVC. Vyhledejte následující: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Nahraďte ho následujícím kódem:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. V Průzkumníku řešení, odstraňte odkaz na *System.Web.Mvc* (která poukazuje na knihovnu DLL z verze 2), pak přidejte odkaz na *System.Web.Mvc* (v3.0.0.0).
7. Přidejte odkaz na System.Web.WebPages.dll a System.Web.Helpers.dll. Tato sestavení se nacházejí v následující složky: 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET 3\Assemblies MVC
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET webové Pages\v1.0\Assemblies
8. V Průzkumníku řešení klikněte pravým tlačítkem myši na název projektu a vyberte Uvolnit projekt. Klikněte pravým tlačítkem na název projektu a vyberte Upravit *ProjectName*csproj.
9. Vyhledejte *ProjectTypeGuids* prvku a nahraďte {F85E285D-A4E0-4152-9332-AB1D724D3325} s {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Uložte změny, klikněte pravým tlačítkem na projekt a poté vyberte znovu načíst projekt.
11. V kořenovém souboru Web.config vaší aplikace, přidejte následující nastavení, která *sestavení* oddílu. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Pokud projekt odkazuje na žádné knihovny třetích stran, které jsou kompilovány pomocí ASP.NET MVC 2, přidejte následující zvýrazněný *bindingRedirect* element v souboru Web.config v kořenovém adresáři aplikace v rámci  *konfigurace* části: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Změny v architektuře ASP.NET MVC 3 nástroje Update

Tato část popisuje změny ve verzi ASP.NET MVC 3 nástroje Update od verze RTM technologie ASP.NET MVC 3.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Dialogové okno "Přidat kontroler" teď můžete vygenerovat řadiče s kódem přístupu k zobrazení a data

Generování uživatelského rozhraní je způsob, jak rychle generování kontroler a zobrazení pro vaši aplikaci. Po vygenerování kódu, můžete upravit tak, aby vyhovovala požadavkům vašeho projektu.

Ke spuštění *přidat kontroler* dialogové okno v technologii ASP.NET MVC 3, klikněte pravým tlačítkem na *řadiče* složky *Průzkumníku řešení*, klikněte na tlačítko *přidat*a potom klikněte na tlačítko *řadič*. Vylepšili jsme dialogové okno nabízí možnosti pro další generování uživatelského rozhraní.

![](mvc3-release-notes/_static/image1.png)

Ve výchozím nastavení jsou k dispozici tři šablony generování uživatelského rozhraní.

#### <a name="empty-controller"></a>Prázdný kontroler

Tato šablona vytvoří soubor prázdný kontroler. Tato šablona je ekvivalentní k nekontrolují *přidání akcí pro vytvoření, úpravy, podrobností, odstraňte scénáře* v předchozích verzích rozhraní ASP.NET MVC. Pokud jste zvolili, nejsou dostupné žádné další možnosti.

#### <a name="controller-with-empty-readwrite-actions"></a>Kontroler s akcemi čtení/zápisu prázdný

Tato šablona generuje soubor kontroler, který má všechny metody požadované akce, ale žádný implementační kód v rámci metod. Tato šablona je ekvivalentní k kontrola *přidání akcí pro vytvoření, úpravy, podrobností, odstraňte scénáře* v předchozích verzích rozhraní ASP.NET MVC. Pokud jste zvolili, nejsou dostupné žádné další možnosti.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Kontroler s akcemi čtení/zápisu a zobrazeními, s využitím rozhraní Entity Framework

Tato šablona umožňuje rychle vytvořit pracovní položku dat uživatelského rozhraní. Generuje kód, který zpracovává celou řadu běžných požadavků na scénáře, jako je následující:

- *Přístup k datům*. Generovaný kód čte a zapisuje entit v databázi. Funguje s Entity Framework Code First přístup, pokud zvolíte existující třída kontextu dat, nebo pokud necháte vygenerovat novou šablonu *DbContext* třídy. Také pracuje s Entity Framework Database First nebo Model první přístup Pokud zvolíte existující *ObjectContext* třídy.
- *Ověření*. Generovaný kód používá vazby modelu ASP.NET MVC a metadata funkce tak, aby odeslání formuláře jsou ověřené podle pravidel deklarovat v třídě modelu. To zahrnuje předdefinovaných ověřovacích pravidel, jako *vyžaduje* a *StringLength* atributy a vlastní ověřovací pravidla.
- *Vztah jednoho k několika*. Pokud definujete vztahy cizího klíče 1 n mezi třídách modelu, generovaný kód vytvoří rozevíracích seznamech pro výběr související entity. Můžete definovat následující třídy modelu Entity Framework Code First konvencemi: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Když je pak generování uživatelského rozhraní řadiče pro *produktu* třídy, jeho zobrazení vám umožní uživatelům si vybrat *kategorie* objekt pro každou *produktu* instance.

    Tato šablona umožňuje další možnosti v *přidat kontroler* dialogové okno. Pro *třída modelu*, můžete použít libovolnou třídu modelu ve vašem řešení, který určuje typ dat, které budou uživatelé moct vytvářet nebo upravovat:
- Pokud chcete používat Entity Framework Code First, můžete všechny třídy modelu.
- Pokud používáte Entity Framework Database First nebo Entity Framework Model First, nezapomeňte vybrat třídu entity definované v konceptuálním modelu.

Pro *třída kontextu dat*, provedete tyto možnosti:

- Pokud chcete použít Code First a nemají žádný existující kontext dat třídy, zvolte ** nový kontext dat **. Třída kontextu dat se vygeneruje pak za vás.
- Pokud chcete použít Code First a mít existující třída kontextu dat, vyberte ho tady. Aktualizují se zachovat třídy modelu, který jste zvolili.
- Pokud používáte Database First nebo první Model, vyberte váš objekt context – třída tady.

Pro zobrazení zvolte zobrazovací modul, který chcete použít, nebo zvolte Žádný, pokud nechcete, aby scaffold všechna zobrazení.

Můžete vybrat rozšířená Optionsto určit další možnosti vygenerovaných zobrazení. Můžete například rozložení nebo hlavní stránku.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Vylepšení "ASP.NET MVC 3 nový projekt" dialogové okno

Dialogové okno, které použijete k vytvoření nových projektech ASP.NET MVC 3 obsahuje několik vylepšení, jak je uvedeno níže.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Nové šablony "Intranetový projekt"

Šablona projektu seznam obsahuje novou šablonu intranetovou aplikaci. Tato šablona obsahuje nastavení pro vytváření webové aplikace s využitím ověřování Windows namísto ověřování pomocí formulářů. Protože aplikace sítě intranet vyžaduje některé nastavení služby IIS, které nelze zapouzdřené v šabloně projektu, šablona obsahuje soubor readme s pokyny, jak zajistit, že šablona projektu pracovat ve službě IIS. Dokumentace k novou šablonu intranetovou aplikaci je k dispozici na webu MSDN na následující adrese URL:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Šablony projektu jsou teď povolené HTML5

Dialogové okno Nový projekt teď obsahuje možnost Přidat funkce specifické pro HTML5 šablon projektu. Výběr možnosti způsobí, že zobrazení chcete vygenerovat, které obsahují nové HTML5 `<header>`, `<footer>`, a `<navigation>` elementy.

Všimněte si, že starší verze prohlížeče nepodporuje značky HTML5. Toto omezení vyřešit, šablony projektů HTML5 obsahovat odkaz na knihovny Modernizr. (Viz další části.)

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Šablony projektů nyní obsahují Modernizr 1.7

Modernizr je knihovna jazyka JavaScript, které přinášejí podporu pro šablony stylů CSS 3 a HTML5 v prohlížečích, které se ještě nepodporují tyto funkce. Tato knihovna je zahrnutý jako předinstalovaný balíček NuGet v šablonách pro projekty ASP.NET MVC 3. Další informace o Modernizr najdete v tématu [ http://www.modernizr.com/ ](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Šablony projektů obsahují aktualizované verze jQuery, uživatelské rozhraní jQuery a jQuery ověření

Šablony projektů nyní zahrnují následující verze jQuery skriptů:

- 1.5.1 jQuery
- jQuery ověření 1.8
- uživatelské rozhraní jQuery 1.8.11

Tyto knihovny jsou k dispozici jako předem nainstalovaných balíčků NuGet.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Šablony projektů nyní obsahují ADO.NET Entity Framework 4.1 jako předinstalovaný balíček NuGet

Zahrnuje ADO.NET Entity Framework 4.1 Code First funkce. Kód je nejprve nový vzor vývoje pro ADO.NET Entity Framework, který poskytuje alternativu k existující databázi první a první Model vzory.

Kód se nejprve zaměřuje kolem definovat model pomocí třídy POCO ("prostý staré CLR objekty") napsané v jazyce Visual Basic nebo C#. Tyto třídy lze mapovat k existující databázi nebo použije k vygenerování schématu databáze. Další konfigurace, může být zadán pomocí *DataAnnotations* atributy nebo pomocí rozhraní API fluent.

Dokumentace pro používání kódu Firstwith ASP.NET MVC je k dispozici na webu ASP.NET na následující adresy URL:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Šablony projektů zahrnout jako předinstalované balíčky NuGet knihoven jazyka JavaScript

Když vytvoříte nový projekt ASP.NET MVC 3, projekt obsahuje soubory jazyka JavaScript již bylo zmíněno dříve (například knihovny Modernizr) nainstalováním je pomocí nástroje NuGet místo přímo přidávání skriptů do složky skriptů v šabloně projektu obsah. Umožňuje aktualizovat skripty na nejnovější verzi, když se vydávají nové verze skriptů pomocí balíčku NuGet.

Například s ohledem na frekvenci nových verzí jQuery, verze jQuery zahrnuty v šabloně projektů v určitém okamžiku budou zastaralá. Ale protože je zahrnutý jako nainstalovaným balíčkem NuGet jQuery, vás upozorníme v dialogovém okně NuGet když jsou k dispozici novější verze jQuery.

JQuery zahrnuje číslo verze v názvu souboru, a proto jQuery aktualizuje na nejnovější verzi také vyžaduje aktualizaci `<script>` značka, která odkazuje na soubor jQuery používat nový název souboru. Další knihovny součástí skriptu neobsahují číslo verze do pole Název skriptu, takže je možné snadno aktualizovat své nejnovější verze.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Známé problémy

- V některých případech může instalace může selhat s chybová zpráva "instalace selhalo s kódem chyby (0x80070643)". Informace o tom, jak tento problém obejít, najdete v části [článku znalostní báze 2531566](https://support.microsoft.com/kb/2531566).
- Generování uživatelského rozhraní pro přidání kontroleru není generování uživatelského rozhraní entity, které budou využívat podporu dědičnosti entity v Entity Framework. Mějme například základní *osoba* třídu, která dědí *Student* třídy, generování uživatelského rozhraní *Student* třídy způsobí generovaného kódu, který nebude zkompilován.
- Vytvoření nového projektu ASP.NET MVC 3 ve složce řešení způsobí, že *NullReferenceException* chyby. Alternativním řešením je vytvoření projektu ASP.NET MVC 3 v kořenové složce řešení a přesuňte ho do složky řešení.
- Technologie IntelliSense pro syntaxi Razor nefunguje při ReSharper je nainstalována. Pokud máte nainstalovaným rozšířením ReSharper a chcete využít výhod podpory funkce Razor IntelliSense v architektuře ASP.NET MVC 3, naleznete v příspěvku [funkce Razor Intellisense a ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri níž se probírá, jak je používat společně ještě dnes.
- Během instalace se zobrazí dialogové okno přijetí smlouvy EULA licenční podmínky v okně, která je menší, než bylo zamýšleno.
- Při úpravách zobrazení Razor (cshtml nebo. *vbhtml* souboru), zobrazení. ASP.NET MVC 3 nezahrnuje žádné fragmenty kódu pro zobrazení syntaxe Razor... Zobrazí se fragmenty kódu pro aspxselecting fragment kódu pro architekturu ASP.NET MVC
- Pokud instalace technologie ASP.NET MVC 3 pro aplikaci Visual Web Developer Express na počítači, kde není nainstalovaná sada Visual Studio a pak instalaci aplikace Visual Studio, je třeba přeinstalovat ASP.NET MVC 3. Visual Studio a Visual Web Developer Express sdílet součásti, které jsou upgradovány architektuře ASP.NET MVC 3 instalační program. Stejný problém platí při instalaci ASP.NET MVC 3 pro sadu Visual Studio na počítači, který bez Visual Web Developer Express a pak později nainstalujete Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Změny ve verzi RTM technologie ASP.NET MVC 3

Tato část popisuje změny a opravy chyb od verze RC2 provedeny ve verzi RTM technologie ASP.NET MVC 3.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Změna: Aktualizován verzi uživatelské rozhraní jQuery 1.8.7

Šablony projektů ASP.NET MVC pro sadu Visual Studio bylo aktualizováno, aby zahrnovalo nejnovější verzi knihovny uživatelského rozhraní jQuery. Šablony zahrnují také minimální sadu souborů prostředků vyžaduje jQuery uživatelského rozhraní, jako je například přidružené šablony stylů CSS a soubory obrázků.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Změny: Změnit výchozí ModelMetadataProvider zpět do DataAnnotationsModelMetadataProvider

V RC2 verzi technologie ASP.NET MVC 3 zavedené *CachedDataAnnotationsMetadataProvider* třída, která poskytuje ukládání do mezipaměti na existující *DataAnnotationsModelMetadataProvider* třídy jako zlepšení výkonu. Nicméně některé chyby byly hlášeny s touto implementací tak, aby tato změna má vrátit a přesunout do projektu MVC termínů, které je k dispozici na [skvělá webová sada ASP.NET](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Opraveno: Vkládání část výrazu Razor, která obsahuje výsledky prázdné znaky v ní se vrátit zpět

Předběžné verze sady ASP.NET MVC 3 Když vložíte součástí výraz Razor, který obsahuje prázdné znaky do souboru Razor výsledný výraz, je obrácený. Představte si třeba následující blok kódu Razor:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Pokud vyberete text "první param" v první metodě a vložte ji jako argument do druhá metoda, výsledek je následující:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Správné chování je, že operaci vložení by měla za následek následující:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Tento problém chyba byla opravena v verze RTM, tak, aby výraz se správně zachovají během operace vložení.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Opraveno: Přejmenováním souboru Razor, který je otevřen v editoru zakáže barevné zvýrazňování syntaxe a IntelliSense

Přejmenování souboru Razor pomocí Průzkumníku řešení, když je soubor otevřen v okně editoru způsobí, že se zvýraznění syntaxe a IntelliSense přestane fungovat pro tento soubor. Tato chyba byla opravena, aby zvýraznění a technologie IntelliSense jsou zachována po přejmenování.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Známé problémy

- Pokud zavřete Visual Studio 2010 SP1 Beta při otevřené konzole Správce balíčků NuGet, Visual Studio dojde k chybě a pokusí o restartování. Tato chyba bude opravena ve verzi RTM sady Visual Studio 2010 SP1.
- ASP.NET MVC 3 instalační program je pouze možné nainstalovat první verzi správce balíčků NuGet. Po dokončení instalace počáteční verze NuGet můžete nainstalovat a aktualizovat pomocí Správce rozšíření pro Visual Studio. Pokud už máte nainstalován nástroj NuGet, přejděte do Galerie rozšíření Visual Studio k aktualizaci na nejnovější verzi balíčku nuget.
- Vytvoření nového projektu ASP.NET MVC 3 ve složce řešení způsobí, že *NullReferenceException* chyby. Alternativním řešením je vytvoření projektu ASP.NET MVC 3 v kořenové složce řešení a přesuňte ho do složky řešení.
- Instalační program může trvat déle než předchozí verze technologie ASP.NET MVC pro dokončení. Je to proto, že je aktualizace součástí sady Visual Studio 2010.
- Technologie IntelliSense pro syntaxi Razor nefunguje při ReSharper je nainstalována. Pokud máte nainstalovaným rozšířením ReSharper a chcete využít výhod podpory funkce Razor IntelliSense v architektuře ASP.NET MVC 3, naleznete v příspěvku [funkce Razor Intellisense a ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri níž se probírá, jak je používat společně ještě dnes.
- CCSHTML a VBHTML zobrazením vytvořeným v Beta verzi technologie ASP.NET MVC 3 nemají jejich akci sestavení správně nastavená, s výsledkem, který toto zobrazení jsou typy vynecháno, pokud je projekt publikován. Hodnota akce sestavení pro tyto soubory by měla být nastavená na "Obsah". ASP.NET MVC 3 RTM řeší tento problém pro nové soubory, ale nemá správné nastavení pro existující soubory pro projekt vytvořeny pomocí předprodejní verze.
- ![](mvc3-release-notes/_static/image3.png)
- Během instalace se zobrazí dialogové okno přijetí smlouvy EULA licenční podmínky v okně, která je menší, než bylo zamýšleno.
- Při úpravách zobrazení Razor (cshtml soubor), přejít na kontroler položky nabídky v sadě Visual Studio nebude k dispozici, a neexistují žádné fragmenty kódu.
- Pokud instalace technologie ASP.NET MVC 3 pro aplikaci Visual Web Developer Express na počítači, kde není nainstalovaná sada Visual Studio a pak instalaci aplikace Visual Studio, je třeba přeinstalovat ASP.NET MVC 3. Visual Studio a Visual Web Developer Express sdílet součásti, které jsou upgradovány architektuře ASP.NET MVC 3 instalační program. Stejný problém platí při instalaci ASP.NET MVC 3 pro sadu Visual Studio na počítači, který bez Visual Web Developer Express a pak později nainstalujete Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Nejnovější změny

- V předchozích verzích rozhraní ASP.NET MVC vytvořte akci, kterou jsou filtry každý požadavek s výjimkou v několika případech. Toto chování bylo nikdy zaručené chování, ale pouze podrobnost implementace a smlouvu pro filtry vzít v úvahu je Bezstavová. V architektuře ASP.NET MVC 3 filtry jsou uložené v mezipaměti agresivnější. Proto všechny filtry vlastní akce, které nesprávně ukládají stav instance může být nefunkční.
- Pořadí zpracování pro filtry výjimek se změnilo filtry výjimek, které mají stejný *pořadí* hodnotu. V ASP.NET MVC 2 a dříve, filtry výjimek na řadiči, které mají stejné *pořadí* hodnoty jako na metodu akce jsou spuštěny před filtry výjimek v metodě akce. To obvykle by tomu bylo při použití filtrů výjimek bez zadaného *pořadí* hodnotu. V architektuře ASP.NET MVC 3 Toto pořadí změněno tak, aby nejprve provede nejspecifičtější obslužná rutina výjimky. Stejně jako v předchozích verzích Pokud *pořadí* explicitně zadaná vlastnost, filtry jsou spuštěny v uvedeném pořadí.
- Novou vlastnost s názvem *FileExtensions* byl přidán do *VirtualPathProviderViewEngine* základní třídy. Když ASP.NET vyhledá zobrazení podle cesty (ne podle názvu), jsou považovány za pouze zobrazení s příponou souboru obsažených v této nové vlastnosti seznamu. Toto je rozbíjející změny v aplikacích, kde chcete-li povolit vlastní příponu souboru pro zobrazení webových formulářů je zaregistrovaný poskytovatel vlastního sestavení a poskytovateli odkazuje tato zobrazení pomocí úplnou cestu, nikoli název. Alternativním řešením je změnit hodnotu *FileExtensions* vlastnosti do vlastního souboru příponu.
- Implementace objekt pro vytváření vlastní zařízení, které přímo implementovat *IControllerFactory* rozhraní musí poskytnout implementaci nového *GetControllerSessionBehavior* metodu, která byla Přidat do rozhraní v této verzi. Obecně se doporučuje, není toto rozhraní implementují přímo a místo toho odvodit třídu z *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Změny v rozhraní ASP.NET MVC 3 RC2

Tato část popisuje změny (nové funkce a opravy chyb) ve verzi ASP.NET MVC 3 RC2 od verze RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Projekt šablony změnit jQuery 1.4.4, jQuery ověření 1.7 a jQuery UI 1.8.6

Šablony projektů pro ASP.NET MVC 3 teď obsahují nejnovější verze jQuery, jQuery ověření a jQuery UI. jQuery UI je novým rozšířením šablony projektů a poskytuje widgetech užitečné uživatelského rozhraní. Další informace o uživatelské rozhraní jQuery, najdete jejich domovské stránky: [ http://jqueryui.com/ ](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Třídy přidané "AdditionalMetadataAttribute"

Můžete použít *AdditionalMetadataAttribute* třídy k naplnění *ModelMetadata.AdditionalValues* slovník pro vlastnosti modelu.

Předpokládejme například, že model zobrazení má vlastnosti, které by měly být zobrazeny pouze správce. Tento model může být komentována atributem nový atribut pomocí AdminOnly jako klíč a hodnotu true jako hodnotu jako v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Tato metadata je k dispozici žádné šablony zobrazení nebo editoru při vykreslení zobrazení modelu produktu. Je jenom na vás jako vývojář aplikace k interpretaci informace metadat.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Vylepšené zobrazení generování uživatelského rozhraní

Šablony T4 teď používá pro generování uživatelského rozhraní zobrazení generovat volání do metody helper šablony, jako *EditorFor* místo pomocných rutin, jako *TextBoxFor*. Tato změna zlepšuje podporu pro metadata o modelu ve formě atributy dat. Poznámka při dialogové okno Přidat zobrazení generuje zobrazení.

Přidat zobrazení generování uživatelského rozhraní také zahrnuje vylepšenou detekční a využití informacemi o primárním klíči pro model podle úmluvy. Například dialogové okno Přidat zobrazení využívá tyto informace k zajištění, že není vygenerovanou hodnotu primárního klíče jako pole Upravitelný formulář.

Výchozí upravit a vytvořit šablony obsahují odkazy na skripty jQuery potřebné pro ověřování na straně klienta.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Přidání Html.Raw – metoda

Ve výchozím nastavení zobrazit syntaxi Razor modul umístí kódování HTML všechny hodnoty. Například následující fragment kódu kóduje HTML uvnitř proměnné pozdrav tak, aby se zobrazí na stránce jako `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

Nové *Html.Raw* metoda poskytuje jednoduchý způsob zobrazení nezakódovaný jazyk HTML, když je obsah známé jako bezpečné. Následující příklad zobrazí řetězce stejné, ale řetězec je vykreslen jako značky:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Vlastnosti přejmenováno "Controller.ViewModel" a "Zobrazit" k "Objekt ViewBag"

Dříve *ViewModel* vlastnost *řadič* korespondenční k *zobrazení* vlastnost zobrazení. Obě tyto vlastnosti poskytují způsob, jak získat přístup k hodnotám z *objektu ViewDataDictionary* pomocí syntaxe dynamické přistupujícího objektu vlastnosti. Obě vlastnosti byly přejmenovány být stejné, aby nedocházelo k záměně a byly konzistentnější.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Přejmenované "ControllerSessionStateAttribute" třída "SessionStateAttribute"

*ControllerSessionStateAttribute* třídy byla zavedena ve verzi RC sady ASP.NET MVC 3. Bude stručnější byla přejmenována vlastnost.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Přejmenované RemoteAttribute vlastnost "Pole" na "AdditionalFields"

*RemoteAttribute* třídy *pole* vlastnost způsobila nejasnostem mezi uživateli. Přejmenování tuto vlastnost na *AdditionalFields* vysvětluje svůj záměr.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Přejmenovat "SkipRequestValidationAttribute" k "AllowHtmlAttribute"

*SkipRequestValidationAttribute* atribut se přejmenoval na *AllowHtmlAttribute* pro lepší reprezentaci její zamýšlené použití.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Metoda změněné "Html.ValidationMessage" k zobrazení první užitečné chybovou zprávu

*Html.ValidationMessage* metoda opravena zobrazíte první užitečné chybovou zprávu místo pouze první chyba.

Během vazby modelu *ModelState* slovník je možné naplnit z více zdrojů s chybovými zprávami o vlastnosti, včetně ze samotného modelu (pokud ho implementuje *IValidatableObject* ), z atributů ověření použitý pro vlastnost a z výjimky vyvolané při přístupu k vlastnosti.

Když *Html.ValidationMessage* metoda zobrazí ověřovací zprávu, přeskočí položky stavu modelu, které zahrnují výjimku, protože nejsou obecně určeny pro koncového uživatele. Místo toho metodu vyhledá první zprávu ověření, který není spojen s výjimku a zobrazí tuto zprávu. Pokud žádná taková zpráva nenajde, použije se výchozí obecné chybové zprávy, který je spojen s výjimkou první.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Oprava @model deklarace není přidávají prázdné znaky v dokumentu

V dřívějších verzích <em>@model</em> deklarace v horní části zobrazení přidat prázdný řádek do vykresleného výstupu protokolu HTML. Tato chyba byla opravena, aby se deklarace nezavádí prázdné znaky.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Vlastnost přidání "FileExtensions" pro moduly zobrazení pro podporu modulu názvy

Modul zobrazení lze vrátit zobrazení pomocí cesty ke explicitní zobrazení jako v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

První modul zobrazení se vždy pokusí k vykreslení zobrazení. Výchozí modul zobrazení webových formulářů je první zobrazovací modul; vzhledem k tomu, že modul webového formuláře nelze vykreslit zobrazení Razor, dojde k chybě. Teď máte moduly zobrazení *FileExtensions* vlastnost, která se používá k určení, jaké přípony souborů, které podporují. Tato vlastnost je zaškrtnuté políčko při ASP.NET určuje, zda modul zobrazení může mít za následek souboru. Toto je zásadní změnu a další podrobnosti jsou součástí [Breaking Changes](#_Toc2_BC) část tohoto dokumentu.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Pomocné rutiny opravené "LabelFor" a vygenerovat správnou hodnotu pro atribut "For"

Chyba byla opravena where *LabelFor* metoda vykreslí *pro* atribut, který odpovídá *vstupní* elementu *název* místo atributu jeho ID. Podle W3C *pro* atribut by měl odpovídat *vstupní* ID elementu.

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Metoda dlouhodobého "RenderAction" dát přednost explicitní hodnoty během vazby modelu

V dřívějších verzích explicitní hodnoty, které bylo předáno *RenderAction* metoda byly se ignorovat ve prospěch formuláře aktuální hodnoty během vazby modelu uvnitř podřízená akce. Oprava zajistí, že explicitní hodnoty přednost během vazby modelu.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Nejnovější změny

- V předchozích verzích rozhraní ASP.NET MVC filtry akcí byly vytvořeny na požadavek s výjimkou v několika případech. Toto chování bylo nikdy zaručené chování, ale pouze podrobnost implementace a smlouvu pro filtry vzít v úvahu je Bezstavová. V architektuře ASP.NET MVC 3 filtry jsou uložené v mezipaměti agresivnější. Proto všechny filtry vlastní akce, které nesprávně ukládají stav instance může být nefunkční.
- Pořadí zpracování pro filtry výjimek se změnilo filtry výjimek, které mají stejný *pořadí* hodnotu. V ASP.NET MVC 2 a dříve, filtry výjimek na kontroler, který má stejný *pořadí* hodnoty jako na metodu akce byly provedeny před filtry výjimek v metodě akce. To by obvykle být případ, kdy byly použity filtry výjimek bez zadaného *pořadí* hodnotu. V architektuře ASP.NET MVC 3 Toto pořadí změněno tak, aby nejprve provede nejspecifičtější obslužná rutina výjimky. Stejně jako v předchozích verzích Pokud *pořadí* explicitně zadaná vlastnost, filtry jsou spuštěny v uvedeném pořadí.
- Novou vlastnost s názvem *FileExtensions* byl přidán do *VirtualPathProviderViewEngine* základní třídy. Když ASP.NET vyhledá zobrazení podle cesty (ne podle názvu), jsou považovány za pouze zobrazení s příponou souboru obsažených v této nové vlastnosti seznamu. Toto je rozbíjející změny v aplikacích, kde chcete-li povolit vlastní příponu souboru pro zobrazení webových formulářů je zaregistrovaný poskytovatel vlastního sestavení a poskytovateli odkazuje tato zobrazení pomocí úplnou cestu, nikoli název. Alternativním řešením je změnit hodnotu *FileExtensions* vlastnosti do vlastního souboru příponu.
- Implementace objekt pro vytváření vlastní zařízení, které přímo implementovat <em>IControllerFactory</em> rozhraní musí poskytnout implementaci nového <em>GetControllerSessionBehavior</em>  <em>Metoda, která byla přidána do rozhraní v této verzi</em>. Obecně se doporučuje, není toto rozhraní implementují přímo a místo toho odvodit třídu z <em>DefaultControllerFactory</em>.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Známé problémy

- ASP.NET MVC 3 instalační program je pouze možné nainstalovat první verzi správce balíčků NuGet. Po dokončení instalace počáteční verze NuGet můžete nainstalovat a aktualizovat pomocí Správce rozšíření pro Visual Studio. Pokud už máte nainstalován nástroj NuGet, přejděte do Galerie rozšíření Visual Studio k aktualizaci na nejnovější verzi balíčku nuget.
- Vytvoření nového projektu ASP.NET MVC 3 ve složce řešení způsobí, že *NullReferenceException* chyby. Alternativním řešením je vytvoření projektu ASP.NET MVC 3 v kořenové složce řešení a přesuňte ho do složky řešení.
- Instalační program může trvat déle než předchozí verze technologie ASP.NET MVC pro dokončení. Je to proto, že je aktualizace součástí sady Visual Studio 2010.
- Technologie IntelliSense pro syntaxi Razor nefunguje při ReSharper je nainstalována. Pokud máte nainstalovaným rozšířením ReSharper a chcete využít výhod podpory funkce Razor IntelliSense v ASP.NET MVC 3 RC2, naleznete v příspěvku [funkce Razor Intellisense a ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) na blogu Hadi Hariri níž se probírá, jak je používat společně ještě dnes.
- CSHTML a VBHTML zobrazením vytvořeným v Beta verzi technologie ASP.NET MVC 3 nemají jejich akci sestavení správně nastavená, s výsledkem, který toto zobrazení jsou typy vynecháno, pokud je projekt publikován. *Akce sestavení* hodnoty pro tyto soubory musí být nastavená na obsah ". ASP.NET MVC 3 RC2 řeší tento problém pro nové soubory, ale nemá správné nastavení pro existující soubory pro projekt vytvořený v Beta verzi.![](mvc3-release-notes/_static/image4.png)
- Během instalace se zobrazí dialogové okno přijetí smlouvy EULA licenční podmínky v okně, která je menší, než bylo zamýšleno.
- Při úpravách zobrazení Razor (cshtml soubor), přejít na kontroler položky nabídky v sadě Visual Studio nebude k dispozici, a neexistují žádné fragmenty kódu.
- Pokud instalace technologie ASP.NET MVC 3 pro aplikaci Visual Web Developer Express na počítači, kde není nainstalovaná sada Visual Studio a pak instalaci aplikace Visual Studio, je třeba přeinstalovat ASP.NET MVC 3. Visual Studio a Visual Web Developer Express sdílet součásti, které jsou upgradovány architektuře ASP.NET MVC 3 instalační program. Stejný problém platí při instalaci ASP.NET MVC 3 pro sadu Visual Studio na počítači, který bez Visual Web Developer Express a pak později nainstalujete Visual Web Developer Express.
- Instalace technologie ASP.NET MVC 3 RC 2 neaktualizuje NuGet, pokud už máte nainstalovaný. Chcete upgradovat NuGet, přejděte do Správce rozšíření sady Visual Studio a jeho měla zobrazit jako k dispozici aktualizace. NuGet můžete upgradovat na nejnovější verzi z něj.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 verze Release Candidate

ASP.NET MVC Release Candidate byla vydána 9 Listopad 2010.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Nové funkce ve verzi RC: technologie ASP.NET MVC 3

Tato část popisuje funkce, které byly zavedeny v ASP.NET MVC 3 RC verze od vydání Beta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>Správce balíčků NuGet

ASP.NET MVC 3 zahrnuje Správce balíčků NuGet (dříve označované jako NuPack), což je nástroj pro správu integrovaného balíčku pro přidání knihoven a nástrojů pro projekty aplikace Visual Studio. Tento nástroj automatizuje kroky, které vývojáři si ještě dnes k získání knihovny do jejich stromu zdrojového kódu.

Můžete pracovat s NuGet jako nástroj příkazového řádku, jako okno Integrovaná konzola v sadě Visual Studio 2010 v místní nabídce sady Visual Studio a jako sada rutin prostředí PowerShell.

Další informace o systému NuGet najdete [dokumentace pro Nuget](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Vylepšené "Nový projekt" dialogové okno

Když vytvoříte nový projekt, dialogové okno Nový projekt teď umožňuje určit zobrazovací modul, stejně jako typ projektu ASP.NET MVC.

![](mvc3-release-notes/_static/image5.png)

Podpora pro úpravu v seznamu šablon a zobrazení modulů uvedených v dialogovém okně je součástí této verze.

Výchozí šablony jsou následující:

Prázdný. Obsahuje minimální sadu souborů pro projektu ASP.NET MVC, včetně výchozí strukturu adresářů pro projekty ASP.NET MVC, Site.css soubor, který obsahuje výchozí styly ASP.NET MVC a skripty adresář, který obsahuje výchozí soubory jazyka JavaScript.

Internetové aplikace. Obsahuje ukázkové funkce, která popisuje způsob používání poskytovatele členství s architekturou ASP.NET MVC.

Seznamu šablon projektu, který se zobrazí v dialogovém okně je uvedený v registru Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Nerelační řadiče

Nové *ControllerSessionStateAttribute* vám dává větší kontrolu nad chování stavu relace pro kontrolery zadáním [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) hodnota výčtu.

Následující příklad ukazuje, jak vypnout stavu relace pro všechny požadavky na kontroleru.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

Následující příklad ukazuje, jak nastavit stav relace jen pro čtení pro všechny požadavky na kontroleru.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Nové atributy ověření

#### <a name="compareattribute"></a>CompareAttribute

Nové *CompareAttribute* ověřovací atribut umožňuje porovnat hodnoty dvě různé vlastnosti objektu modelu. V následujícím příkladu *ComparePassword* vlastnost musí odpovídat *heslo* pole, aby byla platná.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

Nové *RemoteAttribute* ověřovací atribut využívá výhod vzdáleného validátoru jQuery ověření plug v, který umožňuje ověřování na straně klienta pro volání metody na serveru, který provede samotné ověření.

V následujícím příkladu *uživatelské jméno* vlastnost má *RemoteAttribute* použít. Při úpravě této vlastnosti v zobrazení pro úpravy, ověření klienta bude volat akci s názvem *UserNameAvailable* na *UsersController* třídy s cílem ověřit toto pole.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

Následující příklad ukazuje odpovídající kontroler.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Ve výchozím nastavení název vlastnosti, která se atribut používá k odeslání na metodu akce jako parametr řetězce dotazu.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Nová přetížení pro metody "LabelForModel" a "LabelFor"

Přibyla nová přetížení pro *LabelFor* a *LabelForModel* metody, které umožňují zadat text popisku. Následující příklad ukazuje, jak používat tato přetížení.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Podřízená akce ukládání výstupu do mezipaměti

*OutputCacheAttribute* podporuje ukládání výstupu do mezipaměti z podřízené akce, které jsou volány pomocí *Html.RenderAction* nebo *Html.Action* pomocné metody. Následující příklad ukazuje zobrazení, která volá jinou akci.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

*GetDate* akce je opatřen poznámkou *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Při spuštění tohoto kódu je do mezipaměti výsledek volání Html.Action("GetDate") 100 sekund.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>"Přidat zobrazení" vylepšení pole dialogového okna

Když přidáte zobrazení se silnými typy, dialogové okno Přidat zobrazení nyní odfiltruje další není použitelné typy než v předchozích verzích, jako je například mnoho typů rozhraní .NET Framework core. Název třídy a ne plně kvalifikovaný typ název, který bude snazší najít typy navíc nyní řazení seznamu. Název typu se nyní zobrazí jako v následujícím příkladu:

Název třídy (obor názvů)

V dřívějších verzích to by byla zobrazena jako následující:

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Ověření detailní žádosti

*Vyloučit* vlastnost *atribut ValidateInputAttribute* už neexistuje. Místo toho, aby žádost o ověření u určitých vlastností objektu modelu přeskočeno během vazby modelu pomocí nové *SkipRequestValidationAttribute*.

Předpokládejme například, že metody akce se používá k úpravě blogový příspěvek:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

Následující příklad ukazuje zobrazení modelu pro blogový příspěvek.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Když uživatel odešle některé kód pro vlastnost Description, vazby modelu se nezdaří z důvodu ověření žádosti. Chcete-li zakázat ověřování požadavek během vazby modelu pro blogový příspěvek popis, použijte *SkipRequpestValidationAttribute* na vlastnost, jak je znázorněno v tomto příkladu:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Můžete také vypnout ověření žádosti pro každou vlastnost modelu, použít *atribut ValidateInputAttribute* s hodnotou *false* na metodu akce:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Nejnovější změny

- Pořadí zpracování pro filtry výjimek se změnilo filtry výjimek, které mají stejný *pořadí* hodnotu. V ASP.NET MVC 2 a dříve, filtry výjimek na kontroler, který má stejný *pořadí* jako na metodu akce byly provedeny před filtry výjimek v metodě akce. To by obvykle být případ, kdy byly použity filtry výjimek bez zadaného *pořadí* hodnotu. V architektuře ASP.NET MVC 3 Toto pořadí změněno tak, aby nejprve provede nejspecifičtější obslužná rutina výjimky. Stejně jako v předchozích verzích Pokud *pořadí* explicitně zadaná vlastnost, filtry jsou spuštěny v uvedeném pořadí.
- Přidat novou vlastnost s názvem *FileExtensions* k *VirtualPathProviderViewEngine* základní třídy. Při hledání zobrazení cesty (a ne název), pouze zobrazení s příponou souboru obsažené v této nové vlastnosti seznamu se považuje za. Toto je zásadní změny pro ty, kteří zaregistrujte zprostředkovatele vlastního sestavení povolení rozšíření vlastního souboru pro zobrazení webových formulářů a odkazují tyto pohledy pomocí úplnou cestu, nikoli název. Alternativním řešením je změnit hodnotu *FileExtensions* vlastnosti do vlastního souboru příponu.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Známé problémy

- Instalační program může trvat déle než předchozí verze technologie ASP.NET MVC dokončit, protože aktualizuje součásti sady Visual Studio 2010.
- Přidat zobrazení vygenerovaných při výběru astrongly typu scaffold zobrazení jen pro zápis vlastnosti. Ty by měly vždy ignorovat pomocí generování uživatelského rozhraní. Dialogové okno Přidat zobrazení také vygeneruje uživatelské rozhraní vlastnosti jen pro čtení při generování "Upravit" nebo "Vytvořit" zobrazení. Vlastnosti jen pro čtení by měl automaticky generovaný pouze pro zobrazení a seznam zobrazení.
- Ladění nefunguje při instalaci ASP.NET MVC 3 spolu s asynchronní verze CTP. ASP.NET MVC 3 nemůže být nainstalované-souběžně s asynchronní verze CTP. Odinstalujte CTP asynchronní ladění opravit. Další podrobnosti najdete v článku [tento příspěvek na blogu](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) o odinstalaci všechno ASP.NET MVC 3 RC.
- Při instalaci Resharper funkce Razor Intellisense nebude fungovat. Pokud máte nainstalovaným rozšířením ReSharper a chtějí využít nabídky Razor podporu technologie intellisense v ASP.NET MVC 3 RC, přečtěte si prosím [tento příspěvek na blogu](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) od JetBrains, který popisuje způsoby, jak je používat společně ještě dnes.
- CSHTML a VBHTML zobrazením vytvořeným v beta verzi technologie ASP.NET MVC 3 nemají jejich akci sestavení správně které vynechá ze publikování. *Akce sestavení* pro tyto soubory musí být nastavená na "Obsah". ASP.NET MVC 3 RC řeší tento problém pro nové soubory, ale nemá správné nastavení pro existující soubory do projektu vytvořeného s beta verzi.
- Instalační program může trvat déle než předchozí verze technologie ASP.NET MVC dokončit, protože aktualizuje součásti sady Visual Studio 2010.
- Přidat zobrazení vygenerovaných při výběru "Úpravy" silného typu zobrazení scaffold číst pouze vlastnosti. Vlastnosti jen pro zápis, jsou automaticky pro zobrazení "Display".
- Během instalace se zobrazí dialogové okno přijetí smlouvy EULA licenční podmínky v okně, která je menší, než bylo zamýšleno.
- Instalace Visual Studio asynchronní CTP způsobuje konflikt v podobě verze Razor, která je součástí instalace nástrojů ASP.NET MVC 3. Ujistěte se, že není pokusí nainstalovat verzi CTP Visual Studia asynchronní a syntaxi Razor verze ve stejném počítači.
- Při úpravách zobrazení Razor (cshtml soubor), přejít na kontroler položky nabídky v sadě Visual Studio nebude k dispozici, a neexistují žádné fragmenty kódu.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>Beta verze technologie ASP.NET MVC 3

ASP.NET MVC 3 Beta byla vydána 6 říjnu 2010. Následující poznámky jsou specifické pro verzi Beta a jsou v souladu s žádné aktualizace nebo změny odkazuje v oddíle ASP.NET MVC 3 Release Candidate výše.

## <a id="0.1__Toc274034215"></a>  Nové Beta verze 2003V ASP.NET MVC 3

<a id="0.1__Default_validation_system"></a>Tato část popisuje funkce, které byly zavedeny v ASP.NET MVC 3 Beta verzi.

### <a id="0.1__Toc274034216"></a>  Správce balíčků NuGet

ASP.NET MVC 3 zahrnuje Správce balíčků NuGet, což je nástroj pro správu integrovaných balíčků, přidání knihoven a nástrojů pro projekty aplikace Visual Studio. Ve většině případů automatizuje kroky, které vývojáři si ještě dnes k získání knihovny do jejich stromu zdrojového kódu.

Můžete pracovat s NuGet jako nástroj příkazového řádku, jako okno Integrovaná konzola v sadě Visual Studio 2010 v místní nabídce sady Visual Studio a jako sadu rutin Powershellu.

Další informace o systému NuGet najdete [dokumentace pro NuGet](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>  Vylepšené dialogové okno Nový projekt

Když vytvoříte nový projekt, dialogové okno Nový projekt teď umožňuje určit zobrazovací modul, stejně jako typ projektu ASP.NET MVC.

![](mvc3-release-notes/_static/image6.png)

Podpora pro úpravu v seznamu šablon a zobrazení modulů uvedených v dialogovém okně není součástí této verze.

Výchozí šablony jsou následující:

Prázdný. Obsahuje minimální sadu souborů pro projektu ASP.NET MVC, včetně výchozí strukturu adresářů pro projekty ASP.NET MVC, malý soubor Site.css, který obsahuje výchozí styly ASP.NET MVC a skripty adresář, který obsahuje výchozí soubory jazyka JavaScript.

Internetové aplikace. Obsahuje ukázkové funkce, která popisuje způsob používání poskytovatele členství v ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>  Zjednodušený způsob, jak určit silného typu modelů v zobrazení Razor

Zjednodušili jsme způsob, jak určit typ modelu pro zobrazení se silnými typy Razor pomocí nových @model směrnice pro zobrazení CSHTML a @ModelType direktiv VBHTML zobrazení. V dřívějších verzích rozhraní ASP.NET MVC zadali byste, že model silného typu pro syntaxi Razor zobrazení tímto způsobem:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

V této verzi můžete použít následující syntaxi:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  Podpora pro nové rozhraní ASP.NET Web Pages pomocné metody

Nová technologie ASP.NET Web Pages zahrnuje sadu pomocné metody, které jsou užitečné pro přidání běžně používané funkce k zobrazení a kontrolery. ASP.NET MVC 3 podporuje použití těchto metod helper v rámci kontrolerů a zobrazení (podle potřeby). Tyto metody jsou obsaženy v sestavení System.Web.Helpers. Následující tabulka uvádí několik pomocných metod rozhraní ASP.NET Web Pages.

| **Pomocné rutiny** | **Popis** |
| --- | --- |
| Graf | Vykreslí grafu v zobrazení. Obsahuje metody, jako je například Chart.ToWebImage Chart.Save a Chart.Write. |
| Crypto | Používá k vytvoření správně algoritmy hash řetězce Salt a mají hodnotu hash hesla. |
| WebGrid | Vykreslí na kolekci objektů (obvykle dat z databáze) jako mřížky. Podporuje stránkování a řazení. |
| WebImage | Vykreslí obrázek. |
| Webové pošty | Odešle e-mailovou zprávu. |

Stručná referenční příručka téma, které jsou uvedeny pomocné rutiny a základní syntaxe je k dispozici jako součást dokumentace syntaxi ASP.NET Razor na následující adrese URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  Další závislosti vkládání podpory

Staví na verzi ASP.NET MVC 3 Preview 1, aktuální verze zahrnuje přidání podpory pro dvě nové služby a čtyři existující služby a vylepšená podpora pro řešení závislostí a Lokátor společných služeb.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nové rozhraní IControllerActivator pro instance podrobných Kontroleru

Nové rozhraní IControllerActivator poskytuje jemně odstupňovanou kontrolu nad jak jsou vytvořena instance kontrolerů pomocí vkládání závislostí. Následující příklad ukazuje rozhraní:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Porovnejte to role objekt factory kontroleru. Objekt factory kontroleru je implementací rozhraní IControllerFactory, který je zodpovědný k vyhledání typu kontroleru a pro vytvoření instance typu kontroleru.

Aktivátory zodpovídají pouze pro vytvoření instance typu kontroleru. Neprovádějte vyhledání typu kontroleru. Po nalezení typu správné kontroleru, objekty Factory by měly delegovat do instance IControllerActivator zpracování skutečné instance kontroleru.

Třída DefaultControllerFactory má nový konstruktor, který přijímá instanci IControllerFactory. To vám umožní použít injektáž závislostí ke správě tento aspekt vytváření řadiče, aniž byste museli přepsat výchozí chování vyhledávání typ kontroleru.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Rozhraní IServiceLocator nahradí IDependencyResolver

Založené na připomínkách komunity, betaverze ASP.NET MVC 3 nahradil užívání rozhraní IServiceLocator rozhraní IDependencyResolver slimmed dolů, která je specifická pro potřeby ASP.NET MVC. Následující příklad ukazuje nové rozhraní:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

V rámci této změny se třída ServiceLocator nahrazené také s třídou překladače závislostí. Registrace překladače závislostí je podobný starší verze technologie ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Implementace tohoto rozhraní by měly jednoduše delegovat do základního kontejneru pro vkládání závislosti k poskytnutí registrované služby pro požadovaného typu.

Pokud neexistují žádné registrované služby požadovaného typu, ASP.NET MVC očekává, že implementace tohoto rozhraní vrátí hodnotu null z GetService a vrátit prázdnou kolekci z GetServices.

Nová třída překladače závislostí umožňuje registrovat třídy, které implementují nové rozhraní IDependencyResolver nebo Lokátor společných služeb rozhraní (IServiceLocator). Další informace o lokátoru společných služeb naleznete v tématu [CommonServiceLocator na Githubu](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nové IViewActivator rozhraní pro vytvoření instance podrobné zobrazení stránky

Nové rozhraní IViewPageActivator poskytuje jemně odstupňovanou kontrolu nad jak zobrazit stránky jsou vytvořeny pomocí vkládání závislostí. To platí pro instance WebFormView i RazorView instancí. Následující příklad ukazuje nové rozhraní:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Tyto třídy nyní přijímají argument IViewPageActivator konstruktor, který umožňuje pomocí vkládání závislostí řídit, jak jsou typy ViewPage ViewUserControl a WebViewPage vytvořena instance.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Nová podpora překladač závislostí pro stávající služby

Nová verze zahrnuje podporu řešení závislostí pro následující služby:

- Zprostředkovatele ověření modelu. Třídy, které implementují ModelValidatorProvider mohou být registrovány v překladač závislostí a systém bude používat pro podporu ověřování na straně klienta a serveru.
- Zprostředkovatel metadat modelu. Do překladače závislostí lze zaregistrovat jednu třídu, která implementuje ModelMetadataProvider a systém bude používat k poskytování metadat pro systémy šablon a ověření.
- Zprostředkovatele hodnot. Třídy, které implementují ValueProviderFactory mohou být registrovány v překladač závislostí a systém bude používat k vytvoření zprostředkovatele hodnot, které se spotřebovávají adaptérem a během vazby modelu.
- Vazače modelů. Třídy, které implementují IModelBinderProvider mohou být registrovány v překladač závislostí a systém bude používat pro vytváření vazače modelů, které se spotřebovávají systém vazby modelu.

### <a id="0.1__Toc274034221"></a>  Nová podpora Nerušivého jazyka Ajax na základě jQuery

ASP.NET MVC zahrnuje Ajax pomocné metody, jako je následující:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Tyto metody pomocí JavaScriptu můžete vyvolat metodu akce na serveru, nikoli pomocí úplného zpětného odeslání. Aktualizovali jsme tuto funkci využívat jQuery nerušivý způsobem. Místo intrusively generování vložených skriptech klienta, tyto pomocné metody oddělte chování z kódu generování atributů HTML5 s použitím *data-ajax* předponu. Chování se následně použije na značky odkazováním na příslušné soubory jazyka JavaScript. Ujistěte se, že jsou odkazovány následující soubory jazyka JavaScript:

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

Tato funkce je povolená ve výchozím nastavení v souboru Web.config v architektuře ASP.NET MVC 3 nové šablony projektu, ale je zakázané ve výchozím nastavení pro existující projekty. Další informace najdete v tématu [přidat celou aplikaci příznaky pro ověřování na straně klienta a nerušivý JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) dále v tomto dokumentu.

### <a id="0.1__Toc274034222"></a>  Nová podpora Nerušivý jQuery ověření

Ve výchozím nastavení používá ASP.NET MVC 3 Beta k ověřování jQuery nerušivý způsobem k provedení ověření na straně klienta. Povolit ověření nerušivého klienta, ujistěte se, volání, například v rámci zobrazení těchto věcí:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

To vyžaduje, aby ViewContext.UnobtrusiveJavaScriptEnabled vlastnost nastavena na hodnotu true, což lze provést tak, že následující volání:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Zajistěte také že následující soubory jazyka JavaScript jsou odkazovány.

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Tato funkce je zapnutá ve výchozím nastavení v souboru Web.config v nové šablony projektu ASP.NET MVC 3, ale je zakázané ve výchozím nastavení pro existující projekty. Další informace najdete v tématu [nové příznaky celou aplikaci pro ověření klienta a nerušivý JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) dále v tomto dokumentu.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  Nové příznaky celou aplikaci pro ověření klienta a Nerušivý JavaScript

Můžete povolit nebo zakázat ověřování na straně klienta a nerušivý JavaScript globálně pomocí statické členy třídy HtmlHelper, jako v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Výchozí šablony projektu povolit nerušivý JavaScript ve výchozím nastavení. Můžete také povolit nebo zakázat tyto funkce v kořenovém souboru Web.config vaší aplikace pomocí následujících nastavení:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Vzhledem k tomu, že povolíte tyto funkce ve výchozím nastavení se seznámili s nová přetížení třída HtmlHelper, které vám umožňují přepíší výchozí nastavení, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Z důvodu zpětné kompatibility jsou standardně deaktivovány obě tyto funkce.

### <a id="0.1__Toc274034224"></a>  Nová podpora pro kód, který se spustí před spuštěním zobrazení

Nyní můžete umístit soubor s názvem \_viewstart.cshtml (nebo \_viewstart.vbhtml) v adresáři zobrazení a přidejte kód, který bude sdílena mezi více pohledy v tomto adresáři a jeho podadresářích. Může například přidat následující kód do \_viewstart.cshtml stránku ve složce ~/Views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Tím se nastaví na stránce rozložení pro každé zobrazení v rámci zobrazení složky a všechny její podsložky rekurzivně. Při zobrazení vykreslované, kód \_viewstart.cshtml soubor se spustí před spuštěním kódu zobrazení. \_Viewstart.cshtml kód platí pro každé zobrazení v této složce.

Ve výchozím nastavení, kód \_viewstart.cshtml soubor platí i pro zobrazení v libovolné podsložce. Jednotlivé podsložky však může mít svou vlastní verzi \_viewstart.cshtml soubor; v tomto případě místní verze má přednost. Například pro spuštění kódu, která je společná pro všechna zobrazení pro HomeController, umístit \_viewstart.cshtml souboru ve složce ~/Views/Home.

### <a id="0.1__Toc274034225"></a>  Nová podpora pro syntaxi Razor VBHTML

Předchozí verze preview rozhraní ASP.NET MVC rozšířili možnosti pro zobrazení pomocí syntaxe Razor založené na jazyce C#. Tato zobrazení použít příponu souboru cshtml. Jako součást probíhající práci na podporu Razor ASP.NET MVC 3 Beta zavádí podporu pro syntaxi Razor v jazyce Visual Basic, který používá soubor příponou vbhtml.

Úvod do pomocí syntaxe jazyka Visual Basic v VBHTML stránky najdete v kurzu na následující adrese URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  Podrobnější kontrolu nad atribut ValidateInputAttribute

ASP.NET MVC je vždy součástí atribut ValidateInputAttribute třídy, které vyvolá základní infrastruktury ověření požadavku ASP.NET abyste měli jistotu, že příchozí žádost neobsahuje potenciálně škodlivý vstup. Ve výchozím nastavení je povoleno ověření vstupu. Je možné zakázat žádost o ověření pomocí atributu atribut ValidateInputAttribute, jako v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Mnoho webových aplikací, ale mají jednotlivá pole formuláře, které je potřeba povolit HTML, zatímco zbývající pole by neměla. Třída atribut ValidateInputAttribute teď umožňuje určit seznam polí, které by neměl být zařazen ověření žádosti.

Pokud vyvíjíte blogu modul, můžete chtít povolit značky v polích text a shrnutí. Tato pole může být reprezentován dvěma vstupní element s atributem název odpovídá názvu vlastnosti ("Text" a "Přehled"). Pokud chcete zakázat žádost o ověření pro tato pole pouze, zadejte názvy (oddělený čárkami) ve vlastnosti vyloučení ValidateInput třídy, jako v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  Pomocné rutiny převádějí podtržítka pomlčky pro zadané pomocí anonymních objektů názvy atributu HTML

Pomocné metody umožňují určit atribut dvojice název/hodnota pomocí anonymní objekt, jako v následujícím příkladu:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Tento přístup není umožňují používat pomlčky v názvu atributu, protože pomlčkou nelze použít pro název vlastnosti v technologii ASP.NET. Ale jsou důležité pro vlastní atributy HTML5; pomlčky například HTML5 používá předpona "data-".

Ve stejnou dobu podtržítka nelze použít pro názvy atributů ve formátu HTML, ale jsou platné v rámci názvy vlastností. Proto pokud zadáte atributů s použitím anonymní objekt a názvy atributů obsahovat podtržítko, převede pomocné metody podtržítka na pomlčky. Například následující syntaxe pomocné rutiny používá podtržítkem:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

Předchozí příklad vykreslí následující kód při spuštění pomocné rutiny:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  Opravy chyb

Výchozí šablony objektu šablony pomocné rutiny EditorFor a DisplayFor teď podporuje řazení určený ve vlastnosti DisplayAttribute.Order. (V předchozích verzích se nastavení pořadí nepoužil.)

Ověřování na straně klienta teď podporuje ověřování přepsané vlastnosti, které jsou použity atributy ověření.

Ve výchozím nastavení je teď zaregistrované JsonValueProviderFactory.

## <a id="0.1__Toc274034229"></a>  Rozbíjející změny v

Pořadí zpracování pro filtry výjimek změnil pro filtry výjimek, které mají stejnou hodnotu pořadí. V ASP.NET MVC 2 a dříve filtry výjimek na řadiči se stejným pořadím jako na metodu akce byly provedeny před filtry výjimek v metodě akce. Obvykle by to tento případ při bez zadaného pořadí hodnoty byly použity filtry výjimek. V architektuře ASP.NET MVC 3 Toto pořadí změněno tak, aby nejprve provede nejspecifičtější obslužná rutina výjimky. Stejně jako v předchozích verzích, pokud vlastnost pořadí je explicitně zadán, filtry jsou spuštěny v uvedeném pořadí.

## <a id="0.1__Toc274034230"></a>  Známé problémy

Během instalace se zobrazí dialogové okno přijetí smlouvy EULA licenční podmínky v okně, která je menší, než bylo zamýšleno.

Zobrazení syntaxe Razor nemají podporu technologie IntelliSense nebo zvýraznění syntaxe. Předpokládá se, že bude podpora pro syntaxi Razor v sadě Visual Studio zahrnuty jako součást na novější verzi.

Při úpravách zobrazení Razor (CSHTML soubor), <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> položky nabídky přejít na kontroler v sadě Visual Studio nebude k dispozici, a neexistují žádné fragmenty kódu.

Při použití @model zobrazení syntaxe pro určení silného typu CSHTML, klávesové zkratky specifické pro jazyk pro typy nejsou rozpoznány. Například @model int nebude fungovat, ale @model Int32 bude fungovat. Alternativní řešení této chyby je použití skutečný typ název, když zadáte typ modelu.

Při použití @model syntaxe pro určení zobrazení se silnými typy CSHTML (nebo @ModelType k určení VBHTML zobrazení se silnými typy), nejsou podporovány typy připouštějící hodnotu Null a deklarace pole. Například @model int? není podporován. Místo toho použijte `@model Nullable<Int32>`. Syntaxe @model také nepodporuje string []; místo toho použijte `@model IList<string>`.

Při upgradu projektu aplikace ASP.NET MVC 2 na ASP.NET MVC 3, ujistěte se, že jste do oddílu appSettings souboru Web.config přidejte následující:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Existuje známý problém, který způsobí, že ověřování pomocí formulářů vždy přesměrovat neověřené uživatele na ~/Account/přihlášení, ignoruje se používá v souboru Web.config nastavení ověřování formulářů. Alternativním řešením je přidat následující nastavení aplikace.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  Právní omezení

© 2011 Microsoft Corporation. Všechna práva vyhrazena. Tento dokument se poskytuje "jako-je." Informace a názory vyjádřené v tomto dokumentu včetně adres URL a jiných internetových webových odkazů, mohou změnit bez předchozího upozornění. Nesete veškerá rizika s jejich použitím.

Tento dokument neobsahuje jste žádná zákonná práva na duševní vlastnictví produktů společnosti Microsoft. Můžete kopírovat a používat tento dokument pro osobní a referenční účely.
