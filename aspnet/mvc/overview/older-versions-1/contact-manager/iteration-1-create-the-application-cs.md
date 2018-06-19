---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: 'Iterace #1 – Vytvoření aplikace (C#) | Microsoft Docs'
author: microsoft
description: 'V první iteraci vytvoříme obraťte se na správce v nejjednodušší způsob, jak to možné. Nemůžeme přidat podporu pro základní databázových operací: vytvořit, číst, aktualizovat a D....'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: 30f626511164363fea2195a05e73aeee5764933b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877382"
---
<a name="iteration-1--create-the-application-c"></a>Iterace #1 – Vytvoření aplikace (C#)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhněte si kód](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> V první iteraci vytvoříme obraťte se na správce v nejjednodušší způsob, jak to možné. Nemůžeme přidat podporu pro základní databázových operací: vytvořit, číst, aktualizovat a odstranit (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Vytvoření aplikace ASP.NET MVC správy kontaktů (VB)

Z této série kurzů využijeme celou aplikaci obraťte se na správu od začátku ukončíte. Obraťte se na správce aplikace umožňuje ukládat kontaktní údaje - názvy, telefonní čísla a e-mailové adresy – seznam osob.

Přes několikrát jsme sestavení aplikace. S každé iteraci jsme postupně zlepšení aplikace. Cílem tohoto více iterace přístupu je vám umožní pochopit důvod pro každé změně.

- Iterace #1 – Vytvoření aplikace. V první iteraci vytvoříme obraťte se na správce v nejjednodušší způsob, jak to možné. Nemůžeme přidat podporu pro základní databázových operací: vytvořit, číst, aktualizovat a odstranit (CRUD).

- Iterace #2 – zpřístupnění aplikace vypadat dobrý. V této iteraci můžeme vylepšit vzhled aplikace Úprava výchozí stránky předlohy zobrazení ASP.NET MVC a stylů CSS.

- Iterace #3 – přidání ověřování formuláře. V třetím iteraci přidáme ověření základní formulář. Jsme zabránit neoprávněným osobám v odeslání formuláře bez dokončení vyžadovaná pole formuláře. Můžeme také ověřit e-mailových adres a telefonních čísel.

- Iterace #4 - li aplikaci volně vázány. V této třetí iteraci jsme využít výhod několik softwaru vzory návrhu na bylo snazší spravovat a upravovat aplikace obraťte se na správce. Například můžeme Refaktorovat naše aplikace pro použití vzoru úložiště a vzoru vkládání závislostí.

- Iterace #5 – vytvoření testování částí. V páté iteraci jsme snadněji naše aplikace spravovat a upravovat přidáním testování částí. Jsme model třídy modelu našich dat a vytvářet testy částí pro naše řadiče a logiku ověření.

- Iterace #6 - použití vývoje řízeného testováním. V této šesté iteraci přidáme nové funkce pro naši aplikaci tak, že nejprve zápis testů částí a psaní kódu pro testování částí. V této iteraci přidáme kontaktní skupiny.

- Iterace #7 – přidání funkci Ajax. V sedmého iteraci jsme přidáním podpory pro Ajax zvýšit rychlost reakce a výkon aplikace.

## <a name="this-iteration"></a>Tato iterace

V této první iteraci jsme sestavení základní aplikace. Cílem je sestavení obraťte se na správce nejrychleji a nejsnáze způsobem. V novějších iterací jsme zvýšit návrhu aplikace.

Obraťte se na správce aplikace je základní aplikace vycházející z databáze. Aplikace můžete použít k vytvoření nových kontaktů, upravte existující kontakty a odstraňovat kontakty.

V této iteraci jsme proveďte následující kroky:

1. Aplikace ASP.NET MVC
2. Vytvořit databázi k ukládání naše kontaktů
3. Generovat třídu modelu pro naše databáze s Microsoft Entity Framework
4. Vytvoření akce kontroleru a zobrazení, která umožňuje nám seznam všech kontaktů v databázi
5. Vytvoření akce kontroleru a zobrazení, která umožňuje vytvořit nový kontakt v databázi
6. Vytvoření akce kontroleru a zobrazení, která umožňuje nám chcete upravit existující skupiny v databázi
7. Vytvoření akce kontroleru a zobrazení, která umožňuje nám odstranit existující skupiny v databázi

## <a name="software-prerequisites"></a>Požadovaný software

