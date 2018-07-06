---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: 'Iterace #1 – Vytvoření aplikace (VB) | Dokumentace Microsoftu'
author: microsoft
description: 'V první iteraci vytvoříme Správce kontaktů v Nejjednodušším způsobem, jak je to možné. Přidáváme podporu pro základní databázových operací: vytvoření, čtení, aktualizace a D...'
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: aee177164293c178fa7d2d4acfb60f85dc98bb05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803001"
---
<a name="iteration-1--create-the-application-vb"></a>Iterace #1 – Vytvoření aplikace (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout kód](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> V první iteraci vytvoříme Správce kontaktů v Nejjednodušším způsobem, jak je to možné. Přidáváme podporu pro základní databázových operací: vytváření, čtení, aktualizace a odstranění (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Vytvoření aplikace pro správu kontaktů ASP.NET MVC (VB)

V této sérii kurzů jsme integrovali celou aplikaci kontakt správy od začátku na dokončení. Obraťte se na správce aplikace umožňuje ukládat kontaktní údaje - jména, telefonní čísla a e-mailové adresy – seznam lidí.

Vytváříme aplikaci přes více iterací. S každou iterací zvyšujeme postupně aplikace. Cílem tohoto přístupu s více iterace je vám pomohl pochopit důvod pro každou změnu.

- Iterace #1 – Vytvoření aplikace. V první iteraci vytvoříme Správce kontaktů v Nejjednodušším způsobem, jak je to možné. Přidáváme podporu pro základní databázových operací: vytváření, čtení, aktualizace a odstranění (CRUD).

- Ujistěte se, iterace #2 – vylepšení vzhledu aplikace. V této iterace můžeme zlepšit vzhled aplikace tak, že změna výchozích hlavní stránka zobrazení ASP.NET MVC a stylů CSS.

- Iterace #3 – Přidání ověřovacího formuláře. Ve třetí iterace přidáme ověření základní formulář. Můžeme zabránit neoprávněným osobám v odeslání formuláře bez dokončení vyžadovaná pole formuláře. Také ověření e-mailových adres a telefonních čísel.

- Iterace #4 – vytvoření volně spárované aplikace. V této třetí iterace můžeme využít několik způsobů návrhu v softwaru k bylo snazší spravovat a upravovat aplikace Správce kontaktů. Například Refaktorovat jsme naši aplikaci pomocí vzoru úložiště a vzor vkládání závislostí.

- Iterace #5 – vytvoření testů jednotek. V páté iteraci jsme snadněji naší aplikace spravovat a upravovat tak, že přidáte testy jednotek. Jsme napodobení našich tříd datových modelů a vytváření testů jednotek pro naše řadiče a logiku ověřování.

- Iterace #6 – použití vývoje řízeného testováním. V této iterace šestého přidáme nové funkce do naší aplikace tak, že nejprve zápis testů jednotek a psaní kódu pro testování částí. V této iterace můžeme přidat skupiny kontaktů.

- Iterace #7 – přidání funkcí Ajax. V sedmé iteraci můžeme zlepšit rychlost reakce a výkon naší aplikace tak, že přidáte podporu pro Ajax.

## <a name="this-iteration"></a>Tuto iteraci

V této první iterace jsme vytvořit základní aplikaci. Cílem je vytvořit správce kontaktů v nejrychlejší a nejjednodušší způsob je to možné. Ve vyšším počtu iterací zvyšujeme návrhu aplikace.

Aplikace Správce kontaktů je základní aplikace řízené databáze. Aplikace můžete vytvářet nové kontakty, upravte existující kontakty a odstraňovat kontakty.

V této iterace jsme proveďte následující kroky:

