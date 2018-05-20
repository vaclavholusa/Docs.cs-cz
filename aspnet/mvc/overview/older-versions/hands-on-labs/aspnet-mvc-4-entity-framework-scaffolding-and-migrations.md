---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework generování uživatelského rozhraní a migrace | Microsoft Docs
author: rick-anderson
description: Pokud se seznámíte s metody kontroleru architektury ASP.NET MVC 4, nebo byly dokončeny &quot;pomocné rutiny, formulářů a ověřování&quot; praktické cvičení, byste měli vědět...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 42a12ee39223a06054382dbe9b4784196a706216
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/18/2018
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 Entity Framework generování uživatelského rozhraní a migrace

Podle [webové táborech Team](https://twitter.com/webcamps)

[Stažení webové táborech cvičení Kit](https://aka.ms/webcamps-training-kit)

Pokud se seznámíte s metody kontroleru architektury ASP.NET MVC 4, nebo byly dokončeny &quot;pomocné rutiny, formulářů a ověřování&quot; praktických testovací prostředí, je třeba si uvědomit, že řadu logiku pro vytvoření, aktualizovat, seznamu a odebrat jakékoli data entity se opakuje mezi aplikace. Nechcete zmínili, že pokud váš model má několik tříd k manipulaci, bude pravděpodobně tráví mnoho času zápis POST a GET metody akce pro každou operaci entity, jakož i každé z zobrazení.

V tomto testovacím prostředí se dozvíte, jak pomocí generování uživatelského rozhraní ASP.NET MVC 4 automaticky generovat účaří CRUD vaší aplikace (vytvoření, čtení, aktualizace a odstranění). Spouštění z jednoduchého modelu třídy a, bez nutnosti napsat jediný řádek kódu, bude vytvoření kontroler, který bude obsahovat všechny operace CRUD a také všechny nezbytné zobrazení. Po vytváření a spouštění jednoduchým řešením, je nutné databázi aplikace vygenerována, společně s logiku MVC a zobrazení pro manipulaci s daty.

Kromě toho se dozvíte, jak je snadné použití Entity Framework migrace k provedení aktualizací modelu v celé vaší celou aplikaci. Entity Framework migrace vám umožní změnit databázi po změně modelu pomocí jednoduchých kroků. Pomocí všech těchto pamatovat bude možné vytvářet a udržovat efektivněji, webových aplikací s využitím nejnovějších funkcí technologie ASP.NET MVC 4.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [verze Microsoft-webové/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 Entity Framework generování uživatelského rozhraní a migrace](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí Hands-On se dozvíte, jak:

- Pomocí ASP.NET generování uživatelského rozhraní pro operace CRUD v seznamu zařízení.
- Změňte model databáze pomocí migrace Entity Framework.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Musíte mít následující položky k dokončení tohoto testovacího prostředí:

- [Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Instalace

**Instalace fragmenty kódu**

Pro usnadnění práce většinu kódu, který bude spravovat podél toto testovací prostředí je k dispozici jako fragmenty kódu v sadě Visual Studio. K instalaci fragmenty kódu spustit **.\Source\Setup\CodeSnippets.vsi** souboru.

Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha B:](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Následujícím cvičení tvoří toto Hands-On testovací prostředí:

1. [Pomocí generování uživatelského rozhraní ASP.NET MVC 4 migrace Entity Framework](#Exercise1)

> [!NOTE]
> Tento postup je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení výkonu. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení výkonu.


Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Cvičení 1: Pomocí generování uživatelského rozhraní ASP.NET MVC 4 migrace Entity Framework

Generování uživatelského rozhraní aplikace ASP.NET MVC poskytuje rychlý způsob, jak vygenerovat operace CRUD a standardizovaným způsobem, vytváření nezbytné logiky, která umožňuje aplikaci používat v databázové vrstvě.

V tomto cvičení se dozvíte, jak generování uživatelského rozhraní ASP.NET MVC 4 s kódem nejprve vytvořit pomocí metody CRUD. Potom se dozvíte, jak se bude aktualizovat váš model provádění změn v databázi pomocí Entity Framework migrace.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Úloha 1 – Vytvoření nové rozhraní ASP.NET MVC 4 projekt pomocí generování uživatelského rozhraní

1. Pokud už otevřený, spusťte **Visual Studio 2012**.
2. Vyberte **souboru | Nový projekt**. V nový projekt dialogové okno, v části **Visual C# | Webové** vyberte **webové aplikace ASP.NET MVC 4**. Pojmenujte projekt **MVC4andEFMigrations** a nastavte jeho umístění na **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** složky tohoto testovacího prostředí. Nastavte **název řešení** k **začít** a ujistěte se, **vytvořit adresář pro řešení** je zaškrtnuté. Click **OK**.

    ![ASP.NET MVC 4 projektu dialogové okno Nový](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "architektury ASP.NET MVC 4 projektu dialogové okno Nový")

    *ASP.NET MVC 4 projektu dialogové okno Nový*
3. V **nový ASP.NET MVC 4 projekt** dialogovém okně vyberte pole **Internetové aplikace** šablony a ujistěte se, že **Razor** je vybraný **zobrazovací modul**. Klikněte na tlačítko **OK** a vytvořte tak projekt.

    ![Nové aplikace ASP.NET MVC 4 Internet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nové aplikace ASP.NET MVC 4 Internetu")

    *Nové aplikace ASP.NET MVC 4 Internetu*
4. V Průzkumníku řešení klikněte pravým tlačítkem na **modely** a vyberte **přidat | Třída** vytvořit jednoduchou třídu osoba (objektů POCO). Pojmenujte ji **osoba** a klikněte na tlačítko **OK**.
5. Otevřete třídu osoby a vložte následující vlastnosti.

    (Code fragment kódu - *architektury ASP.NET MVC 4 a Entity Framework migrace - Ex1 osoba vlastnosti*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Klikněte na tlačítko **sestavení | Vytvoření řešení** uložte změny a sestavte projekt.

    ![Vytváření aplikace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "vytváření aplikace")

    *Vytváření aplikace*
7. V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče a vyberte **přidat | Řadič**.
8. Název kontroleru *PersonController* a dokončete **možnosti generování uživatelského rozhraní** s následujícími hodnotami.

   1. V **šablony** rozevíracího seznamu, vyberte **kontroler MVC s akcemi čtení/zápisu a zobrazeními, s využitím nástroje Entity Framework** možnost.
   2. V **třída modelu** rozevíracího seznamu, vyberte **osoba** třídy.
   3. V **třída kontextu dat** seznamu, vyberte  **&lt;nový kontext data... &gt;**. Vyberte libovolný název a klikněte na **OK**.
   4. V **zobrazení** rozevíracího seznamu, ujistěte se, že **Razor** je vybrána.

      ![Přidání řadiče osoba s generování uživatelského rozhraní](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "přidání řadiče osoba s generování uživatelského rozhraní")

      *Přidání řadiče osoba s generování uživatelského rozhraní*
9. Klikněte na tlačítko **přidat** k vytvoření nového řadiče pro osoby s generování uživatelského rozhraní. Byly generovány akce kontroleru, jakož i zobrazení.

    ![Po vytvoření kontroleru osoba pomocí generování uživatelského rozhraní](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "po vytvoření kontroleru osoba pomocí generování uživatelského rozhraní")

    *Po vytvoření kontroleru osoba pomocí generování uživatelského rozhraní*
10. Otevřete **PersonController** třídy. Všimněte si, že úplné metody akce CRUD byly vytvořeny automaticky.

   ![Uvnitř řadičem osoba](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "uvnitř osoba řadiče")

   *Uvnitř řadičem osoba*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Úloha 2 – spuštění aplikace

V tomto okamžiku není ještě vytvořit databázi. V této úloze se spustit aplikaci poprvé a testování operací CRUD. Databáze bude vytvořena na za chodu s Code First.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. V prohlížeči přidat **/Person** na adresu URL, otevře se stránka osoby.

    ![Prvním spuštění aplikace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "prvním spuštění aplikace")

    *Aplikace: prvním spuštění*
3. Nyní se následující stránky osoby a testování operací CRUD.

    1. Klikněte na tlačítko **vytvořit nový** chcete přidat nového uživatele. Zadejte jméno a příjmení a klikněte na tlačítko **vytvořit**.

        ![Přidání nové osobě](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "přidání nové osobě")

        *Přidání nové osobě*
    2. V seznamu osoby můžete odstranit, upravit nebo přidat položky.

        ![seznam osob](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "seznam osob")

        *Seznam osob*
    3. Klikněte na tlačítko **podrobnosti** otevřete její podrobnosti.

        ![Podrobnosti uživatele](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "podrobnosti uživatele")

        *Podrobnosti uživatele*
4. Zavřete prohlížeč a vraťte se k sadě Visual Studio. Všimněte si, že jste vytvořili celou CRUD pro entitu osoby v celé vaší aplikaci - z modelu, který má zobrazení – bez nutnosti napsat jediný řádek kódu!

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Úloha 3Aktualizace databázi pomocí Entity Framework migrace

V této úloze zaktualizuje databázi pomocí Entity Framework migrace. Zjistíte, jak je snadné a změňte model projevily změny v databázích pomocí funkce migrace Entity Framework.

1. Otevřete konzolu Správce balíčků. Vyberte **nástroje | Správce balíčků knihoven | Konzola správce balíčků**.
2. V konzole Správce balíčků zadejte následující příkaz:

    POMOCÍ PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Povolení migrace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "povolení migrace")

    *Povolení migrace*

    Příkaz Enable-migraci vytvoří **migrace** složky, která obsahuje skriptu k chybě při inicializaci databáze.

    ![Složku migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "složku Migrations")

    *Složku migrations*
3. Otevřete **Configuration.cs** soubor ve složce migrace. Vyhledejte konstruktoru třídy a změňte **AutomaticMigrationsEnabled** hodnotu *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Otevřete osoba třídu a přidejte atribut pro křestní jméno osoby. Pomocí tohoto nového atributu změníte model.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Vyberte **sestavení | Vytvoření řešení** v nabídce pro sestavení aplikace.

    ![Vytváření aplikace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "vytváření aplikace")

    *Vytváření aplikace*
6. V konzole Správce balíčků zadejte následující příkaz:

    POMOCÍ PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Tento příkaz bude hledat změny v datových objektů, a potom přidá potřebné příkazy k úpravám databáze odpovídajícím způsobem.

    ![Přidání křestní jméno](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "přidání křestní jméno")

    *Přidání křestní jméno*
7. (Volitelné) Spuštěním následujícího příkazu vygenerovat skript SQL s rozdílové aktualizace. To vám umožní aktualizovat databázi ručně (v takovém případě není nutné), nebo použít změny v ostatních databázích:

    POMOCÍ PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generování skriptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "generování skriptu SQL")

    *Generování skriptu SQL*

    ![Skript SQL aktualizace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aktualizace skript SQL")

    *Skript SQL aktualizace*
8. V konzole Správce balíčků zadejte následující příkaz k aktualizaci databáze:

    POMOCÍ PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Aktualizace databáze](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "aktualizace databáze")

    *Aktualizace databáze*

    Se přidá **MiddleName** sloupec v **osoby** tabulky tak, aby odpovídala aktuální definice **osoba** třídy.
9. Po aktualizaci databáze, klikněte pravým tlačítkem na složku řadiče a vyberte **přidat | Řadič** pro přidání osoba řadiče znovu (dokončení se stejnými hodnotami). Tím dojde k aktualizaci existující metody a zobrazení přidání nového atributu.

    ![Přidání řadiče aktualizace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "přidání řadiče aktualizace")

    *Aktualizace kontroleru*
10. Klikněte na tlačítko **přidat**. Potom vyberte hodnoty **přepsat PersonController.cs** a **přepsat přidružené zobrazení** a klikněte na tlačítko **OK**.

   ![Přidání přepsat řadiče](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Aktualizace kontroleru*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - spuštění aplikace

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Otevřete **/Person**. Všimněte si, že byla data zachována, při sloupci Křestní jméno byla přidána.

    ![Křestní jméno přidat](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "přidat křestní jméno")

    *Křestní jméno přidat*
3. Pokud kliknete na tlačítko **upravit**, nebudete moct přidat křestní jméno aktuální osobě.

    ![Křestní jméno edice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edice křestní jméno")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

V tomto testovacím prostředí praktických jste se naučili jednoduché kroky k vytvoření operace CRUD s ASP.NET MVC 4 generování pomocí libovolné třídy modelu. Pak jste zjistili, jak provést aktualizaci koncová ve vaší aplikaci - z databáze k zobrazení - pomocí migrace Entity Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nyní nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.

    ![Nainstalovat Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "nainstalovat Visual Studio Express")

    *Nainstalovat Visual Studio Express*
4. Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Vyjádření souhlasu s podmínkami licence](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Vyjádření souhlasu s podmínkami licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Průběh instalace*
6. Po dokončení instalace, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.

    ![VS Express pro Web dlaždice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express pro Web dlaždice*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Příloha B: používání fragmentů kódu

S fragmenty kódu máte všechny kód, který je nutné na dosah ruky. Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")

*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.
4. Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).
5. Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.

![Začněte psát název fragmentu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")

*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*

![Stisknutím klávesy Tab znovu a fragmentu rozšíří](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")

*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.
2. Vyberte relevantní fragment kódu ze seznamu, kliknutím na.

![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")

*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*