V aplikacích ASP.NET MVC musíte mít Visual Studio 2008 nebo Visual Web Developer 2008 nainstalovaná ve vašem počítači (Visual Web Developer je bezplatnou verzi Visual Studia, která nezahrnuje všechny rozšířené funkce sady Visual Studio). Buď zkušební verzi sady Visual Studio 2008 nebo Visual Web Developer si můžete stáhnout z následující adresy:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Pro aplikace ASP.NET MVC s Visual Web Developer musí mít Visual Web Developer Service Pack 1 nainstalována. Bez aktualizace Service Pack 1 nelze vytvořit projekty webových aplikací.


Architektura ASP.NET MVC. Rozhraní ASP.NET MVC můžete stáhnout z následující adresy:

[https://www.asp.net/mvc](../../../index.md)

V tomto kurzu používáme Microsoft Entity Framework pro přístup k databázi. Rozhraní Entity Framework je součástí rozhraní .NET Framework 3.5 Service Pack 1. Tato aktualizace service pack můžete stáhnout z následujícího umístění:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Jako alternativu k provádění každé z těchto souborů ke stažení jeden po druhém můžete využít výhod služby instalace webové platformy (instalace webové platformy). Instalace webové platformy si můžete stáhnout z následující adresy:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Projektu ASP.NET MVC

Projekt webové aplikace ASP.NET MVC. Spusťte sadu Visual Studio a vyberte možnost nabídky **soubor, nový projekt**. **Nový projekt** otevře se dialogové okno (viz obrázek 1). Vyberte **webové** typ projektu a **webové aplikace ASP.NET MVC** šablony. Název nového projektu *ContactManager* a klikněte na tlačítko OK.


Ujistěte se, zda máte rozhraní .NET Framework 3.5 vybraný z rozevíracího seznamu v horní části vpravo **nový projekt** dialogové okno. Jinak se zobrazí šablony webové aplikace ASP.NET MVC won t.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Obrázek 01**: dialogové okno Nový projekt ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image2.png))


Aplikace ASP.NET MVC **vytvoření projektu testování částí** otevře se dialogové okno. Tento dialog můžete použít k označení, že chcete vytvořit a přidat projektu testů jednotek pro vaše řešení, když vytvoříte aplikaci ASP.NET MVC. I když jsme won t vytváření testů jednotek v této iteraci, byste měli vybrat možnost **Ano, vytvoření projektu testování částí** protože plánujeme přidat testování částí v novější iteraci. Přidání testovacího projektu při prvním vytvoření nového projektu ASP.NET MVC je mnohem jednodušší než přidání testovacího projektu po vytvoření projektu ASP.NET MVC.

> [!NOTE] 
> 
> Protože Visual Web Developer nepodporuje projektů testů, obdržíte dialogové okno Vytvoření projektu testů jednotek při použití Visual Web Developer.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Obrázek 02**: dialogové okno Vytvoření projektu testování částí ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image4.png))


Aplikace ASP.NET MVC se zobrazí v okně Průzkumníka řešení Visual Studio (viz obrázek 3). Pokud tento t zobrazí okno Průzkumník řešení, pak toto okno můžete otevřít tak, že vyberete možnost nabídky **zobrazení, Průzkumník řešení**. Všimněte si, že řešení obsahuje dva projekty: projektu ASP.NET MVC a k testovacímu projektu. ContactManager názvem projektu ASP.NET MVC a k testovacímu projektu jmenuje ContactManager.Tests.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Obrázek 03**: okno Průzkumníku řešení ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Odstraňuje se ukázkové soubory projektu

Šablona projektu ASP.NET MVC zahrnuje ukázkové soubory pro kontrolery a zobrazení. Před vytvořením nové aplikace ASP.NET MVC, odstraňte tyto soubory. Pravým tlačítkem myši na soubor nebo složku a výběrem možnosti nabídky můžete odstranit soubory a složky v Průzkumníku řešení okně **odstranit**.

Je třeba odstranit následující soubory z projektu ASP.NET MVC:

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

A je nutné odstranit následující soubory z testovacího projektu:

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>Vytvoření databáze

Obraťte se na správce aplikace je řízené databáze webové aplikace. Používáme databázi k ukládání kontaktní informace.

Rozhraní ASP.NET MVC framework s všechny moderní databáze, včetně databází systému Microsoft SQL Server, Oracle, MySQL a IBM DB2. V tomto kurzu používáme databázi systému Microsoft SQL Server. Při instalaci sady Visual Studio, jsou k dispozici možnost instalace Microsoft SQL Server Express, která je bezplatnou verzi databáze Microsoft SQL Server.