1. Aplikace ASP.NET MVC
2. Vytvořit databázi pro ukládání naše kontakty
3. Generování třídy modelu pro naše database s Entity Framework společnosti Microsoft
4. Vytvoření akce kontroleru a zobrazení, které umožňuje nám to seznam všech kontaktů v databázi
5. Vytvoření akce kontroleru a zobrazení, která umožňuje vytvořit nový kontakt v databázi
6. Vytvoření akce kontroleru a zobrazení, která umožňuje upravit existující kontakt v databázi
7. Vytvoření akce kontroleru a zobrazení, která umožňuje odstranit existující kontakt v databázi

## <a name="software-prerequisites"></a>Bezpodmínečně nutný software

V aplikacích ASP.NET MVC musíte mít Visual Studio 2008 nebo Visual Web Developer 2008 nainstalovány v počítači (Visual Web Developer je bezplatná verze sady Visual Studio, která nezahrnuje všechny o pokročilých funkcích sady Visual Studio). Buď zkušební verzi sady Visual Studio 2008 nebo Visual Web Developer si můžete stáhnout z následující adresy:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Pro aplikace ASP.NET MVC s aplikaci Visual Web Developer musíte mít Visual Web Developer Service Pack 1 nainstalovaný. Bez aktualizace Service Pack 1 nelze vytvořit projekty webových aplikací.


Architektura ASP.NET MVC. Architektura ASP.NET MVC si můžete stáhnout z následující adresy:

[https://www.asp.net/mvc](../../../index.md)

V tomto kurzu používáme Microsoft Entity Framework pro přístup k databázi. Entity Framework je součástí rozhraní .NET Framework 3.5 Service Pack 1. Tato aktualizace service pack si můžete stáhnout z následujícího umístění:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp; displaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Jako alternativu k provádění jednotlivých tyto soubory ke stažení jeden po druhém můžete využít Web Platform Installer (instalace webové platformy). Instalace webové platformy si můžete stáhnout z následující adresy:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Projekt ASP.NET MVC

Projekt webové aplikace ASP.NET MVC. Spusťte sadu Visual Studio a vyberte možnost nabídky **soubor, nový projekt**. **Nový projekt** (viz obrázek 1) se zobrazí dialogové okno. Vyberte **webové** typ projektu a **webové aplikace ASP.NET MVC** šablony. Název nového projektu *ContactManager* a klikněte na tlačítko OK.


Ujistěte se, že máte vybraný z rozevíracího seznamu v horní části rozhraní .NET Framework 3.5 vpravo **nový projekt** dialogového okna. V opačném případě se zobrazí šablony webové aplikace ASP.NET MVC vyhráli t.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**Obrázek 01**: dialogové okno Nový projekt ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image2.png))


Aplikace ASP.NET MVC **vytvořit projekt testování částí** se zobrazí dialogové okno. Toto dialogové okno můžete použít k označení, že chcete vytvořit a přidat projekt testování částí do vašeho řešení při vytváření vaší aplikace ASP.NET MVC. I když vyhráli jsme t vytváření testů jednotek v této iterace, by měl vybrat možnost **Ano, vytvořit projekt testování částí** protože plánujeme přidání jednotkových testů v pozdější iterace. Přidání testovacího projektu při prvním vytvoření nového projektu ASP.NET MVC je mnohem jednodušší než přidáte projekt testu po vytvoření projektu ASP.NET MVC.

> [!NOTE] 
> 
> Protože aplikaci Visual Web Developer nepodporuje projekty testů, se nezobrazí dialogové okno Vytvořit projekt testů jednotek při použití aplikace Visual Web Developer.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**Obrázek 02**: dialogové okno pro vytvoření projektu testů jednotek ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image4.png))


Aplikace ASP.NET MVC se zobrazí v okně Průzkumník řešení Visual Studio (viz obrázek 3). Pokud don t najdete v okně Průzkumník řešení, pak toto okno můžete otevřít tak, že vyberete možnost nabídky **zobrazení, Průzkumník řešení**. Všimněte si, že toto řešení obsahuje dva projekty: projekt ASP.NET MVC a testovací projekt. ContactManager názvem projektu ASP.NET MVC a názvem ContactManager.Tests testovacího projektu.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**Obrázek 03**: okno Průzkumníka řešení ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Odstraňuje se ukázkové soubory projektu

