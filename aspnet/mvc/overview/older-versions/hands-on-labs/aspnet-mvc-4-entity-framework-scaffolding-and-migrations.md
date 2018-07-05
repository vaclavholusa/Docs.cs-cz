---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework generování uživatelského rozhraní a migrace | Dokumentace Microsoftu
author: rick-anderson
description: Pokud se seznámíte s metodami kontroleru ASP.NET MVC 4, nebo dokončení &quot;Pomocníci, formuláře a ověřování&quot; praktických cvičení, byste měli vědět...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: a385ffd3f0067d4ac56d592b0f2bf151fc0f8dd4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394202"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 Entity Framework generování uživatelského rozhraní a migrace

podle [Campy Web týmu](https://twitter.com/webcamps)

[Stáhněte si Web Campy školení Kit](https://aka.ms/webcamps-training-kit)

Pokud se seznámíte s metodami kontroleru ASP.NET MVC 4, nebo dokončení &quot;Pomocníci, formuláře a ověřování&quot; praktických cvičení, je třeba si uvědomit, že spoustu logiky k vytvoření, aktualizace, seznamu a odebrat entitu dat se opakuje mezi aplikace. Nemluvě, že pokud váš model obsahuje několik tříd k manipulaci s, budete chtít věnovat značný čas zápisu POST a GET metody akce pro každou operaci entity, jakož i každé zobrazení.

V tomto testovacím prostředí se dozvíte, jak používat generování uživatelského rozhraní ASP.NET MVC 4 k automatickému generování účaří vaší aplikaci CRUD (vytvoření, čtení, aktualizace a odstranění). Spouští se od jednoduchého modelu třídy a, aniž byste museli napsat jediný řádek kódu, vytvoříte kontroler, který bude obsahovat všechny operace CRUD, jakož i všechny nezbytné zobrazení. Po sestavení a spuštění jednoduché řešení, je nutné databázi aplikace, které jsou vygenerovány spolu s logikou MVC a zobrazení pro manipulaci s daty.

Kromě toho se dozvíte, jak je snadné použití migrace Entity Framework k provedení aktualizace modelu v rámci celé aplikace. Migrace rozhraní Entity Framework umožňují měnit databáze po změně modelu jednoduchých krocích. Pomocí všech těchto v úvahu budete moct sestavovat a spravovat webové aplikace efektivněji, využívat nejnovější funkce technologie ASP.NET MVC 4.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 Entity Framework generování uživatelského rozhraní a migrace](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto praktického testovacího prostředí se dozvíte, jak:

- Použití ASP.NET generování uživatelského rozhraní pro operace CRUD v zařízení.
- Změna databáze modelu s použitím migrace rozhraní Entity Framework.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Musíte mít následující položky k dokončení tohoto testovacího prostředí:

- [Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny k jeho instalaci).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Instalace

**Instalace fragmenty kódu**

Pro usnadnění práce velkou část kódu, které budete spravovat podél tohoto testovacího prostředí je k dispozici jako fragmenty kódu sady Visual Studio. K instalaci spustit fragmenty kódu **.\Source\Setup\CodeSnippets.vsi** souboru.

Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha B:](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Následující cvičení tvoří tohoto praktického testovacího prostředí:

1. [Migrace rozhraní Entity Framework pomocí generování uživatelského rozhraní ASP.NET MVC 4](#Exercise1)

> [!NOTE]
> V tomto cvičení se sadou **End** složku, která obsahuje výsledný řešení byste měli získat po dokončení výkonu. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím výkonu.


Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Cvičení 1: S migrací architektury Entity pomocí generování uživatelského rozhraní ASP.NET MVC 4

Generování uživatelského rozhraní ASP.NET MVC poskytuje rychlý způsob, jak vygenerovat operace CRUD a standardizovaným způsobem, vytváření nezbytnou logiku, která umožňuje aplikacím komunikovat s databázové vrstvě.

V tomto cvičení se dozvíte, jak používat generování uživatelského rozhraní ASP.NET MVC 4 s kódem nejprve vytvořit metody CRUD. Potom se dozvíte, jak aktualizovat váš model použití změn v databázi s použitím migrace rozhraní Entity Framework.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Úloha 1 – Vytvoření nové technologie ASP.NET MVC 4 projektu pomocí generování uživatelského rozhraní

1. Pokud ještě není otevřený, začněte **Visual Studio 2012**.
2. Vyberte **soubor | Nový projekt**. V nový projekt dialogového okna, v části **Visual C# | Web** vyberte **webové aplikace ASP.NET MVC 4**. Pojmenujte projekt tak, aby **MVC4andEFMigrations** a nastavte umístění **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** složka tohoto testovacího prostředí. Nastavte **název řešení** k **začít** a zajištění **vytvořit adresář pro řešení** je zaškrtnuté políčko. Klikněte na tlačítko **OK**.

    ![ASP.NET MVC 4 projektu dialogové okno Nový](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "nové pole dialogové okno projektu ASP.NET MVC 4")

    *Nové pole dialogové okno projektu ASP.NET MVC 4*
3. V **nového projektu ASP.NET MVC 4** dialogové okno Vyberte **internetovou aplikaci** šablony a ujistěte se, že **Razor** je vybraný **zobrazovací modul**. Klikněte na tlačítko **OK** pro vytvoření projektu.

    ![Nové aplikace ASP.NET MVC 4 Internet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nové aplikace ASP.NET MVC 4 Internet")

    *Nové aplikace ASP.NET MVC 4 Internet*
4. V Průzkumníku řešení klikněte pravým tlačítkem na **modely** a vyberte **přidat | Třída** vytvořit jednoduchou třídu osoba (POCO). Pojmenujte ji **osoba** a klikněte na tlačítko **OK**.
5. Otevřete třídu osoby a vložte následující vlastnosti.

    (Fragment - kódu *architektury ASP.NET MVC 4 a migrace rozhraní Entity Framework – vlastnosti osoby Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Klikněte na tlačítko **sestavení | Vytvoření řešení** uložte změny a sestavte projekt.

    ![Sestavení aplikace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "sestavení aplikace")

    *Sestavení aplikace*
7. V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče a vyberte **přidat | Kontroler**.
8. Název kontroleru *PersonController* a proveďte **možnosti generování uživatelského rozhraní** s použitím následujících hodnot.

   1. V **šablony** rozevíracího seznamu, vyberte **kontroler MVC s akcemi čtení/zápisu a zobrazeními, s využitím rozhraní Entity Framework** možnost.
   2. V **třída modelu** rozevíracího seznamu, vyberte **osoba** třídy.
   3. V **třída kontextu dat** seznamu vyberte  **&lt;nový kontext dat... &gt;**. Vyberte libovolný název a klikněte na tlačítko **OK**.
   4. V **zobrazení** rozevíracího seznamu, ujistěte se, že **Razor** zaškrtnuto.

      ![Přidání kontroleru osoba s generování uživatelského rozhraní](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "přidání kontroleru osoba s generování uživatelského rozhraní")

      *Přidání kontroleru osoba s generování uživatelského rozhraní*
9. Klikněte na tlačítko **přidat** můžete vytvořit nový kontroler pro osoby se generování uživatelského rozhraní. Nyní jste vygenerovali akce kontroleru, jakož i zobrazení.

    ![Po vytvoření kontroleru osoba s generování uživatelského rozhraní](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "po vytvoření kontroleru osoba s generování uživatelského rozhraní")

    *Po vytvoření kontroleru osoba s generování uživatelského rozhraní*
10. Otevřít **PersonController** třídy. Všimněte si, že úplné metody akcí CRUD byly vytvořeny automaticky.

   ![V kontroleru osoba](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "uvnitř osoba kontroleru")

   *V kontroleru osoby*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Úloha 2 běžící aplikace

V tomto okamžiku není nevytvořili databázi. V této úloze se spustit aplikaci poprvé a otestujte operace CRUD. Databáze se vytvoří v reálném čase s Code First.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. V prohlížeči, přidejte **/Person** na adresu URL a otevřete stránku osoby.

    ![Při prvním spuštění aplikace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "při prvním spuštění aplikace")

    *Aplikace: prvním spuštění*
3. Nyní bude procházení stránek osoba a otestujte operace CRUD.

    1. Klikněte na tlačítko **vytvořit nový** chcete přidat nové uživatele. Zadejte jméno a příjmení a klikněte na tlačítko **vytvořit**.

        ![Přidání nové osobě](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "přidání nové osobě")

        *Přidání nové osobě*
    2. V seznamu osoby můžete odstranit, upravit nebo přidat položky.

        ![seznam osob](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "seznam osob")

        *Seznam osob*
    3. Klikněte na tlačítko **podrobnosti** zobrazíte její podrobnosti.

        ![Podrobnosti o vaší osobě](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "podrobnosti uživatele")

        *Podrobnosti uživatele*
4. Zavřete prohlížeč a vraťte se do sady Visual Studio. Všimněte si, že vytvoříte celý CRUD entity osoba v rámci aplikace – z modelu, který má zobrazení – aniž byste museli napsat jediný řádek kódu.

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Úloha 3Aktualizace databáze pomocí migrace rozhraní Entity Framework

V této úloze budete aktualizovat databázi pomocí migrace rozhraní Entity Framework. Zjistíte, jak snadné je změna modelu a zavedení změn ve vašich databázích pomocí funkce migrace rozhraní Entity Framework.

1. Otevřete konzolu Správce balíčků. Vyberte **nástroje | Správce balíčků knihoven | Konzola správce balíčků**.
2. V konzole Správce balíčků zadejte následující příkaz:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Povolení migrace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "povolení migrace")

    *Povolení migrace*

    Příkaz povolení migrace vytvoří **migrace** složky, která obsahuje skript pro inicializaci databáze.

    ![Migrace složky](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "složky migrace")

    *Migrace složky*
3. Otevřít **Configuration.cs** souboru ve složce migrace. Vyhledejte konstruktoru třídy a změňte **AutomaticMigrationsEnabled** hodnota, která se *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Otevřete třídu osoby a přidejte atribut pro křestní jméno osoby. Pomocí tohoto nového atributu změníte model.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Vyberte **sestavení | Vytvoření řešení** v nabídce k sestavení aplikace.

    ![Sestavení aplikace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "sestavení aplikace")

    *Sestavení aplikace*
6. V konzole Správce balíčků zadejte následující příkaz:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Tento příkaz vyhledá změn v datové objekty a pak přidáte nezbytné příkazy k úpravám databáze odpovídajícím způsobem.

    ![Přidání křestní jméno](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "přidání křestní jméno")

    *Přidání křestní jméno*
7. (Volitelné) Spuštěním následujícího příkazu vygenerujte skript SQL s rozdílové aktualizace. To vám umožní aktualizovat databázi ručně (v tomto případě není nutné), nebo použít změny v jiných databázích:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generování skriptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "generování skriptu SQL")

    *Generování skriptu SQL*

    ![Aktualizace skriptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aktualizace skript SQL")

    *Aktualizace skriptu SQL*
8. V konzole Správce balíčků zadejte následující příkaz k aktualizaci databáze:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Aktualizace databáze](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "aktualizace databáze")

    *Aktualizace databáze*

    Tím se přidá **MiddleName** sloupec v **lidé** tabulku tak, aby odpovídala stávající definici **osoba** třídy.
9. Jakmile se aktualizuje databázi, klikněte pravým tlačítkem na složku Kontroleru a vyberte **přidat | Kontroler** přidat osoba kontroleru znovu (dokončete pomocí stejných hodnot). Tím se aktualizují existující metody a zobrazení přidáte nový atribut.

    ![Přidání kontroleru aktualizace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "přidává aktualizace kontroleru")

    *Aktualizace kontroleru*
10. Klikněte na tlačítko **přidat**. Vyberte hodnoty **přepsat PersonController.cs** a **přepsat přidružené zobrazení** a klikněte na tlačítko **OK**.

   ![Přidání kontroleru přepsání](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Aktualizace kontroleru*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 – spouštění aplikace

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Otevřít **/Person**. Všimněte si, aby se zajistilo uchování dat, zatímco byl přidán sloupec křestní jméno.

    ![Přidání druhé křestní jméno](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "přidání druhé křestní jméno")

    *Přidání druhé křestní jméno*
3. Vyberete-li **upravit**, bude možné přidat křestní jméno aktuálního osobě.

    ![Křestní jméno edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edition křestní jméno")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

V této praktických cvičení jste se naučili jednoduché kroky k vytvoření operace CRUD s ASP.NET MVC 4 Scaffolding pomocí libovolné třídy modelu. Pak jste se naučili provede aktualizaci začátku do konce v aplikaci – z databáze k zobrazení – s použitím migrace rozhraní Entity Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.

    ![Instalace sady Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "instalace sady Visual Studio Express")

    *Instalace sady Visual Studio Express*
4. Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Přijetí podmínek licence](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Přijetí podmínek licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Průběh instalace*
6. Až instalace skončí, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.

    ![VS Express for Web dlaždice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express for Web dlaždice*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Příloha B: používání fragmentů kódu

Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky. Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")

*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.
4. Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).
5. Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.

![Začněte psát název fragmentu kódu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")

*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*

![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")

*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.
2. Kliknutím na vyberte relevantní fragment kódu ze seznamu.

![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")

*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*