Vytvořit novou databázi kliknutím pravým tlačítkem na aplikaci\_složky dat v okně Průzkumníka řešení a výběrem možnosti nabídky **přidat, nové položky**. V **přidat novou položku** dialogovém okně, vyberte **Data** kategorie a **databáze systému SQL Server** šablony (viz obrázek 4). Název nové databáze ContactManagerDB.mdf a klikněte na tlačítko OK.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Obrázek 04**: vytvoření nové databáze Microsoft SQL Server Express ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image8.png))


Po vytvoření nové databáze, databáze se zobrazí v aplikaci\_složky dat v okně Průzkumníka řešení. Poklikejte na soubor ContactManager.mdf a otevřete okno Průzkumníka serveru a připojení k databázi.

> [!NOTE] 
> 
> Okno Průzkumníka serveru se nazývá okno Průzkumník databáze v případě Microsoft Visual Web Developer.


Okno Průzkumníka serveru můžete použít k vytvoření nové databázové objekty, například databázových tabulek, zobrazení, triggery a uložené procedury. Klikněte pravým tlačítkem na složku tabulky a vyberte možnost nabídky **přidat novou tabulku**. Návrhář tabulky databáze se zobrazí, (viz obrázek 5).


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Obrázek 05**: Návrhář tabulky databáze ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image10.png))


Je potřeba vytvořit tabulku, která obsahuje následující sloupce:

<a id="0.1_table01"></a>


| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | int | false |
| FirstName | nvarchar(50) | false |
| LastName | nvarchar(50) | false |
| Telefon | nvarchar(50) | false |
| E-mailu | nvarchar(255) | false |


První sloupec, ve sloupci Id je speciální. Je třeba označit Id sloupec jako sloupec Identity a sloupec primárního klíče. Můžete znamenat, že sloupec sloupec Identity tak, že rozšíření sloupec Properties (podívejte se v dolní části Obrázek 6) a posunete se dolů vlastnost specifikace Identity. Nastavte **(je identita)** vlastnost na hodnotu **Ano**.

Označte sloupec jako sloupec primárního klíče výběrem sloupce a kliknutím na tlačítko s ikonou klíče. Po sloupec označen jako sloupec primárního klíče, se vedle sloupec zobrazuje ikonu klíče (viz obrázek 6).

Po dokončení vytváření tabulky, klikněte na tlačítko Uložit (tlačítko ikonou floppy) k uložení nové tabulky. Zadejte název nové tabulky *kontakty*.

Po dokončení vytvoření databázové tabulky kontaktů měli byste přidat některé záznamy do tabulky. Klikněte pravým tlačítkem na tabulku kontaktů v okně Průzkumníka serveru a vyberte možnost nabídky **zobrazit Data tabulky**. Zadejte jeden nebo více kontaktů v mřížce, který se zobrazí.

## <a name="creating-the-data-model"></a>Vytvoření datového modelu

Aplikace ASP.NET MVC se skládá z modely, zobrazení a Kontrolery. Začneme vytvořením třídy modelu, která představuje tabulku kontaktů, kterou jsme vytvořili v předchozí části.

V tomto kurzu používáme Microsoft Entity Framework má automaticky generovat třídu modelu z databáze.

> [!NOTE] 
> 
> Rozhraní ASP.NET MVC není vázaný na rozhraní Entity Framework Microsoft žádným způsobem. ASP.NET MVC můžete použít s technologií přístupu k alternativní databáze, včetně NHibernate, technologie LINQ to SQL nebo ADO.NET.


Postupujte podle těchto kroků můžete vytvořit datové třídy modelu:

1. Složku modely v okně Průzkumníku řešení klikněte pravým tlačítkem a vyberte **přidat, nové položky**. **Přidat novou položku** otevře se dialogové okno (viz obrázek 6).
2. Vyberte **Data** kategorie a **ADO.NET Entity Data Model** šablony. Název datového modelu *ContactManagerModel.edmx* a klikněte na **přidat** tlačítko. Zobrazí se průvodce Entity Data Model (viz obrázek 7).
3. V **zvolte obsah modelu** krok, vyberte **generování z databáze** (viz obrázek 7).
4. V **vybrat datové připojení** krok, vyberte databázi ContactManagerDB.mdf a zadejte název *ContactManagerDBEntities* pro nastavení připojení Entity (viz obrázek 8).
5. V **zvolte si databázové objekty** kroku, zaškrtněte políčko s názvem bez přípony tabulky (viz obrázek 9). Datový model bude obsahovat všechny tabulky obsažené v databázi (je právě, kontakty tabulky). Zadejte obor názvů *modely*. Kliknutím na tlačítko Dokončit dokončete průvodce.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Obrázek 06**: dialogové okno Přidat novou položku ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image12.png))


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Obrázek 07**: Zvolte obsah modelu ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image14.png))


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Obrázek 08**: Vybrat datové připojení ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image16.png))


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Obrázek 09**: Zvolte si databázové objekty ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image18.png))


Po dokončení průvodce Entity Data Model Entity Data Model Designer se zobrazí. Návrhář zobrazí třídu, která odpovídá na všechny tabulky modelovaných. Měli byste vidět jednu třídu s názvem Kontakty.

Průvodce Entity Data Model vygeneruje názvy tříd, které jsou založené na názvy tabulek databáze. Téměř vždy musíte změnit název třídy je vygenerovat průvodcem. Klikněte pravým tlačítkem na třídu kontaktů v návrháři a vyberte možnost nabídky **přejmenovat**. Změňte název třídy z kontakty (množném čísle) kontaktovat (jednotném čísle). Po provedení změny názvu třídy, třída měla vypadat jako obrázek 10.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Obrázek 10**: vyhodnocení kontaktu ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image20.png))


V tuto chvíli jsme vytvořili naše model databáze. Obraťte se na třídu jsme můžete použít k reprezentaci konkrétní kontaktní záznam v naší databázi.

## <a name="creating-the-home-controller"></a>Vytvoření domácí řadiče

Dalším krokem je vytvoření domovské kontroleru. Řadič domovské je výchozí volána v aplikaci ASP.NET MVC.

Vytvoříte třídu kontroleru domovské kliknutím pravým tlačítkem na složku řadiče v okně Průzkumníka řešení a výběrem možnosti nabídky **přidat, řadiče** (viz obrázek 11). Všimněte si, zaškrtávací políčko s názvem bez přípony **přidejte metody akce pro vytvoření, aktualizace a podrobnosti o scénářích**. Ujistěte se, že je toto políčko zaškrtnuté před kliknutím na tlačítko **přidat** tlačítko.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Obrázek 11**: přidání řadiče Home ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image22.png))


Při vytváření řadičem domovské získání třídy v výpis 1.

**Výpis 1 - Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Výpis kontaktů

Aby bylo možné zobrazit záznamy v tabulce databáze kontakty, je potřeba vytvořit Index() akce a zobrazení indexu.

Řadičem domovské již obsahuje Index() akce. Potřebujeme upravit tuto metodu tak, aby vypadal jako výpis 2.

**Výpis 2 - Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Všimněte si, že třídy kontroleru domovské v výpis 2 obsahuje soukromé pole s názvem \_entity. \_Entity pole představuje z datového modelu entity. Používáme \_entity pole ke komunikaci s databází.

Index() metoda vrací zobrazení, který reprezentuje všechny kontakty z tabulky databáze Kontakty. Výraz \_entity. ContactSet.ToList() vrátí seznam kontaktů jako obecný seznam.

Nyní který jsme jste vytvořili Index řadič, musíme vedle vytvořit Index zobrazení. Před vytvořením indexu zobrazení, kompilace aplikace tak, že vyberete možnost nabídky **sestavení, sestavit řešení**. Projekt by měl vždycky kompilovat před přidáním v pořadí seznam tříd modelu, který se má zobrazit v zobrazení **přidat zobrazení** dialogové okno.

Vytvoření zobrazení pro Index kliknutím pravým tlačítkem myši na metodu Index() a výběrem možnosti nabídky **přidat zobrazení** (viz obrázek 12). Výběrem této možnosti nabídky otevře **přidat zobrazení** dialogové okno (viz obrázek 13).


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Obrázek 12**: Přidání zobrazení indexu ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image24.png))


V **přidat zobrazení** dialogové okno, zaškrtněte políčko s názvem bez přípony **vytvořit zobrazení silného typu**. Vyberte třídu data zobrazení ContactManager.Models.Contact a zobrazení seznamu obsahu. Zobrazení, který zobrazí seznam záznamů, obraťte se na těchto možností generuje.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Obrázek 13**: Přidat zobrazení dialogu ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image26.png))