Šablona projektu ASP.NET MVC zahrnuje ukázkové soubory pro kontrolerů a zobrazení. Před vytvořením nové aplikace ASP.NET MVC, odstraňte tyto soubory. Soubory a složky v okně Průzkumník řešení můžete odstranit kliknutím pravým tlačítkem na soubor nebo složku a vyberte možnost nabídky **odstranit**.

Je třeba odstranit následující soubory z projektu ASP.NET MVC:

- \Controllers\HomeController.vb

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

A je třeba odstranit následující soubor z testovacího projektu:

\Controllers\HomeControllerTest.vb

## <a name="creating-the-database"></a>Vytvoření databáze

Aplikace Správce kontaktů je databázově řízeného webové aplikace. Používáme databázi k ukládání kontaktní informace.

Architektura ASP.NET MVC s libovolnou moderní databází, včetně databází Microsoft SQL Server, Oracle, MySQL a IBM DB2. V tomto kurzu používáme databáze Microsoft SQL Server. Při instalaci sady Visual Studio jsou k dispozici možnost instalace Microsoft SQL Server Express, která je bezplatná verze databáze Microsoft SQL Server.

Vytvořit novou databázi kliknutím pravým tlačítkem myši aplikaci\_složce dat v okně Průzkumník řešení a vyberte možnost nabídky **přidat, nová položka**. V **přidat novou položku** dialogového okna, vyberte **Data** kategorie a **databázi systému SQL Server** šablony (viz obrázek 4). Název nové databáze ContactManagerDB.mdf a klikněte na tlačítko OK.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**Obrázek 04**: vytvoření nové databáze Microsoft SQL Server Express ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image8.png))


Po vytvoření nové databáze se databáze zobrazí v aplikaci\_složce dat v okně Průzkumník řešení. Poklikejte na soubor ContactManager.mdf a otevřete okno Průzkumníka serveru a připojení k databázi.

> [!NOTE] 
> 
> V okně Průzkumníka serveru je volána v okně Průzkumník databáze v případě Microsoft Visual Web Developer.


Okno Průzkumníka serveru můžete použít k vytvoření nové databázové objekty, jako jsou databázové tabulky, zobrazení, triggery a uložené procedury. Klikněte pravým tlačítkem na složku tabulky a vyberte možnost nabídky **přidat novou tabulku**. Návrhář tabulky databáze se zobrazí (viz obrázek 5).


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**Obrázek 05**: návrháře tabulky databázi ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image10.png))


Potřebujeme vytvořit tabulku, která obsahuje následující sloupce:

<a id="0.2_table01"></a>


| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | int | false |
| Jméno | nvarchar(50) | false |
| Příjmení | nvarchar(50) | false |
| Telefon | nvarchar(50) | false |
| E-mailu | nvarchar(255) | false |


První sloupec sloupec Id je speciální. Budete muset označit Id sloupec jako sloupec Identity a sloupec primárního klíče. Určujete, že sloupec je sloupec Identity tak, že rozšíření Properties sloupce (podívejte se v dolní části Obrázek 6) a dostanete posunutím do vlastnost specifikace Identity. Nastavte **(je identita)** k hodnotě **Ano**.

Výběrem sloupce a kliknutím na tlačítko s ikonou klíč označíte sloupec jako sloupec primárního klíče. Označeno jako sloupec jako sloupec primárního klíče, vedle sloupce se zobrazí ikona klíče (viz obrázek 6).

Po dokončení vytváření tabulky, klepněte na tlačítko Uložit (tlačítko s ikonou floppy) Chcete-li uložit nové tabulky. Zadejte název nové tabulky *kontakty*.

