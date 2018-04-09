---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Předá na testovacím: udržovatelný weby Azure: Správa změn a škálování | Microsoft Docs'
author: rick-anderson
description: V tomto testovacím prostředí zjistěte, jak Microsoft Azure umožňuje snadno vytvářet a nasazovat weby do produkčního prostředí.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: a79921681b4e742b5cd23f7119d19f4dd74c3f83
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Předá na testovacím: udržovatelný weby Azure: Správa změn a škálování
====================
Podle [webové táborech Team](https://twitter.com/webcamps)

[Stažení webové táborech cvičení Kit](http://aka.ms/webcamps-training-kit)

> Microsoft Azure umožňuje snadno vytvářet a nasazovat weby do produkčního prostředí. Ale nejsou uděláte, když je aplikace za provozu, budete právě začínat! Budete potřebovat pro zpracování Změna požadavky, aktualizace databáze, škálování a další. Naštěstí Azure App Service se můžete zahrnutých s dostatkem funkce vám pomůže ochránit vaše lokality plynulý chod.
> 
> Azure nabízí zabezpečené a flexibilní vývoj, nasazení a škálování možnosti pro všechny velikosti webovou aplikaci. Využívejte existující nástroje k vytváření a nasazování aplikací bez starostí o správu infrastruktury.
> 
> Poskytnout produkční webové aplikace se v minutách snadno nasazení obsahu, které jsou vytvořené pomocí Oblíbené vývojový nástroj. Můžete nasadit přímo od správy zdrojového kódu se podpora pro existující lokalitu **Git**, **Githubu**, **Bitbucket**, **TFS**a to i v  **DropBox**. Nasazení přímo z vašeho oblíbeného rozhraní IDE nebo skripty s použitím **prostředí PowerShell** v systému Windows nebo **rozhraní příkazového řádku** nástroje systémem žádné operačního systému. Po nasazení zachovat vaše lokality s podporou pro průběžné nasazování neustále aktuální.
> 
> Azure poskytuje škálovatelné, odolné cloudového úložiště, zálohování a obnovení řešení pro všechna data, velké nebo malé. Při nasazování aplikací pro produkční prostředí, služby úložiště, jako jsou tabulky, objekty BLOB a databází SQL, můžete škálovat aplikaci v cloudu.
> 
> U databází SQL je důležité k zachování aktualizovaného stavu databáze produktivitu při nasazování nové verze aplikace. Poděkování **migrace Entity Framework Code First**, vývoj a nasazení datový model je jednodušší aktualizovat vaše prostředí v minutách. Toto praktické cvičení se zobrazí v různých tématech, ke kterým může dojít při nasazování webové aplikace do produkčního prostředí v Microsoft Azure.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).
> 
> Další podrobné pokrytí tohoto tématu, najdete v článku [vytváření reálných cloudových aplikací pomocí Azure elektronická kniha](building-real-world-cloud-apps-with-windows-azure/introduction.md).


<a id="Overview"></a>
## <a name="overview"></a>Přehled

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí praktických se dozvíte, jak:

- Povolit Entity Framework migrace pomocí existujícího modelu
- Aktualizace modelu objektu a odpovídajícím způsobem používat Entity Framework migrace databáze
- Nasazení do Azure App Service pomocí Git
- Vrácení zpět na předchozí nasazení pomocí portálu pro správu Azure
- Škálování webové aplikace pomocí Azure Storage
- Konfigurace automatického škálování pro webovou aplikaci pomocí portálu pro správu Azure
- Vytvoření a konfigurace testovacího projektu zatížení v sadě Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Pro dokončení této praktické cvičení je vyžadován následující text:

- [Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/) nebo vyšší
- [Azure SDK pro .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [Systém správy verzí GIT](http://git-scm.com/download)
- Předplatné Microsoft Azure 

    - Zaregistrujte si [bezplatné zkušební verze](http://aka.ms/watk-freetrial)
    - Pokud jste Visual Studio Professional, Test Professional, Premium nebo Ultimate s předplatitele MSDN nebo MSDN platformy, aktivovat vaší [výhody webu MSDN](http://aka.ms/watk-msdn) nyní zahájíte vývoj a testování v Azure
    - [BizSpark](http://aka.ms/watk-bizspark) členové automaticky obdrží Azure benefit prostřednictvím jejich Visual Studio Ultimate s předplatné MSDN
    - Členové [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials na program přijímat měsíční kredity Azure zdarma.

<a id="Setup"></a>
### <a name="setup"></a>Instalace

Aby bylo možné spustit praktická cvičení v tomto testovacím prostředí praktické, musíte nejdřív nastavit svoje prostředí.

1. Otevřete Průzkumníka Windows a přejděte do testovacího prostředí na **zdroj** složky.
2. Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a instalaci sady Visual Studio fragmenty kódu pro toto testovací prostředí.
3. Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, která bude pokračovat.

> [!NOTE]
> Zkontrolujte, zda že jste zkontrolovali všechny závislosti pro toto testovací prostředí před spuštěním instalačního programu.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Používání fragmentů kódu

V dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu. Pro usnadnění vaší práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, kterým můžete přistupovat z v rámci Visual Studio 2013, abyste nemuseli přidat ručně.

> [!NOTE]
> Každý úkol je přiložena počáteční řešení umístěný v **začít** složky cvičení, která umožňuje postupujte podle jednotlivých cvičení nezávisle na ostatních. Upozorňujeme, že fragmenty kódu, které jsou přidány během cvičení chybí z nich spuštění řešení a nemusí fungovat, dokud nedokončíte výkonu. Uvnitř zdrojový kód pro cvičení, je také k dispozici **End** složku obsahující řešení sady Visual Studio s kódem, který je výsledkem kroků v odpovídající cvičení. Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické cvičení.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické cvičení zahrnuje následující cvičení:

1. [Pomocí migrace Entity Framework](#Exercise1)
2. [Nasazení webové aplikace do pracovní](#Exercise2)
3. [Provedení vrácení nasazení v produkčním prostředí](#Exercise3)
4. [Škálování používat úložiště Azure](#Exercise4)
5. [Použití automatického škálování pro webové aplikace](#Exercise5) (volitelné pro Visual Studio 2013 Ultimate edition)

Odhadovaný čas dokončení tohoto testovacího prostředí: **75 minut**

> [!NOTE]
> Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce. Každé předdefinované kolekce je navržené tak, aby odpovídala konkrétním vývoj styl a určuje rozložení oken, editor chování, IntelliSense – fragmenty kódu a možnosti dialogového okna. Postupy v tomto testovacím prostředí popisovat akce, které jsou nutné k dokončení daného úkolu v sadě Visual Studio, při použití **obecné nastavení vývoj** kolekce. Pokud si zvolíte jiné nastavení kolekce pro vývojové prostředí, může být rozdíly v krocích, které byste měli vzít v úvahu.


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Cvičení 1: Pomocí migrace Entity Framework

Při vývoji aplikace, můžou časem změnit datový model. Tyto změny mohou ovlivnit existující model v databázi (Pokud vytváříte novou verzi) a je důležité zajistit, aby vaše databáze aktuální zabráníte tak chybám.

Pro zjednodušení sledování tyto změny v modelu, **migrace Entity Framework Code First** automaticky zjišťuje změny porovnání modelu pomocí schématu databáze a generuje konkrétní kód aktualizovat vaši databázi vytváření nových *verze* vaší databáze.

Toto cvičení se dozvíte, jak povolit **migrace** pro vaše aplikace a jak můžete snadno zjistit a generovat změny k aktualizaci databáze.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Úloha 1 – povolení migrace

V této úloze bude projít kroky povolení **migrace Entity Framework Code First** k **Geek kvízu** databáze, změna modelu a pochopení, jak tyto změny se projeví v databáze.

1. Otevřete Visual Studio a otevřete **GeekQuiz.sln** soubor řešení z **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.
2. Sestavte řešení Chcete-li stáhnout a nainstalovat **NuGet** balíček závislosti. Chcete-li to provést, klikněte pravým tlačítkem na řešení a klikněte na **sestavit řešení** nebo stiskněte klávesu **Ctrl + Shift + B**.
3. Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**a potom klikněte na **Konzola správce balíčků**.
4. V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**. Vytvoří se při počáteční migraci na základě existující modelu.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Povolení migrace](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "povolení migrace")

    *Povolení migrace*

    > [!NOTE]
    > Tento příkaz přidá **migrace** složky pro Geek kvízu projekt, který obsahuje soubor s názvem **Configuration.cs**. **Konfigurace** třída umožňuje konfigurovat chování migrace pro váš kontext.
5. Pomocí migrace povolena, je potřeba aktualizovat **konfigurace** třídu k naplnění databáze počáteční data, **Geek kvízu** vyžaduje. V části **migrace**, nahraďte **Configuration.cs** soubor se nachází v **Source\Assets** složky tohoto testovacího prostředí.

    > [!NOTE]
    > Vzhledem k tomu **migrace** zavolá **počáteční hodnoty** metoda při každé aktualizaci databáze, je potřeba mít jistotu, že nejsou duplicitní záznamy v databázi. **AddOrUpdate** metoda pomáhají zabránit duplicitní data.
6. Chcete-li přidat počáteční migrace, zadejte následující příkaz a stiskněte klávesu **Enter**.

    > [!NOTE]
    > Ujistěte se, že neexistuje žádná databáze s názvem &quot;GeekQuizProd&quot; ve vaší instanci LocalDB.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Přidání základní schématu migrace](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "přidání základní schématu migrace")

    *Přidání základní schématu migrace*

    > [!NOTE]
    > **Přidat migrace** bude vygenerovat další migraci na základě změn, které jste udělali modelu od vytvoření posledního migrace. V takovém případě je to první migrace projektu, přidá skripty k vytvoření všechny tabulky definované v **TriviaContext** třídy.
7. Spusťte migrace k aktualizaci databáze spuštěním následujícího příkazu. Pro tento příkaz zadejte **podrobné** zobrazíte příkazy SQL aplikované na cílovou databázi.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Vytváření počáteční databáze](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "vytváření počáteční databáze")

    *Vytvoření počáteční databáze*

    > [!NOTE]
    > **Update-Database** použije všechny nevyřízené migrace do databáze. V takovém případě vytvoří databázi pomocí připojovacího řetězce definované v souboru web.config.
8. Přejděte na **zobrazení** nabídky a otevřete **Průzkumník objektů systému SQL Server**.

    ![Otevřít v Průzkumníku objektů systému SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "otevřít v Průzkumníku objektů systému SQL Server")

    *Otevřít v Průzkumníku objektů systému SQL Server*
9. V **Průzkumník objektů systému SQL Server** okně připojení k vaší instanci LocalDB kliknutím pravým tlačítkem myši **systému SQL Server** uzlu a výběrem **přidat SQL Server...**  možnost.

    ![Přidání Instance systému SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "přidání Instance systému SQL Server")

    *Přidání instance systému SQL Server do Průzkumník objektů systému SQL Server*
10. Nastavte **název serveru** k *(localdb) \v11.0* a nechte **ověřování systému Windows** jako vaše režim ověřování. Klikněte na tlačítko **Connect** pokračujte.

    ![Připojení k LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "připojení na instanci LocalDB")

    *Připojení k instanci LocalDB*
11. Otevřete **GeekQuizProd** databáze a rozbalte **tabulky** uzlu. Jak je vidět **Update-Database** příkaz vygeneruje všechny tabulky definované v **TriviaContext** – třída. Vyhledejte **dbo. TriviaQuestions** tabulky a otevřete uzel sloupce. V dalším úkolem přidáte nový sloupec do této tabulky a aktualizovat databázi pomocí **migrace**.

    ![Trivia otázky, které sloupce](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia otázky, které sloupce")

    *Trivia otázky, které sloupce*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Úloha 2 – aktualizace schématu databáze pomocí migrace

V této úloze budete používat **migrace Entity Framework Code First** zjistí změnu v modelu a generovat kód nezbytné k aktualizaci databáze. Aktualizujte **TriviaQuestions** entity přidáním novou vlastnost. Potom budete spouštět příkazy k vytvoření nové migrace zahrnout nového sloupce v tabulce.

1. V **Průzkumníku řešení**, dvakrát klikněte **TriviaQuestion.cs** soubor umístěný uvnitř **modely** složky.
2. Přidat novou vlastnost s názvem **pomocný parametr**, jak ukazuje následující fragment kódu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**. Nové migrace bude vytvořen odrážející změny v našich modelu.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Přidat migrace](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "přidat migrace")

    *Přidat migrace*

    > [!NOTE]
    > Soubor migrace se skládá ze dvou metod **až** a **dolů**.
    > 
    > - **Až** metoda se použije k určení, jaké změny aktuální verzi naše aplikace potřeba použít k databázi.
    > - **Dolů** se používá k změny jsme přidali do **až** metoda.
    > 
    > Při migraci databáze aktualizuje databázi, se aplikace spustí všechny migrace v pořadí časové razítko a pouze ty, které nebyly použity od poslední aktualizace ( \_MigrationHistory tabulky uchovává informace o migrace, které byly použity). **Až** bude volána metoda všechny migrace a provede změny, které jsme určili do databáze. Pokud jsme rozhodne se vrátíte k předchozí migrace **dolů** metoda bude volána k vrátit změny v obráceném pořadí.
4. V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. Výstup **Update-Database** příkaz generované **příkaz Alter Table** příkaz jazyka SQL, které chcete přidat nový sloupec na **TriviaQuestions** tabulky, jak je znázorněno na obrázku níže.

    ![Přidejte příkaz SQL sloupec generované](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "přidejte příkaz SQL sloupec vygenerována")

    *Přidejte příkaz SQL sloupec vygenerována*
6. V **Průzkumník objektů systému SQL Server**, aktualizujte **dbo. TriviaQuestions** tabulky a zkontrolujte, že nové **pomocný parametr** je zobrazen sloupec.

    ![Zobrazení nového sloupce pomocný parametr](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "zobrazující nový sloupec pomocný parametr")

    *Zobrazení nového sloupce pomocný parametr*
7. Zpět v **TriviaQuestion.cs** editoru přidejte **StringLength** omezení, které *pomocný parametr* vlastnost, jak je znázorněno v následující fragment kódu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. Výstup **Update-Database** příkaz generované **příkaz Alter Table** příkazu SQL k aktualizaci *pomocný parametr* typ sloupce pro **TriviaQuestions** tabulky, jak je znázorněno na obrázku níže.

    ![Příkaz ALTER vygenerovat příkaz SQL sloupec](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter vygenerovat příkaz SQL sloupce")

    *Příkaz ALTER vygenerovat příkaz SQL sloupce*
11. V **Průzkumník objektů systému SQL Server**, aktualizujte **dbo. TriviaQuestions** tabulky a zkontrolujte, zda **pomocný parametr** typ sloupce je **nvarchar(150)**.

    ![Zobrazuje nové omezení](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "zobrazující nové omezení")

    *Zobrazuje nové omezení*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Cvičení 2: Nasazení webové aplikace do pracovní

**Webové aplikace v Azure App Service** umožňuje provádět dvoufázové instalace publikování. Dvoufázové instalace publikování vytvoří přípravný slot lokality pro každou lokalitu výchozí produkční a umožňuje Prohodit tyto sloty bez výpadek. To je opravdu užitečné ověřit změny před uvolněním veřejné, postupně integrovat obsah webu a vrátit zpět, jestliže změny nefungují podle očekávání.

V tomto cvičení budete nasazovat **Geek kvízu** aplikace pracovní prostředí vaší webové aplikace pomocí Git zdrojového kódu. K tomuto účelu bude vytvoření webové aplikace a zřídit požadované součásti v portálu pro správu, konfiguraci **Git** úložiště a nabízenými aplikace zdrojový kód z místního počítače pro přípravný slot. Je také aktualizovat vaši produkční databázi s **migrace Code First** jste vytvořili v předchozím cvičení. Aplikace pak provede v tomto prostředí test ověřit jeho funkci. Jakmile budete spokojeni to funguje podle vašim požadavkům, bude podporovat aplikace do produkčního prostředí.

> [!NOTE]
> Chcete-li povolit publikování v dvoufázové instalace, musí být webové aplikace v **standardní režim**. Všimněte si, že další poplatky budou vám být účtovány Pokud změníte vaší webové aplikace do standardního režimu. Další informace o cenách najdete v tématu [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Úloha 1 – Vytvoření webové aplikace v Azure App Service

V této úloze, vytvoříte webovou aplikaci v **Azure App Service** z portálu pro správu. Můžete také nakonfigurovat **SQL Database** zachovat data aplikací a konfigurace místní úložiště Git pro řízení zdrojů.

1. Přejděte na [portál pro správu Azure](https://manage.windowsazure.com) a přihlaste se pomocí účtu Microsoft, které jsou spojené s vaším předplatným.

    ![Přihlaste se k portálu správy Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Přihlaste se k portálu správy Azure*
2. Klikněte na tlačítko **nový** na panelu příkazů v dolní části stránky.

    ![Vytvoření nové webové aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "vytvoření nové webové aplikace")

    *Vytvoření nové webové aplikace*
3. Klikněte na tlačítko **výpočetní**, **webu** a potom **vytvořit vlastní**.

    ![Vytvoření nové webové aplikace pomocí vytvořit vlastní](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "vytvoření nové webové aplikace pomocí vytvořit vlastní")

    *Vytvoření nové webové aplikace pomocí vytvořit vlastní*
4. V **webu nový - vytvořit vlastní** dialogové okno Zadejte dostupný **URL** (například *geek kvízu*), vyberte umístění, do **oblast** rozevírací seznam a vyberte **vytvořit novou databázi SQL** v **databáze** rozevíracího seznamu. Nakonec vyberte **publikování od správy zdrojového kódu** zaškrtávací políčko a klikněte na tlačítko **Další**.

    ![Přizpůsobení nové webové aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Přizpůsobení nové webové aplikace*
5. Zadejte následující informace pro nastavení databáze:

   - V **název** textové pole, zadejte název databáze (například *geekquiz\_db*)
   - Na serveru **rozevíracího seznamu** seznamu, vyberte **nové SQL databázový server**. Alternativně můžete vybrat existující server.
   - V **uživatelské jméno** a **heslo k databázi** polí zadejte uživatelské jméno správce a heslo pro server databáze SQL. Pokud zvolíte možnost server již jste vytvořili, zobrazí se výzva k zadání hesla.

     ![Zadání nastavení databáze](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Zadání nastavení databáze*
6. Klikněte na tlačítko **Další** pokračujte.
7. Vyberte **úložiště místní Git** pro zdrojového kódu použít a klikněte na tlačítko **Další**.

    > [!NOTE]
    > Můžete být vyzváni k nasazení pověření (uživatelské jméno a heslo).

    ![Vytvoření úložiště Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Vytvoření úložiště Git*
8. Počkejte, dokud se vytvoří nové webové aplikace.

    > [!NOTE]
    > Ve výchozím nastavení, Azure poskytuje domén v *azurewebsites.net* , ale také vám dává možnost nastavit vlastní domény pomocí portálu správy Azure. Pokud používáte určité režimy služby Azure App Service však můžete spravovat pouze vlastní domény.
    > 
    > Aplikační služba Azure je k dispozici v edicích Free, sdílené, Basic, Standard a Premium. Všechny webové aplikace v režimu volné a sdílené spustit v prostředí s více klienty a mít kvóty pro využití procesoru, paměti a sítě. Maximální počet bezplatných aplikací se může lišit s plánu. Ve standardním režimu vyberte, které aplikace spustit ve vyhrazených virtuálních počítačů, které odpovídají na standardní Azure výpočetní prostředky. Můžete najít v konfiguraci webové aplikace režimu **škálování** nabídky vaší webové aplikace.
    > 
    > ![Aplikační služba Azure režimy](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "režimy služby Azure App Service")
    > 
    > Pokud používáte **sdílené** nebo **standardní** režimu, bude možné spravovat vlastní domény pro vaši webovou aplikaci tak, že přejdete do vaší aplikace **konfigurace** nabídky a kliknutím na **Správa domén** pod *názvy domén*.
    > 
    > ![Spravovat domény](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "spravovat domény")
    > 
    > ![Spravovat vlastní domény](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "správě vlastních domén")
9. Po vytvoření webové aplikace klikněte na odkaz v části **URL** sloupec zkontrolujte, zda je spuštěna nové webové aplikace.

    ![Procházení do nové webové aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Procházení do nové webové aplikace*

    ![Webová aplikace spuštěná](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *Webová aplikace spuštěná*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Úloha 2 – Vytvoření provozní databáze SQL

V této úloze budete používat **migrace Entity Framework Code First** k vytvoření databáze cílení **Azure SQL Database** instanci jste vytvořili v předchozí úloze.

1. V portálu pro správu, přejděte na webovou aplikaci, kterou jste vytvořili v předchozí úloze a přejděte do jeho **řídicí panel**.
2. V **řídicí panel** klikněte na tlačítko **zobrazit připojovací řetězce** v části **rychlého přehledu** části.

    ![Zobrazit připojovací řetězce](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "zobrazit připojovací řetězce")

    *Zobrazit připojovací řetězce*
3. Kopírování **připojovací řetězec** hodnotu a zavřete dialogové okno.

    ![Připojovací řetězec v portál pro správu Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "připojovací řetězec v portál pro správu Azure")

    *Připojovací řetězec v portál pro správu Azure*
4. Klikněte na tlačítko **databází SQL** zobrazíte seznam databází SQL v Azure

    ![Databáze SQL nabídky](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "nabídky databáze SQL")

    *Databáze SQL nabídky*
5. Vyhledejte databázi, kterou jste vytvořili v předchozí úloze a klepněte na serveru.

    ![Databáze SQL serveru](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "databáze SQL serveru")

    *Databáze SQL serveru*
6. V **rychlý Start** stránka serveru, klikněte na **konfigurace**.

    ![Konfigurace nabídky](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "konfigurovat nabídky")

    *Konfigurace nabídky*
7. V **povolené IP adresy** části, klikněte na **přidat do povolené IP adresy** odkaz na povolit IP pro připojení k databázi SQL serveru.

    ![Povolené IP adresy](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "povolené IP adresy")

    *Povolené IP adresy*
8. Klikněte na tlačítko **Uložit** v dolní části stránky k dokončení kroku.
9. Přepněte zpátky na Visual Studio.
10. V **Konzola správce balíčků**, spustit následující příkaz nahrazení *[YOUR PŘIPOJOVACÍHO ŘETĚZCE]* zástupného symbolu připojovacím řetězcem, který jste zkopírovali z Azure

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Aktualizace databáze cílení na Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "aktualizace databáze cílení na Windows Azure SQL Database")

    *Aktualizace databáze cílení na Azure SQL Database*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Úloha 3 – nasazení kvízu Geek do přípravy pomocí Git

V této úloze povolíte dvoufázové instalace publikování ve vaší webové aplikaci. Poté použije Git pro publikování aplikace Geek kvízu přímo z místního počítače v pracovní prostředí vaší webové aplikace.

1. Přejděte zpět na portál a klikněte na název webové aplikace v rámci **název** sloupec pro zobrazení stránky pro správu.

    ![Otevírání webových stránek správy aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Otevírání webových stránek správy aplikace*
2. Přejděte na **škálování** stránky. V části **Obecné** vyberte **standardní** pro konfiguraci a klikněte na **Uložit** na panelu příkazů.

    > [!NOTE]
    > Lze spustit všechny webové aplikace v aktuální oblasti a předplatné v **standardní** režimu, ponechejte **Vybrat vše** v zaškrtnuto políčko **vyberte lokality** konfigurace. Jinak, zrušte **Vybrat vše** zaškrtávací políčko.

    ![Upgrade webové aplikace do standardního režimu](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "upgrade webové aplikace do standardního režimu")

    *Upgrade webové aplikace do standardního režimu*
3. Klikněte na tlačítko **Ano** potvrďte změny.

    ![Potvrzení změn do standardního režimu](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "pokračováním Změna režimu webové aplikace")

    *Potvrzení změn do standardního režimu*
4. Přejděte na **řídicí panel** a klikněte na tlačítko **povolit dvoufázové instalace publikování** pod **rychlého přehledu** části.

    ![Povolení publikování dvoufázové instalace](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "povolení připravený publikování")

    *Povolení publikování dvoufázové instalace*
5. Klikněte na tlačítko **Ano** povolit dvoufázové instalace publikování.

    ![Potvrzení připravený publikování](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "kliknutím na tlačítko Ano, chcete-li povolit dvoufázové instalace publikování")

    *Potvrzení publikování dvoufázové instalace*
6. V seznamu webové aplikace rozbalte značku nalevo od název vaší webové aplikace zobrazíte přípravný slot lokality. Obsahuje název vaší webové aplikace, za nímž následuje ***(pracovní)***. Klepněte na pracovní síť, přejděte na stránku správy.

    ![Navigace na pracovní webovou aplikaci](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "přejdete na pracovní webové aplikace")

    *Přejdete na pracovní aplikace*
7. Všimněte si, že této stránce správy he vypadá stránce management jakékoli jiné webové aplikace. Přejděte na **nasazení** stránky a zkopírujte **adresy URL pro Git** hodnotu. Použije ho později v tomto cvičení.

    ![Kopírování hodnota adresy URL pro Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Kopírování hodnota adresy URL pro Git*
8. Otevřete nový **Git Bash** konzolu a spusťte následující příkazy. Aktualizace *[YOUR cesta aplikace]* zástupný symbol cestu k **GeekQuiz** řešení, které jsou umístěné v **Source\Ex1 DeployingWebSiteToStaging\Begin** složky Toto testovací prostředí.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inicializace Git a první potvrzení změn](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Inicializace Git a první potvrzení změn*
9. Spusťte následující příkaz pro uložení vaší webové aplikace do vzdálených **Git** úložiště. Nahraďte zástupný symbol s adresou URL, které jste získali z portálu pro správu. Zobrazí se výzva k zadání hesla nasazení.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Když zavedete do služby Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Vkládání do Azure*

    > [!NOTE]
    > Při nasazení obsahu FTP hostitele nebo úložiště GIT webové aplikace, je třeba ověřit, pomocí **přihlašovací údaje nasazení** vytvořený z webové aplikace **rychlý Start** nebo **řídicí panel**  správu stránek. Pokud si nejste jisti, přihlašovací údaje nasazení je snadno obnovit pomocí portálu pro správu. Otevřete web app **řídicí panel** a klikněte na tlačítko **resetovat přihlašovací údaje nasazení** odkaz. Zadejte nové heslo a klikněte na **OK**. Přihlašovací údaje nasazení jsou platné pro použití s všechny webové aplikace, které jsou spojené s vaším předplatným.
10. Chcete-li ověřit, webové aplikace byla úspěšně nabídnutých do Azure, přejděte zpět na portálu pro správu a klikněte na tlačítko **weby**.
11. Vyhledejte vaší webové aplikace a rozbalte položku zobrazíte přípravný slot lokality. Klikněte na jeho **název** přejděte na stránku správy.
12. Klikněte na tlačítko **nasazení** zobrazíte **historii nasazení**. Ověřte, zda je **aktivní nasazení** s vaší  *&quot;počáteční potvrdit&quot;*.

    ![Aktivní nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Aktivní nasazení*
13. Nakonec klikněte na **Procházet** na panelu příkazů na Přejít do webové aplikace.

    ![Procházet webové aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Procházet webové aplikace*
14. Pokud je aplikace nasazena úspěšně, zobrazí se stránka Geek kvízu přihlášení.

    > [!NOTE]
    > Adresa URL nasazené aplikace obsahuje název vaší webové aplikace, za nímž následuje *-pracovní*.

    ![Aplikace běžící v testovacím prostředí](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Aplikace běžící v testovacím prostředí*
15. Pokud chcete prozkoumat aplikace, klikněte na tlačítko **zaregistrovat** k registraci nového uživatele. Podrobnosti o účtu dokončete tak, že zadáte uživatelské jméno, e-mailovou adresu a heslo. V dalším kroku aplikaci ukazuje na první otázku kvízu. Odpovězte na několik otázek a ujistěte se, že funguje podle očekávání.

    ![Aplikace připravené k použití](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Aplikace připravené k použití*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Úloha 4 – povýšení webové aplikace do produkčního prostředí

Teď, když si ověříte, že webová aplikace funguje správně v testovacím prostředí, budete chtít přesunout do výroby. V této úloze bude Prohodit přípravný slot lokality s lokality produkční slot.

1. Přejděte zpět na portálu pro správu a vyberte přípravný slot lokality. Klikněte na tlačítko **odkládacího souboru** na panelu příkazů.

    ![Do ostrého provozu](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Do ostrého provozu*
2. Klikněte na tlačítko **Ano** v potvrzovacím dialogovém okně, chcete-li pokračovat v provádění této operace prohození. Azure bude okamžitě odkládacího souboru obsahu pracoviště s obsahem webu pracovní.

    > [!NOTE]
    > Některá nastavení z dvoufázové instalace verze se automaticky zkopírují na produkční verzi (například připojovací řetězec přepsání, mapování obslužných rutin, atd.), ale ostatní nastavení nedojde ke změně (např. DNS koncových bodů, vazby SSL, atd.).

    ![Potvrzení operace prohození](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Potvrzení operace prohození*
3. Po dokončení prohození vyberte produkční slot a klikněte na **Procházet** na panelu příkazů otevřete pracoviště. Všimněte si adresu URL na panelu Adresa.

    > [!NOTE]
    > Možná budete muset aktualizujte webový prohlížeč a vymažte mezipaměť. V aplikaci Internet Explorer, můžete k tomu stisknutím **CTRL + R**.

    ![Webová aplikace spuštěná v produkčním prostředí](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. V **Git Bash** konzole, aktualizovat vzdálenou adresu URL pro místní úložiště Git pro produkční slot. To provedete pomocí následujícího příkazu nahraďte zástupné symboly vaše nasazení uživatelské jméno a název webové aplikace.

    > [!NOTE]
    > V následujícím cvičení bude doručte změny do pracoviště místo pracovní jenom pro jednoduchost testovací prostředí. Ve scénáři reálného doporučujeme ověřit změny v testovacím prostředí před zvýšením úrovně do produkčního prostředí.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Cvičení 3: Provedení vrácení nasazení v produkčním prostředí

Existují scénáře, kde jste přípravný slot na Zaměnit aktivní přípravné nebo produkční prostředí, například při práci s **volné** nebo **sdílené** režimu. V těchto scénářích měli byste otestovat svoji aplikaci v testovacím prostředí – místně nebo ve vzdálené lokalitě – před nasazením do produkčního prostředí. Je však možné, že problém nebyl zjištěn během fáze testování mohou nastat v produkční lokality. V takovém případě je důležité mít mechanismus snadno přepnout na předchozí a další stabilní verzi aplikace co nejrychleji.

V **Azure App Service**, průběžné nasazování od správy zdrojového kódu umožňuje toto možné díky **znovu nasaďte** akce, které jsou k dispozici v portálu pro správu. Azure uchovává informace o nasazení přiřazená k potvrzení nabídnutých do úložiště a poskytuje možnost znovu nasaďte aplikaci pomocí některé z předchozích nasazení, kdykoli.

V tomto cvičení budete provádět změny kódu v **Geek kvízu** aplikace, která záměrně vloží *chyb*. Nasadíte aplikace do produkčního prostředí, abyste viděli chybu a pak bude využívat funkci znovu ho zaveďte se vrátíte do předchozího stavu.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Úloha 1 – aktualizace aplikace Geek kvízu s časovým limitem

V této úloze bude Refaktorovat malou část kódu **TriviaController** třídy extrahovat část logiky, která načte možnost vybrané kvízu z databáze do nové metody.

1. Přepnout na instanci aplikace Visual Studio s **GeekQuiz** řešení z předchozím cvičení.
2. V **Průzkumníku řešení**, otevřete **TriviaController.cs** souboru uvnitř **řadiče** složky.
3. Vyhledejte **StoreAsync** metoda a vyberte zvýrazněnou kód na následujícím obrázku.

    ![Výběr kód](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Výběr kód*
4. Klikněte pravým tlačítkem na vybraný úsek kódu, rozbalte **Refaktorovat** nabídku a vyberte **extrahovat metodu...** .

    ![Extrahování kód jako nové metody](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Výběr metody extrakce*
5. V **extrahovat metodu** dialogové okno, název nové metody *MatchesOption* a klikněte na tlačítko **OK**.

    ![Zadání názvu – metoda](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Zadejte název pro metodu extrahované*
6. Vybraný úsek kódu je pak extrahován do **MatchesOption** metoda. Následující fragment kódu ukazuje výsledný kód.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Stiskněte klávesu **kombinaci kláves CTRL + S** a uložte změny.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Úloha 2 – opětovného nasazení aplikace Geek kvízu s časovým limitem

Nyní jste předá změny, které jste provedli v předchozí úloze úložiště, které aktivují nového nasazení do produkčního prostředí. Potom jste se potíže, k problém pomocí **nástroje pro vývoj F12** poskytované Internet Explorer a potom proveďte vrácení zpět na předchozí nasazení z portálu správy Azure.

1. Otevřete nový **Git Bash** konzolu k nasazení aktualizované aplikace do služby Azure App Service.
2. Spuštěním následujících příkazů pro uložení změn do Azure. Aktualizace *[YOUR cesta aplikace]* zástupný symbol cestu k **GeekQuiz** řešení. Zobrazí se výzva k zadání hesla nasazení.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Vkládání kódu rozdělili do Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Vkládání kódu rozdělili do Azure*
3. Otevřete Internet Explorer a přejděte na webovou aplikaci (například `http://<your-web-site>.azurewebsites.net`). Přihlaste se pomocí dříve vytvořeného přihlašovacích údajů.
4. Stiskněte klávesu **F12** ke spuštění nástroje pro vývoj, vyberte **sítě** a klikněte **přehrání** tlačítko spusťte záznam.

    ![Spouštění sítě záznam](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "od sítě záznam")

    *Výchozí záznam sítě*
5. Vyberte všechny možnost kvízu. Zobrazí se, že se nic nestane.
6. V **F12** okně zobrazuje položku odpovídající požadavek POST HTTP protokolu HTTP **500** výsledek.

    ![HTTP 500 Chyba](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 error*
7. Vyberte **konzoly** kartě. Podrobnosti o příčině je zaznamenána chyba.

    ![Chyba protokolu](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Chyba protokolu*
8. Vyhledejte část Podrobnosti o chybě. Je zřejmé tato chyba je způsobená kódu refaktoringu je potvrzené v předchozích krocích.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. Nezavírejte okno prohlížeče.
10. V nové instanci prohlížeče, přejděte na [portál pro správu Azure](https://manage.windowsazure.com) a přihlaste se pomocí účtu Microsoft, které jsou spojené s vaším předplatným.
11. Vyberte **weby** a klikněte na webovou aplikaci, které jste vytvořili v cvičení 2.
12. Přejděte na **nasazení** stránky. Všimněte si, že všechny potvrzení provést jsou uvedeny v historii nasazení.

    ![Seznam existující nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Seznam existující nasazení*
13. Vyberte předchozí potvrzení a klikněte na **znovu nasaďte** na panelu příkazů.

    ![Opětovné nasazení předchozí potvrzení](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Opětovné nasazení předchozí potvrzení*
14. Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano**.

    ![Potvrzení opakované nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Po dokončení nasazení přepnout zpět do instance prohlížeče s webovou aplikaci a stiskněte klávesu **kombinaci kláves CTRL + F5**.
16. Klikněte na některou z možností. Flip animace bude trvat teď místní a výsledek (*opravte nesprávné*) se zobrazí.
17. (Volitelné) Přepnout **Git Bash** konzolu a spusťte následující příkazy se vrátit k předchozí potvrzení.

    > [!NOTE]
    > Tyto příkazy vytvořit nové potvrzení změn, které vrátí zpět všechny změny v úložišti Git, které byly provedeny v chybný potvrzení. Azure bude poté znovu nasaďte aplikace pomocí nového potvrzení.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Cvičení 4: Škálování používat úložiště Azure

**Objekty BLOB** jsou nejjednodušší způsob, jak ukládat velké objemy nestrukturovaných textu nebo binárních dat, jako je například video, zvuk a obrázků. Přesunutí statický obsah aplikace do úložiště, pomáhá škálování aplikace pomocí poskytování obrázků nebo dokumentů přímo do prohlížeče.

V tomto cvičení se přesune statický obsah vaší aplikace na kontejner objektů Blob. Pak budete konfigurovat aplikace pro přidání **adresy URL technologie ASP.NET, přepište pravidlo** v **Web.config** přesměrovat obsah do kontejneru objektů Blob.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Úloha 1 – Vytvoření účtu úložiště Azure

V této úloze se dozvíte, jak vytvořit nový účet úložiště pomocí portálu pro správu.

1. Přejděte na [portál pro správu Azure](https://manage.windowsazure.com) a přihlaste se pomocí účtu Microsoft, které jsou spojené s vaším předplatným.
2. Vyberte **nové | Datové služby | Úložiště | Funkce rychle vytvořit** chcete začít vytvářet nový účet úložiště. Zadejte jedinečný název pro účet a vyberte **oblast** ze seznamu. Klikněte na tlačítko **vytvořit účet úložiště** pokračujte.

    ![Vytvoření nového účtu úložiště](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "vytvoření nového účtu úložiště")

    *Vytvoření nového účtu úložiště*
3. V **úložiště** část, počkejte, dokud stav nového účtu úložiště se změní na *Online* než budete pokračovat v následujícím kroku.

    ![Vytvořit účet úložiště](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "vytvořený účet úložiště")

    *Vytvořit účet úložiště*
4. Klikněte na název účtu úložiště a pak klikněte na **řídicí panel** odkaz v horní části stránky. **Řídicí panel** stránka poskytuje informace o stavu účet a koncové body služby, které lze použít v rámci vaší aplikace.

    ![Zobrazení řídicího panelu účet úložiště](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "zobrazení řídicího panelu účtu úložiště")

    *Zobrazení řídicího panelu účtu úložiště*
5. Klikněte **spravovat přístupové klíče** tlačítka na navigačním panelu.

    ![Správa přístupových klíčů tlačítko](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "tlačítko Spravovat přístupové klíče")

    *Správa přístupových klíčů tlačítko*
6. V **spravovat přístupové klíče** dialogové okno, kopie **název účtu úložiště** a **primární přístupový klíč** jako v následujícím cvičení budete potřebovat. Zavřete dialogové okno.

    ![Dialogové okno Spravovat přístupový klíč](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "dialogové okno Spravovat přístupový klíč")

    *Přístupový klíč dialogové okno Spravovat*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Úloha 2 – nahrání prostředek do Azure Blob Storage

V této úloze budete používat v okně Průzkumníka serveru Visual Studio pro připojení k účtu úložiště. Potom vytvořte kontejner objektů blob a nahrát soubor s logem kvízu Geek do kontejneru.

1. Přepnout na instanci aplikace Visual Studio s **GeekQuiz** řešení z předchozím cvičení.
2. V řádku nabídek vyberte **zobrazení** a pak klikněte na **Průzkumníka serveru**.
3. V **Průzkumníka serveru**, klikněte pravým tlačítkem myši **Azure** uzel a vyberte možnost **připojit k Azure...** . Přihlaste se pomocí účtu Microsoft, které jsou spojené s vaším předplatným.

    ![Připojení k systému Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Připojit k Azure*
4. Rozbalte **Azure** uzel, klikněte pravým tlačítkem na **úložiště** a vyberte **připojit externí úložiště...** .
5. V **přidat nový účet úložiště** dialogovém okně zadejte **název účtu** a **klíč účtu** jste získali v předchozí úloze a klikněte na tlačítko **OK**.

    ![Přidat dialogové okno Nový účet úložiště](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Přidat dialogové okno Nový účet úložiště*
6. V dialogovém okně účtu úložiště **úložiště** uzlu. Rozbalte účtu úložiště, klikněte pravým tlačítkem na **objekty BLOB** a vyberte **vytvořit kontejner objektů Blob...** .

    ![Vytvoření kontejneru objektů Blob](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "vytvořit kontejner objektů Blob")

    *Vytvoření kontejneru objektů Blob*
7. V **vytvořit kontejner objektů Blob** dialogovém okně zadejte název pro kontejner objektů blob a klikněte na tlačítko **OK**.

    ![Dialogové okno vytvořit kontejner objektů Blob](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "dialogové okno vytvořit kontejner objektů Blob")

    *Kontejner objektů Blob dialogové okno vytvořit*
8. Nový kontejner objektu blob musí být přidaní do **objekty BLOB** uzlu. Změňte přístupová oprávnění v kontejneru zveřejnit kontejneru. Chcete-li to provést, klikněte pravým tlačítkem **bitové kopie** kontejneru a vyberte **vlastnosti**.

    ![vlastnosti kontejneru image](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "Image vlastnosti kontejneru")

    *Vlastnosti kontejneru bitové kopie*
9. V **vlastnosti** nastavte **veřejný přístup pro čtení** k **kontejneru**.

    ![Změna vlastnosti veřejný přístup pro čtení](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Změna vlastnosti veřejný přístup pro čtení")

    *Změna vlastnosti veřejný přístup pro čtení*
10. Po zobrazení výzvy, pokud jste si jistí, kterou chcete změnit vlastnost veřejný přístup, klikněte na **Ano**.

    ![Microsoft Visual Studio upozornění](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "upozornění Microsoft Visual Studio")

    *Microsoft Visual Studio upozornění*
11. V **Průzkumníka serveru**, klikněte pravým tlačítkem myši **bitové kopie** kontejner objektů blob a vyberte **kontejner objektů Blob zobrazení**.

    ![Zobrazit kontejner objektů Blob](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "zobrazení kontejneru objektů Blob")

    *Kontejner objektů Blob zobrazení*
12. Kontejner imagí by měla otevřít v novém okně a by se měly zobrazit Legenda s žádné položky. Klikněte **nahrát** ikonu pro nahrání souboru do kontejneru objektů blob.

    ![Kontejner imagí s žádné položky](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "kontejner Imagí s žádné položky")

    *Kontejner imagí s žádné položky*
13. V **nahrát objekt Blob** dialogové okno pole, přejděte **prostředky** složky prostředí. Vyberte **logo big.png** souboru a klikněte na tlačítko **otevřete**.
14. Počkejte, až se soubor odešle. Po dokončení nahrávání souboru by měl být uvedený v kontejneru bitové kopie. Pravým tlačítkem na položku soubor a vyberte **kopie URL**.

    ![Zkopírujte adresu URL objektů blob](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "zkopírujte adresu URL souboru objektů blob")

    *Zkopírujte adresu URL objektů blob*
15. Otevřete Internet Explorer a vložte adresu URL. Na následujícím obrázku se mají v prohlížeči.

    ![Obrázek loga big.png z úložiště objektů Blob Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo big.png bitovou kopii z úložiště")

    *Obrázek loga big.png z Azure Blob Storage*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Úloha 3 – aktualizace řešení využívají statický obsah z Azure Blob Storage

V této úloze nakonfigurujete **GeekQuiz** řešení využívat image nahrané do Azure Blob Storage (namísto bitovou kopii umístěné ve webové aplikaci) tak, že přidáte pravidlo přepsání adresy URL technologie ASP.NET v **web.config**souboru.

1. V sadě Visual Studio, otevřete **Web.config** souboru uvnitř **GeekQuiz** projektu a najděte **&lt;system.webServer&gt;** element.
2. Přidejte následující kód do přidat adresu URL přepisování pravidlo, aktualizace zástupného textu s názvem svého účtu úložiště.

    (Code fragment kódu - *WebSitesInProduction - Ex4 - UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > Přepisování adres URL je proces zachycení příchozí webové žádosti a požadavek přesměrování na jiný prostředek. Přepisování pravidel adres URL informuje třeba přepisovat modul, když potřebuje žádost o přesměrování a kde by měl být přesměrován. Třeba přepisovat pravidlo se skládá ze dvou řetězců: vzor hledání v požadovanou adresu URL (obvykle s použitím regulárních výrazů), a řetězec, který má nahradit vzor, pokud nalezen. Další informace najdete v tématu [přepisování adres URL technologie ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).
3. Stiskněte klávesu **kombinaci kláves CTRL + S** a uložte změny.
4. Otevřete nový **Git Bash** konzolu k nasazení aktualizované aplikace do služby Azure App Service.
5. Spuštěním následujících příkazů pro uložení změn do Azure. Aktualizace *[YOUR cesta aplikace]* zástupný symbol cestu k **GeekQuiz** řešení. Zobrazí se výzva k zadání hesla nasazení.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Nasazení aktualizací do Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Nasazení aktualizací do Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Úloha 4 – ověření

V této úloze budete používat **Internet Explorer** a přejděte **Geek kvízu** aplikaci a zkontrolujte, že adresa URL přepsat pravidlo pro funguje bitové kopie a zprávy jsou přesměrovány na bitovou kopii hostované na **objektů Blob v Azure Úložiště**.

1. Otevřete Internet Explorer a přejděte na webovou aplikaci (například `http://<your-web-site>.azurewebsites.net`). Přihlaste se pomocí dříve vytvořeného přihlašovacích údajů.

    ![Zobrazuje webovou aplikaci Geek kvízu s bitovou kopii](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "zobrazující webovou aplikaci Geek kvízu s bitovou kopii")

    *Zobrazuje webovou aplikaci Geek kvízu s obrázkem*
2. Stiskněte klávesu **F12** ke spuštění nástroje pro vývoj, vyberte **sítě** kartě a spusťte záznam.

    ![Spouštění sítě záznam](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "od sítě záznam")

    *Výchozí záznam sítě*
3. Stiskněte klávesu **kombinaci kláves CTRL + F5** aktualizovat webovou stránku.
4. Po dokončení načítání stránky, měli byste vidět požadavku HTTP pro **/img/logo-big.png** adresu URL pomocí protokolu HTTP **301** výsledek (přesměrování) a jiným požadavkem na `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` adresu URL s HTTP **200** výsledek.

    ![Ověřování adresy URL přesměrování](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "zobrazující přesměrování v nástrojích pro vývojáře")

    *Ověřování adresy URL přesměrování*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Cvičení 5: Pomocí automatického škálování pro webové aplikace

> [!NOTE]
> Tento postup je volitelná, protože vyžaduje podporu pro zatížení webové &amp; testování výkonu, která je dostupná jenom pro **Visual Studio 2013 Ultimate Edition**. Další informace o konkrétní funkce sady Visual Studio 2013, porovnat verze [zde](https://www.microsoft.com/visualstudio/eng/products/compare).


**Azure App Service Web Apps** poskytuje funkci automatického škálování pro webové aplikace běžící **standardní režim**. Škálování umožňuje Azure automaticky škálovat počet instancí vaší webové aplikace v závislosti na zatížení. Pokud je povoleno automatické škálování, Azure kontroluje procesoru vaší webové aplikace každých pět minut a přidá instancí podle potřeby v tomto bodě v čase. Pokud je nízké využití procesoru, Azure instance budou odstraněny, každé dvě hodiny zajistit, že není ke snížení výkonu webové aplikace.

V tomto cvičení jste projít kroky nutné ke konfiguraci **škálování** funkcí pro **Geek kvízu** webové aplikace. Tato funkce ověříte spuštěním zátěžového testu sady Visual Studio k vygenerování dostatek zatížení procesoru v aplikaci k aktivaci instance upgrade.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Úloha 1 – konfigurace automatického škálování podle metriky využití procesoru

V této úloze budete používat portál pro správu Azure k povolení této funkce automatického škálování pro webovou aplikaci, kterou jste vytvořili v cvičení 2.

1. V [portál pro správu Azure](https://manage.windowsazure.com/), vyberte **weby** a klikněte na webovou aplikaci, které jste vytvořili v cvičení 2.
2. Přejděte na **škálování** stránky. V části **kapacity** vyberte **procesoru** pro **škálování podle metriky** konfigurace.

    > [!NOTE]
    > Při zvětšení velikosti podle využití procesoru, Azure dynamicky upraví počet instancí, které aplikace používá, pokud se změní využití procesoru.

    ![Výběr škálování podle využití procesoru](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "výběrem metriky využití procesoru pro automatické škálování")

    *Výběr škálování podle využití procesoru*
3. Změna **cílový procesor** konfigurace **20**-**40** procent.

    > [!NOTE]
    > Tento rozsah představuje průměrné využití procesoru pro vaši webovou aplikaci. Azure přidá nebo odebere instance, které chcete zachovat vaší webové aplikace v tomto rozsahu. Minimální a maximální počet instancí použitý pro škálování je zadána v **počet instancí** konfigurace. Azure nikdy přejde nad nebo překračuje tento limit.
    > 
    > Výchozí hodnota **cílový procesor** hodnoty jsou změnit jenom pro účely tohoto testovacího prostředí. Konfigurace rozsahu procesoru malé hodnoty, jsou zvýšit pravděpodobnost k aktivační události škálování střední zatížení při umístění na aplikaci.

    ![Změna cílové procesoru být až 20, 40 procent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "změna cíl procesoru, který má být až 20, 40 procent")

    *Změna cílové procesoru být až 20, 40 procent*
4. Klikněte na tlačítko **Uložit** na panelu příkazů a uložte změny.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Úloha 2 – zatížení testování v sadě Visual Studio

Teď, když **škálování** byl nakonfigurován, vytvoříte **výkonu webu a spouštění testovacího projektu** v sadě Visual Studio k vygenerování některých zatížení procesoru na vaší webové aplikace.

1. Otevřete **Visual Studio Ultimate 2013** a vyberte **souboru | Nové | Projekt...**  zahájíte nové řešení.

    ![Vytvoření nového projektu](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "vytvoření nového projektu")

    *Vytvoření nového projektu*
2. V **nový projekt** dialogové okno, vyberte **výkonu webu a zatížení testovacího projektu** pod **Visual C# | Test** kartě. Zajistěte, aby **rozhraní .NET Framework 4.5** je vybrána, název projektu *WebAndLoadTestProject*, vyberte **umístění** a klikněte na tlačítko **OK**.

    ![Vytvoření nového projektu webu a zátěžového testu](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "vytvoření nového projektu webu a zátěžového testu")

    *Vytvoření nového projektu webu a zátěžového testu*
3. V **WebTest1.webtest** klikněte pravým tlačítkem myši **WebTest1** uzel a klikněte na tlačítko **přidat požadavku**.

    ![Žádost o přidání do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "žádost o přidání do WebTest1")

    *Žádost o přidání do WebTest1*
4. V **vlastnosti** okno uzlu novou žádost o aktualizaci **Url** vlastnost tak, aby odkazoval na adresu URL webové aplikace (například *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).

    ![Změna vlastnosti adresy Url](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Změna vlastnosti adresy Url")

    *Změna vlastnosti adresy Url*
5. V **WebTest1.webtest** okna, klikněte pravým tlačítkem na **WebTest1** a klikněte na tlačítko **přidání smyčky...** .

    ![Přidání smyčky do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "přidání smyčky do WebTest1")

    *Přidání smyčky do WebTest1*
6. V **přidat podmíněné pravidlo a položek smyčky** dialogové okno, vyberte **cyklu For** pravidla a upravit následující vlastnosti.

   1. **Hodnota se ukončuje:** 1000
   2. **Název parametru kontextu:** iterátorů
   3. **Hodnota přírůstku:** 1

      ![Vyberte toto pravidlo pro smyčky a aktualizace vlastností](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "vyberte toto pravidlo pro smyčky a aktualizace vlastností")

      *Vyberte toto pravidlo pro smyčky a aktualizace vlastností*
7. V části **položky ve smyčce** vyberte žádosti, které jste předtím vytvořili jako první a poslední položku smyčky. Klikněte na tlačítko **OK** pokračujte.

    ![Výběr položek první a poslední smyčky](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "výběr položek první a poslední smyčky")

    *Výběr položek první a poslední smyčky*
8. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **WebAndLoadTestProject** projektu, rozbalte **přidat** nabídku a vyberte **zátěžový Test...** .

    ![Přidání zátěžového testu do projektu WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "přidání do projektu WebAndLoadTestProject zátěžového testu")

    *Přidání do projektu WebAndLoadTestProject zátěžového testu*
9. V **nového Průvodce zátěžovým testem** dialogové okno, klikněte na tlačítko **Další**.

    ![Nového Průvodce zátěžovým testem](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "nového Průvodce zátěžovým testem")

    *Nového Průvodce zátěžovým testem*
10. V **scénář** vyberte **nepoužívejte časů přemýšlení** a klikněte na tlačítko **Další**.

    ![Zvolíte nepoužívat časů přemýšlení](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "zvolíte nepoužívat časů přemýšlení")

    *Výběr nepoužívat časů přemýšlení*
11. V **vzor zatížení** stránky, ujistěte se, že **konstantní zatížení** je vybraná možnost. Změna **počet uživatelů** nastavení **250** uživatele a klikněte na tlačítko **Další**.

    ![Změna počtu uživatelů na 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Změna počtu uživatelů na 250")

    *Změna počtu uživatelů na 250*
12. V **Model kombinace testů** vyberte **podle pořadí sekvenční test** a klikněte na tlačítko **Další**.

    ![Výběr model kombinace testů](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "výběr model kombinace testů")

    *Výběr model kombinace testů*
13. V **Model kombinace testů** klikněte na tlačítko **přidat...**  přidat test s různými.

    ![Přidání testu do poměru testů](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "přidávání testu do poměru testů")

    *Přidání testu do poměru testů*
14. V **přidat testy** dialogové okno, dvakrát klikněte na **WebTest1** přidat test **vybrané testy** seznamu. Klikněte na tlačítko **OK** pokračujte.

    ![Přidání WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "přidání WebTest1 testu")

    *Přidání WebTest1 testu*
15. Zpět v **poměru testů** klikněte na tlačítko **Další**.

    ![Dokončení stránce poměru testů](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "dokončení stránce poměru testů")

    *Dokončení stránce poměru testů*
16. V **kombinace sítí** klikněte na tlačítko **Další**.

    ![Kliknutím na další na stránce poměr sítí](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "kliknutím další na stránce poměr sítí")

    *Kliknutím na tlačítko Další na stránce poměr sítí*
17. V **prohlížeče kombinaci** vyberte **Internet Explorer 10.0** jako typ prohlížeče a klikněte na tlačítko **Další**.

    ![Výběr typu prohlížeče](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "výběr typu prohlížeče")

    *Výběr typu prohlížeče*
18. V **sad čítačů** klikněte na tlačítko **Další**.

    ![Kliknutím na tlačítko Další na stránce sad čítačů](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "kliknutím na tlačítko Další na stránce sady čítačů")

    *Kliknutím na tlačítko Další na stránce sady čítačů*
19. V **spustit nastavení** nastavte **doba trvání testu zatížení** k **5 minut** a klikněte na tlačítko **Dokončit**.

    ![Nastavení Doba trvání testu zatížení na 5 minut](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "nastavení Doba trvání testu zatížení na 5 minut")

    *Nastavení Doba trvání testu zatížení na 5 minut*
20. V **Průzkumníku řešení**, dvakrát klikněte **Local.settings** souboru prozkoumat nastavení testu. Ve výchozím nastavení používá Visual Studio místního počítače ke spuštění testů.

    > [!NOTE]
    > Alternativně můžete nakonfigurovat projekt test spouštění zátěžových testů v cloudu pomocí **Visual Studio Online (VSO)**. VSO poskytuje cloudové zátěžové testování služba, která simuluje realističtější zatížení, zabraňující omezení místní prostředí jako kapacity procesoru, dostupné paměti a šířky pásma sítě. Další informace o spouštění zátěžových testů pomocí VSO najdete v tématu [v tomto článku](https://www.visualstudio.com/get-started/load-test-your-app-vs).

    ![Nastavení testu](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Úloha 3 – ověření škálování

Nyní se provést zátěžový test, který jste vytvořili v předchozí úloze a najdete v části chování vaší webové aplikace zatížení.

1. V **Průzkumníku řešení**, dvakrát klikněte na **LoadTest1.loadtest** otevřete zátěžový test.

    ![Otevírání LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "otevírání LoadTest1.loadtest")

    *Opening LoadTest1.loadtest*
2. V **LoadTest1.loadtest** okně klikněte na první tlačítko panelu nástrojů ke spuštění zátěžového testu.

    ![Spuštění zátěžového testu](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "spuštění zátěžového testu")

    *Spuštění zátěžového testu*
3. Počkejte na dokončení zátěžový test.

    > [!NOTE]
    > Zátěžový test simuluje více uživatelů, kteří souběžně odesílat žádosti do webové aplikace. Po spuštění testu, můžete monitorovat dostupné čítače zjistit případné chyby, upozornění a další informace související s vaší zátěžový test, spustit.

    ![Zátěžový test systémem](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "čeká, až do dokončení zátěžového testu")

    *Spuštění zátěžového testu*
4. Po dokončení testu, přejděte zpět na portálu pro správu a přejděte do **škálování** stránku vaší webové aplikace. V části **kapacity** část, měli byste vidět v grafu, byla automaticky nasadit novou instanci.

    ![Nová instance automatické nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Nová instance automatické nasazení*

    > [!NOTE]
    > Může trvat několik minut, než změny zobrazit v grafu (stiskněte **kombinaci kláves CTRL + F5** pravidelně obnovíte stránku). Pokud nevidíte všechny změny, můžete vyzkoušet tyto věci:
    > 
    > - Prodloužit dobu trvání testu zatížení (například k **10 minut**)
    > - Omezit maximální a minimální hodnoty **cílový procesor** rozsah v konfiguraci automatického škálování webové aplikace
    > - Spuštění zátěžového testu v cloudu s **Visual Studio Online**. Další informace [sem](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)

* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

V tomto testovacím prostředí praktických jste zjistili, jak nastavit a nasadit aplikaci na produkční webové aplikace v Azure. Spuštění zjišťování a aktualizace vašich databází pomocí **migrace Entity Framework Code First**, pak dál nasazením nové verze vaší lokality pomocí **Git** a provádění vracení zpět, abyste nejnovější stabilní verze vaší lokality. Kromě toho jste zjistili, jak škálování aplikace pomocí úložiště přesunout statický obsah do kontejneru objektů Blob.