Když kliknete **přidat** generováno tlačítko zobrazení indexu v výpis 3. Upozornění &lt;% @ % stránky&gt; direktiva, která se zobrazí v horní části souboru. Zobrazení indexu dědí z ViewPage&lt;rozhraní IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; třídy. Jinými slovy třídu modelu v zobrazení představuje seznam entity kontaktů.

Text Index zobrazení obsahuje smyčka typu foreach, který iteruje v rámci každé kontaktů reprezentována třídu modelu. Hodnoty vlastností třídy, obraťte se zobrazí v rámci tabulky HTML.

**Listing 3 - Views\Home\Index.aspx (unmodified)**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

Musíme mít jeden změnu zobrazení indexu. Vzhledem k tomu, že jsme nejsou vytváření zobrazení podrobností, jsme odebrat odkaz podrobnosti. Vyhledat a odstranit ze zobrazení indexu následující kód:

{id = položky. ID}) %&gt;

Po úpravě zobrazení indexu můžete spustit aplikace obraťte se na správce. Vyberte možnost nabídky ladění, spustit ladění nebo jednoduše stisknutím klávesy F5. Při prvním spuštění aplikace, zobrazí se dialogové okno obrázek 14. Vyberte možnost **upravit soubor Web.config, který chcete povolit ladění** a klikněte na tlačítko OK.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Obrázek 14**: povolení ladění ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image28.png))


Ve výchozím nastavení se vrátí zobrazení indexu. Toto zobrazení uvádí všechna data v tabulce databáze Kontakty (viz obrázek 15).


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Obrázek 15**: Zobrazení indexu ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image30.png))


Všimněte si, že Index zobrazení obsahuje odkaz s názvem bez přípony vytvořit nový, v dolní části zobrazení. V další části zjistěte, jak vytvořit nové kontakty.

## <a name="creating-new-contacts"></a>Vytvoření nových kontaktů

Povolit uživatelům vytvářet nové kontakty, je potřeba přidat dvě akce Create() domovské řadiče. Je potřeba vytvořit jednu akci Create(), který vrací formuláře HTML pro vytvoření nového kontaktu. Je potřeba vytvořit druhou akci Create(), který provádí vložení skutečné databáze nového kontaktu.

Nové Create() metody, které je potřeba přidat na domovskou řadič jsou obsažené v výpis 4.

**Výpis 4 - Controllers\HomeController.cs (s vytvořit metody)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

První metoda Create() mohou být vyvolány s HTTP GET, zatímco druhý Metoda Create() můžete být volána pouze pomocí HTTP POST. Jinými slovy druhé metody Create() nelze vyvolat jenom v případě, že publikování formuláře HTML. První metoda Create() jednoduše vrátí zobrazení obsahující formulář HTML pro vytvoření nového kontaktu. Druhá metoda Create() je mnohem zajímavějšího: Přidá nový kontakt do databáze.

Všimněte si, že druhé metody Create() se změnilo tak, aby přijímal instance třídy, obraťte se na. Hodnot formuláře odeslat z formuláře HTML je vázána na této třídě kontaktujte rozhraním ASP.NET MVC automaticky. Každé pole formuláře z formuláře HTML vytvořit je přiřadit k vlastnosti parametru kontaktu.

Všimněte si, že parametr kontaktujte opatřen atributem [vazby]. Atribut [vazby] slouží k vyloučení Vlastnost Id kontaktní z vazby. Vzhledem k tomu, že vlastnost Id představuje vlastnost Identity, jsme nejsou zobrazeny t chcete nastavit vlastnost Id.

V těle Create() metody rozhraní Entity Framework slouží k vložení nového kontaktu do databáze. Nový kontakt je přidán do stávající sady kontakty a SaveChanges() metoda je volána k replikaci těchto změn zpět na základní databázi.

Můžete vygenerovat formuláře HTML pro vytvoření nových kontaktů buď tyto dvě metody Create() kliknete pravým tlačítkem a výběrem možnosti nabídky **přidat zobrazení** (viz obrázek 16).


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Obrázek 16**: Přidání zobrazení pro vytváření ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image32.png))


V **přidat zobrazení** dialogovém okně, vyberte **ContactManager.Models.Contact** třídy a **vytvořit** možnost pro zobrazení obsahu (viz obrázek 17). Když kliknete **přidat** tlačítko, vytvořit zobrazení je generován automaticky.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Obrázek 17**: zobrazuje stránku Rozbalit ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image34.png))