Po dokončení vytváření tabulky databáze kontaktů měli byste přidat některé záznamy do tabulky. Klikněte pravým tlačítkem na tabulku kontaktů v okně Průzkumníka serveru a vyberte možnost nabídky **zobrazit Data tabulky**. Zadejte jeden nebo více kontaktů v mřížce, který se zobrazí.

## <a name="creating-the-data-model"></a>Vytvoření datového modelu

Aplikace ASP.NET MVC se skládá z modelů, zobrazení a Kontrolerů. Začneme vytvořením třídu modelu, který představuje tabulku kontaktů, který jsme vytvořili v předchozí části.

V tomto kurzu používáme k automatickému vygenerování třídy modelu z databáze Entity Framework společnosti Microsoft.

> [!NOTE] 
> 
> Architektura ASP.NET MVC se neváže k Entity Frameworku Microsoft žádným způsobem. ASP.NET MVC lze pomocí technologií přístupu k alternativní databáze, včetně NHibernate, LINQ to SQL a ADO.NET.


Postupujte podle těchto kroků k vytvoření tříd datových modelů:

1. Klikněte pravým tlačítkem na složku modely v okně Průzkumník řešení a vyberte **přidat, nová položka**. **Přidat novou položku** se zobrazí dialogové okno (viz obrázek 6).
2. Vyberte **Data** kategorie a **datový Model Entity ADO.NET** šablony. Název datového modelu *ContactManagerModel.edmx* a klikněte na tlačítko **přidat** tlačítko. Zobrazí se průvodce Entity Data Model (viz obrázek 7).
3. V **výběr obsahu modelu** kroku, vyberte **Generovat z databáze** (viz obrázek 7).
4. V **vyberte datové připojení** kroku, vyberte databázi ContactManagerDB.mdf a zadejte název *ContactManagerDBEntities* pro nastavení připojení Entity (viz obrázek 8).
5. V **zvolte vaše databázové objekty** krok, zaškrtněte políčko s názvem tabulky (viz obrázek 9). Datový model bude obsahovat všechny tabulky obsažené v databázi (je jen jeden, tabulce Kontakty). Zadejte obor názvů *modely*. Kliknutím na tlačítko Dokončit dokončete průvodce.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**Obrázek 06**: dialogové okno Přidat nové položky ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image12.png))


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**Obrázek 07**: Výběr obsahu modelu ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image14.png))


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**Obrázek 08**: Vyberte datové připojení ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image16.png))


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**Obrázek 09**: Zvolte vaše databázové objekty ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image18.png))


Po dokončení Průvodce entitního modelu dat Entity Data Model Designer se zobrazí. Návrhář zobrazí pro každou tabulku modelovaných třídu, která odpovídá. Zobrazí se jedné třídy s názvem Kontakty.

Průvodce Entity Data Model vygeneruje názvy tříd, které jsou založené na názvy tabulek databáze. Téměř vždy je nutné změnit název třídy generované průvodcem knihovnou. Klikněte pravým tlačítkem na třídu kontakty v návrháři a vyberte možnost nabídky **přejmenovat**. Změňte název třídy z kontakty (množné číslo) na kontakt (jednotném čísle). Po změně názvu třídy třída by měla vypadat obrázek 10.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**Obrázek 10**: Třída Contact ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image20.png))


V tuto chvíli jsme vytvořili náš model databáze. Můžeme použít třídu kontakt představující konkrétní kontaktní záznam v databázi.

## <a name="creating-the-home-controller"></a>Vytvoření domovské Kontroleru

Dalším krokem je vytvoření naší kontroler Home. Kontroler Home, je výchozí řadič vyvolána v aplikaci ASP.NET MVC.