Vytvořit zobrazení obsahuje pro každé z vlastností třídy, obraťte se na pole formuláře. Kód pro vytvoření zobrazení je součástí výpis 5.

**Listing 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Po metody Create() upravit a přidat zobrazení pro vytváření, můžete spustit aplikace obraťte se na správce a vytvořit nové kontakty. Klikněte **vytvořit nový** odkaz, který se zobrazí v zobrazení indexu navigaci na zobrazení pro vytváření. Měli byste vidět zobrazení v 18 obrázek.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Obrázek 18**: vytvořit zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image36.png))


## <a name="editing-contacts"></a>Úpravy kontaktů

Přidání funkce pro úpravu záznamu kontaktu je velmi podobný jako při přidávání funkce pro vytváření nových kontaktní záznamů. Nejprve musíme přidejte dva nové upravit metody do třídy kontroleru domovské. Tyto nové metody Edit() jsou obsažené v výpis 6.

**Výpis 6 - Controllers\HomeController.cs (pomocí metod úpravy)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

První Edit() metoda je volána operací GET protokolu HTTP. Tato metoda, která reprezentuje Id záznamu kontaktu upravovaný je předán parametr s Id. Rozhraní Entity Framework se používá k načtení kontaktu, který odpovídá Id. Vrátí se zobrazení obsahující formulář HTML pro úpravu záznamu.

Druhá metoda Edit() provede skutečné aktualizace do databáze. Tato metoda přijímá instanci třídy kontakt jako parametr. Rozhraní ASP.NET MVC váže pole formuláře z formuláře upravit pro tuto třídu automaticky. Všimněte si, že neuděláte t include – atribut [vazby] při úpravě kontakt (potřebujeme hodnotu vlastnosti Id).

Rozhraní Entity Framework se používá pro uložení upravených kontakt do databáze. Původní kontakt musí nejdřív načíst z databáze. Dále je k zaznamenání změny kontakt volání metody Entity Framework ApplyPropertyChanges(). Nakonec je volání metody Entity Framework SaveChanges() a zachová tak změny do základní databáze.

Můžete vygenerovat zobrazení, které obsahuje formulář upravit tak, že kliknete pravým tlačítkem na metodu Edit() a výběrem možnosti nabídky přidat zobrazení. V dialogovém okně Přidat zobrazení, vyberte **ContactManager.Models.Contact** třídy a **upravit** zobrazit obsah (viz obrázek 19).


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Obrázek 19**: Přidání zobrazení upravit o ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image38.png))


Když kliknete na tlačítko Přidat, se automaticky vytvoří nové zobrazení upravit. Formuláře HTML, který je generován obsahuje pole, které odpovídají každé vlastnosti třídy, obraťte se na (viz seznam 7).

**Listing 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Odstraňování kontaktů

Pokud chcete odstranit kontakty budete muset přidat do třídy kontroleru domovské dvě Delete() akce. Je první akcí Delete() zobrazí formulář potvrzení odstranění. Druhou akci Delete() provede skutečného odstranění.

> [!NOTE] 
> 
> Později v iterace #7, jsme upravte obraťte se na správce tak, aby podporoval jednom kroku odstranit Ajax.


Tyto dvě nové metody Delete() jsou obsažené v výpis 8.

**Výpis 8 - Controllers\HomeController.cs (metody odstranění)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

Vrátí první metoda Delete() formuláři potvrzení odstranění kontaktní záznam z databáze (viz Figure20). Druhý Metoda Delete() provádí operace skutečného odstranění v databázi. Po původní kontakt byla načtena z databáze, se nazývají metody Entity Framework DeleteObject() a SaveChanges() provést odstranění databáze.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Obrázek 20**: zobrazení potvrzení odstranění ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image40.png))


Potřebujeme upravit zobrazení indexu tak, aby obsahoval odkaz pro odstraňování kontaktů záznamů (viz obrázek 21). Je nutné přidat následující kód do stejné buňky tabulky, který obsahuje odkaz pro úpravy:

Html.ActionLink ({id = položky. ID}) %&gt;


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Obrázek 21**: Index zobrazení s odkaz pro úpravy ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image42.png))