Vytvoříte třídu kontroleru domovské tak, že pravým tlačítkem na složku řadiče v okně Průzkumník řešení a vyberte možnost nabídky **přidat, řadič** (viz obrázek 11). Všimněte si, že zaškrtávací políčko **přidejte metody akce pro vytvoření, aktualizace a podrobnosti o scénářích**. Ujistěte se, že je toto políčko zaškrtnuté políčko, před kliknutím na tlačítko **přidat** tlačítko.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**Obrázek 11**: Přidání kontroler Home ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image22.png))


Při vytváření kontroler Home, získáte v informacích 1 třídy.

**Výpis 1 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>Seznam kontaktů

Aby bylo možné zobrazit záznamy v tabulce databáze kontaktů, potřebujeme vytvořit Index() akci a zobrazení indexu.

Kontroler Home již obsahuje Index() akci. Potřebujeme upravit tuto metodu tak, aby vypadal jako výpis 2.

**Výpis 2 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

Všimněte si, že třída kontroleru Domovská stránka v informacích 2 obsahuje soukromé pole s názvem \_entity. \_Pole entity představuje entity z modelu. Používáme \_entit pole ke komunikaci s databází.

Index() metoda vrací zobrazení, který reprezentuje všechny kontakty z databázové tabulky kontaktů. Výraz \_entity. ContactSet.ToList() vrátí seznam kontaktů podobě obecného seznamu.

Nyní, který jsme vytvoření kontroleru Index dále je třeba vytvořit zobrazení indexu. Před vytvořením indexu zobrazení, kompilovat aplikaci tak, že vyberete možnost nabídky **vytvořit, sestavit řešení**. Vždy byste měli kompilovat projekt před přidáním pořadí seznam tříd modelu, který se má zobrazit v zobrazení **přidat zobrazení** dialogového okna.

Vytvořit zobrazení indexu tak, že pravým tlačítkem myši na metodu Index() a vyberte možnost nabídky **přidat zobrazení** (viz obrázek 12). Výběrem této možnosti se otevře **přidat zobrazení** dialogového okna (viz obrázek 13).


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**Obrázek 12**: Přidání zobrazení Index ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image24.png))


V **přidat zobrazení** dialogového okna, zaškrtněte políčko s popiskem **vytvoření zobrazení se silnými typy**. Vyberte třídu ContactManager.Contact zobrazení dat a zobrazit seznam obsahu. Výběr tyto možnosti generuje zobrazení, které se zobrazí seznam záznamů kontaktů.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**Obrázek 13**: dialogové okno pro přidání zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image26.png))


Když kliknete **přidat** generováno tlačítko, zobrazení indexu v informacích 3. Všimněte si, že &lt;% @ Page %&gt; směrnice, které se zobrazí v horní části souboru. Index zobrazení dědilo ze ViewPage třídy&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; třídy. Jinými slovy tříd modelu v zobrazení představuje seznam entity kontaktů.

Text zobrazení indexu obsahuje smyčku foreach, která iteruje přes všechny kontakty, reprezentovaný třídou modelu. Hodnota každá vlastnost třídy kontaktu se zobrazí v rámci tabulku HTML.

**Listing 3 - Views\Home\Index.aspx (unmodified)**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

Budeme muset provést některé úpravy zobrazení indexu. Protože jsme se vytváření zobrazení podrobností, můžeme odebrat odkaz podrobnosti. Vyhledat a odstranit ze zobrazení indexu následující kód:

{.id = položka. % ID})&gt;

Po úpravě zobrazení indexu může správce kontaktů aplikaci spouštíte. Vyberte možnost nabídky ladění, spustit ladění nebo stisknutím klávesy F5. Při prvním spuštění aplikace, získáte dialogového okna obrázku 14. Vyberte možnost **upravit soubor Web.config pro povolení ladění** a klikněte na tlačítko OK.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**Obrázek 14**: povolení ladění ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image28.png))


Zobrazení Index se vrátí ve výchozím nastavení. Toto zobrazení uvádí všechna data z tabulky databáze kontaktů (viz obrázek 15).


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**Obrázek 15**: Index zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image30.png))


Všimněte si, že Index zobrazení obsahuje odkaz s názvem vytvořit nový v dolní části zobrazení. V další části se dozvíte, jak vytvořit nové kontakty.

## <a name="creating-new-contacts"></a>Vytváření nových kontaktů

Pokud chcete povolit uživatelům vytvářet nové kontakty, potřebujeme přidat dvě akce Create() pro kontroler Home. Potřebujeme vytvořit jednu akci Create(), který vrací formuláře HTML pro vytvoření nového kontaktu. Potřebujeme vytvořit druhou akci Create(), který provádí skutečné databáze vložení nového kontaktu.

Výpis 4 jsou součástí nové Create() metody, které je potřeba přidat pro kontroler Home.

**Část 4 – Controllers\HomeController.vb (s vytvořit metody)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

První metoda Create() lze vyvolat pomocí HTTP GET, zatímco druhá metoda Create() lze vyvolat pouze vytvoří požadavkem HTTP POST. Jinými slovy druhá metoda Create() lze vyvolat pouze při účtování formuláře HTML. První metoda Create() jednoduše vrací zobrazení, které obsahuje formulář HTML pro vytvoření nového kontaktu. Druhá metoda Create() je mnohem zajímavější: Přidá nový kontakt do databáze.

Všimněte si, že druhá metoda Create() byla změněna tak, aby přijímal instance třídy kontaktu. Hodnot formuláře, pošle z formuláře HTML jsou vázány na této třídě kontakt rozhraním ASP.NET MVC automaticky. Každé pole formuláře z formuláře HTML vytvořit je přiřazená k vlastnosti parametru kontaktu.

Všimněte si, že parametr kontakt je upravený pomocí atributu [vazby]. Atribut [vazby] umožňuje vyloučit z vazby Vlastnost Id kontaktu. Protože vlastnost Id reprezentuje vlastnost Identity, můžeme zadávat t chcete nastavit vlastnost Id.

V těle metody Create() Entity Framework slouží k vložení nového kontaktu do databáze. Nový kontakt se přidá do existující sady kontakty a je volána metoda SaveChanges() tak, aby nabízel tyto změny zpět do podkladové databáze.

Můžete generovat formulář HTML pro vytváření nových kontaktů buď ze dvou způsobů Create() kliknete pravým tlačítkem a výběrem možnosti nabídky **přidat zobrazení** (viz obrázek 16).


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**Obrázek 16**: Přidání zobrazení pro vytváření ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image32.png))


V **přidat zobrazení** dialogového okna, vyberte **ContactManager.Contact** třídy a **vytvořit** možnost pro zobrazení obsahu (viz obrázek 17). Když kliknete **přidat** tlačítko, použít příkaz pro vytvoření zobrazení se vygeneruje automaticky.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**Obrázek 17**: nezobrazí stránka Rozbalit ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image34.png))


Vytvořit zobrazení obsahuje pole formuláře pro jednotlivé vlastnosti třídy kontaktu. Kód pro zobrazení pro vytváření je součástí výpis 5.

**Listing 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Po úpravě Create() metody a přidat zobrazení pro vytváření, můžete spustit aplikaci kontaktujte správce a vytvořit nové kontakty. Klikněte na tlačítko **vytvořit nový** odkaz, který se zobrazí v zobrazení indexu a přejděte do zobrazení pro vytváření. Měli byste vidět zobrazení obrázek 18.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**Obrázek 18**: příkaz Create View ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image36.png))


## <a name="editing-contacts"></a>Úpravy kontakty

Přidání funkce pro úpravy záznamu kontaktu se podobá přidání funkce pro vytváření nových záznamů kontaktů. Nejdřív potřebujeme přidat dva nové metody upravit třídy kontroleru Domovská stránka. Tyto nové metody Edit() jsou obsaženy v informacích 6.

**Výpis 6 - Controllers\HomeController.vb (pomocí metod Edit)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