Dále je potřeba vytvořit zobrazení potvrzení odstranění. Klikněte pravým tlačítkem na metodu Delete() ve třídě domovské řadiče a vyberte možnost nabídky přidat zobrazení. (Viz obrázek 22) se zobrazí dialogové okno Přidat zobrazení.

Na rozdíl od v případě zobrazení seznamu, vytvořit a upravit, dialogové okno Přidat zobrazení neobsahuje možnost pro vytvoření zobrazení odstranit. Místo toho vyberte **ContactManager.Models.Contact** třída dat a **prázdný** zobrazit obsah. Prázdné zobrazení, obsahu možnost bude potřeba vytvořit zobrazení, si vyberete.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**Obrázek 22**: Přidání zobrazení potvrzení odstranění ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image44.png))


Obsah zobrazení odstranění je obsažený v výpis 9. Toto zobrazení obsahuje formulář, který potvrdí, zda by měla být konkrétní kontakt odstranit (viz obrázek 21).

**Listing 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Změna názvu řadiče výchozí

Může nepokoušejte jste, že je název třídy Naše řadiče pro práci s kontakty s názvem HomeController třídy. T by neměl mít řadičem název ContactController?

Tento problém je snadné dostatečně opravit. Nejprve musíme Refaktorovat název domovské řadiče. Třída HomeController otevřete v editoru kódu sady Visual Studio, klikněte pravým tlačítkem na název třídy a vyberte možnost nabídky **Refaktorovat přejmenování**. Výběrem této možnosti nabídky, otevře se dialogové okno Přejmenovat.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Obrázek 23**: refaktoring název řadiče ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image46.png))


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Obrázek 24**: V dialogovém okně přejmenovat ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image48.png))


Pokud přejmenujete vaší třídy kontroleru, Visual Studio bude aktualizovat název složky ve složce zobrazení. Visual Studio bude přejmenujte složku \Views\Home \Views\Contact složky.

Když provedete tuto změnu, aplikace nebude mít řadič domovské. Při spuštění aplikace, získáte v obrázku 25 chybovou stránku.


[![Dialogové okno Nový projekt](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Obrázek 25**: žádný výchozí řadič ([Kliknutím zobrazit obrázek v plné velikosti](iteration-1-create-the-application-cs/_static/image50.png))


Je potřeba aktualizovat výchozí trasu v souboru Global.asax pro použití řadiče obraťte se na místo domovské řadiče. Otevřete soubor Global.asax a upravte výchozí řadiče, používá výchozí trasa (viz seznam 10).

**Listing 10 - Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Po provedení těchto změn, obraťte se na správce bude fungovat správně. Nyní použije třídy kontroleru kontakt jako řadič výchozí.

## <a name="summary"></a>Souhrn

V této první iteraci jsme vytvořili základní obraťte se na správce aplikace v nejrychlejší způsob možné. Vzali jsme výhody sady Visual Studio automaticky vygenerovat kód počáteční pro naše kontrolery a zobrazení. Vzali jsme také výhod rozhraní Entity Framework má automaticky generovat naše databáze třídy modelu.

V současné době jsme seznamu, vytvářet, upravovat a odstraňovat kontaktní záznamy s aplikací, obraťte se na správce. Jinými slovy jsme můžete provádět všechny operace databáze basic vyžadována databáze webových aplikací.

Bohužel se naše aplikace má některé problémy. První a I váhání to přijmout, obraťte se na správce aplikace není nejvíce atraktivní aplikace. Musí být některé pracovní návrhu. V další iterace podíváme jsme jak změnit výchozí zobrazení stránky předlohy a zlepšit vzhled aplikace stylů CSS.

Druhý jsme ještě implementována žádné ověřování formuláře. Například není nic zabránit odeslání formuláře kontaktů vytvořit bez nutnosti zadávat hodnoty pro všechna pole formuláře. Kromě toho můžete zadat neplatné telefonní čísla a e-mailové adresy. Začneme o vyřešení problému ověřování formuláře v iteraci #3.

Nakonec a co je nejdůležitější na aktuální iteraci aplikace, obraťte se na správce nelze snadno změnit ani zachována. Například logika přístup k databázi je zaručená vpravo do akce kontroleru. To znamená, že jsme nelze upravit naše data přístup kód beze změny naše řadiče. V novějších iterací jsme prozkoumejte vzory návrhu softwaru, které jsme můžete implementovat, aby odolnější vůči změnit obraťte se na správce.

> [!div class="step-by-step"]
> [Next](iteration-2-make-the-application-look-nice-cs.md)