První metoda Edit() vyvolá operaci HTTP GET. Parametr Id je předaný této metodě, která představuje Id záznamu kontaktu, který právě upravujete. Entity Framework slouží k načtení kontaktu, který odpovídá Id. Vrátí se zobrazení obsahující formulář HTML pro úpravu záznamu.

Druhá metoda Edit() provádí skutečné aktualizace do databáze. Tato metoda přijímá jako parametr instance třídy kontaktu. Rozhraní ASP.NET MVC váže pole formuláře z formuláře pro úpravy pro tuto třídu automaticky. Všimněte si, že nepoužíváte t include – atribut [vazby] při úpravách kontakt (potřebujeme hodnoty vlastnosti Id).

Entity Framework se používá pro uložení upravené kontaktu k databázi. Původní kontakt musí načíst z databáze. V dalším kroku je volána metoda Entity Framework ApplyPropertyChanges() k zaznamenání změn do kontaktu. Nakonec je volána metoda Entity Framework SaveChanges() a zachová tak změny do databáze.

Můžete vytvořit zobrazení, která obsahuje formulář pro úpravy ve metodu Edit() pravým tlačítkem myši a vyberte možnost nabídky přidat zobrazení. V dialogovém okně Přidat zobrazení, vyberte **ContactManager.Models.Contact** třídy a **upravit** zobrazení obsahu (viz obrázek 19).


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**Obrázek 19**: Přidání zobrazení pro úpravy ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image38.png))


Když kliknete na tlačítko Přidat, je automaticky generovat nové zobrazení pro úpravy. Formuláře HTML, který je generován obsahuje pole, které odpovídají jednotlivým vlastnosti třídy kontakt (viz seznam 7).

**Listing 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Odstraňování kontaktů

Pokud chcete odstranit kontakty, pak budete muset přidat dvě akce Delete() třídy kontroleru Domovská stránka. První akci Delete() zobrazí formulář pro potvrzení odstranění. Druhou akci Delete() provádí skutečné odstranit.

> [!NOTE] 
> 
> Později v iteraci #7, můžeme upravit Správce kontaktů tak, aby podporoval jeden krok odstranit Ajax.


Dvě nové metody Delete() jsou obsaženy v informacích 8.

**Výpis 8 - Controllers\HomeController.vb (metod Delete)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

Vrátí první metodě Delete() formuláři potvrzení k odstranění záznamu kontaktu z databáze (viz Figure20). Druhá metoda Delete() provádí skutečné operaci v databázi. Po původní kontakt byl načten z databáze, jsou volány metody Entity Framework DeleteObject() a SaveChanges() provést odstranění databáze.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**Obrázek 20**: zobrazení potvrzení odstranění ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image40.png))


Potřebujeme upravit zobrazení indexu tak, aby obsahoval odkaz pro odstranění záznamů kontaktů (viz obrázek 21). Je potřeba přidat následující kód do stejné tabulce buňku, která obsahuje odkaz pro úpravy:

{.id = položka. % ID})&gt;


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**Obrázek 21**: Index zobrazení na mapách odkaz pro úpravy ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image42.png))


V dalším kroku se musíme vytvořit zobrazení potvrzení odstranění. Klikněte pravým tlačítkem na metodě Delete() ve třídě controller domovské a vyberte možnost nabídky přidat zobrazení. Zobrazí se dialogové okno Přidat zobrazení (viz obrázek 22).

Na rozdíl od v případě zobrazení seznamu, vytvořit a upravit, dialogové okno Přidat zobrazení neobsahuje možnost pro vytvoření zobrazení odstranit. Místo toho vybrat **ContactManager.Models.Contact** datové třídy a **prázdný** zobrazení obsahu. Vyberete možnost obsahu vyžaduje vytvořit zobrazení, chceme prázdné zobrazení.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**Obrázek 22**: Přidání potvrzení odstranění zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image44.png))


Obsah zobrazení. odstraňte je obsažen v informacích 9. Toto zobrazení obsahuje formulář, který potvrdí, zda by měla být konkrétní kontakt odstraněna (viz obrázek 21).

**Listing 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Mění název výchozího Kontroleru

Může se nepokoušejte se vám, že je název naší třídy kontroleru pro práci s kontakty s názvem třídy HomeController. Nesmí obsahovat více t kontroleru jmenovat ContactController?

Je docela jednoduché, chcete-li vyřešit tento problém. Nejdřív potřebujeme Refaktorovat název kontroler Home. Otevřete třídu HomeController v editoru kódu sady Visual Studio, klikněte pravým tlačítkem na název třídy a vyberte možnost nabídky **přejmenovat**. Výběrem této možnosti nabídky, otevře se dialogové okno pro přejmenování.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**Obrázek 23**: refaktoring názvu kontroleru ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image46.png))


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**Obrázek 24**: V dialogovém okně přejmenovat ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image48.png))


Pokud přejmenujete třídu kontroleru, bude Visual Studio update název složky ve složce zobrazení. Visual Studio se přejmenovat složku \Views\Home \Views\Contact složky.

Po provedení této změny bude mít vaše aplikace už kontroler Home. Když spustíte svou aplikaci, získáte v obrázek 25 chybovou stránku.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**Obrázek 25**: žádný výchozí kontroler ([kliknutím ji zobrazíte obrázek v plné velikosti](iteration-1-create-the-application-vb/_static/image50.png))


Potřebujeme aktualizovat výchozí trasu v souboru Global.asax pro použití kontroléru kontakt místo kontroler Home. Otevřete soubor Global.asax a upravit výchozí kontrolér používá výchozí trasa (viz seznam 10).

**Listing 10 - Global.asax.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

Po provedení těchto změn Správce kontaktů bude fungovat správně. Nyní použije třídy kontroleru kontaktu jako výchozí kontroleru.

## <a name="summary"></a>Souhrn

V této první iterace jsme vytvořili základní aplikace Správce kontaktů v nejrychlejší způsob, jak je to možné. Společnost Microsoft využil sady Visual Studio k automatickému vygenerování počáteční kód pro naše kontrolerů a zobrazení. Můžeme také využil Entity Framework k automatickému vygenerování naše databáze třídy modelu.

V současné době jsme seznamu, vytvořit, upravit a odstranit záznamů kontaktů aplikaci obraťte se na správce. Jinými slovy můžeme provádět všechny operace databáze basic vyžadované databázově řízeného webové aplikace.

Bohužel náš aplikace má některé problémy. První a já váhání to uznat, kontaktujte správce aplikace není nejvíce atraktivní aplikace. Rozpojuje úkony návrhu. V další iteraci podíváme na tom, jak můžeme změnit výchozí zobrazení stránky předlohy a ke zlepšení vzhledu aplikace stylů CSS.

Za druhé jsme neimplementovali žádné ověřování formuláře. Například nic a tím vám znemožnit odesílání kontaktní formulář vytvořit bez zadání hodnoty pro některý z pole formuláře. Kromě toho můžete zadat neplatné telefonní čísla a e-mailové adresy. Začneme o vyřešení problému ověřování formuláře v iteraci #3.

A konečně a co je nejdůležitější aktuální iterace, kontaktujte správce aplikace nelze snadno změnit ani udržuje. Například logika přístupu k databázi je vloženými vpravo na akce kontroleru. To znamená, že nemůžeme upravit, náš kód přístupu k datům beze změny naše řadiče. Ve vyšším počtu iterací budeme věnovat vzory návrhu softwaru, které můžeme implementovat aby odolnější vůči změnit správce kontaktů.

> [!div class="step-by-step"]
> [Předchozí](iteration-7-add-ajax-functionality-cs.md)
> [další](iteration-2-make-the-application-look-nice-vb.md)
