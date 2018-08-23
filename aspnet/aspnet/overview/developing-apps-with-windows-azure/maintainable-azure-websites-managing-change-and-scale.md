---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Praktické cvičení: udržitelné weby Azure: Správa změn a škálování | Dokumentace Microsoftu'
author: rick-anderson
description: V tomto testovacím prostředí zjistěte, jak Microsoft Azure umožňuje snadno vytvářet a nasazovat weby do produkčního prostředí.
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: a26f22a7cf39593ee068fb8e8d57200120c97ccb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755193"
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Praktické cvičení: udržitelné weby Azure: Správa změn a škálování
====================
podle [Campy Web týmu](https://twitter.com/webcamps)

[Stáhněte si Web Campy školení Kit](http://aka.ms/webcamps-training-kit)

> Microsoft Azure umožňuje snadno vytvářet a nasazovat weby do produkčního prostředí. Ale nebyly provedeny, když vaše aplikace je v provozu, je právě začínáte! Budete potřebovat pro zpracování se měnící požadavky, aktualizace databáze, škálování a další. Naštěstí služby Azure App Service vám kryje záda, spoustou funkcí, které vám pomůže ochránit vaše weby, které běží plynule.
> 
> Azure nabízí bezpečného a flexibilního vývoje, možnosti nasazení a škálování webových aplikací libovolné velikosti. Pomocí svých stávajících nástrojů k vytváření a nasazování aplikací bez starostí o správu infrastruktury.
> 
> Zřiďte provozní webové aplikace si během několika minut a snadno nasadit obsah vytvořený pomocí svůj oblíbený vývojový nástroj. Můžete nasadit existující lokalitu přímo ze správy zdrojových kódů s podporou **Git**, **Githubu**, **Bitbucket**, **TFS**a dokonce i  **DropBox**. Nasazení přímo z vašeho oblíbeného prostředí IDE nebo skripty s využitím **PowerShell** ve Windows nebo **rozhraní příkazového řádku** nástroje spuštěná v jakémkoli operačním systému. Po nasazení, informujte webů neustále o podporu pro průběžné nasazování.
> 
> Azure poskytuje škálovatelné, odolné cloudové úložiště, zálohu a řešení obnovy pro jakákoli data, libovolném objemu. Při nasazování aplikací do produkčního prostředí, služeb úložiště, jako například tabulky, objekty BLOB a databází SQL, můžete škálovat aplikaci v cloudu.
> 
> U databází SQL je potřeba aktualizovat databázi produktivní při nasazování nové verze aplikace. K **migrace Entity Framework Code First**, vývoj a nasazení modelu dat zjednodušili jsme se aktualizovat vaše prostředí během několika minut. Tato praktická cvičení se dozvíte, dalších tématech, které by mohly nastat při nasazení vaší webové aplikace do produkčního prostředí v Microsoft Azure.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).
> 
> Další podrobné pokrytí v tomto tématu najdete v článku [vytváření skutečných cloudových aplikací s Azure e kniha](building-real-world-cloud-apps-with-windows-azure/introduction.md).


<a id="Overview"></a>
## <a name="overview"></a>Přehled

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V této praktická cvičení se dozvíte, jak:

- Povolení migrace rozhraní Entity Framework s existující model
- Aktualizovat objektový model a databáze pomocí migrace rozhraní Entity Framework
- Nasazení do Azure App Service pomocí Gitu
- Vrátit zpět na předchozí nasazení pomocí portálu pro správu Azure
- Škálování webové aplikace pomocí služby Azure Storage
- Konfigurace automatického škálování pro webovou aplikaci pomocí portálu pro správu Azure
- Vytvoření a konfigurace projektu zátěžového testu v sadě Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

K dokončení této praktické testovací prostředí jsou vyžadovány následující položky:

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) nebo vyšší
- [Sada Azure SDK for .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [Systém správy verzí GIT](http://git-scm.com/download)
- Předplatné Microsoft Azure 

    - Zaregistrovat [bezplatnou zkušební verzi](http://aka.ms/watk-freetrial)
    - Pokud jste Visual Studio Professional, Test Professional, Premium nebo Ultimate s MSDN nebo MSDN Platforms odběratele, aktivovat váš [výhodu MSDN](http://aka.ms/watk-msdn) hned a začít s vývojem a testování v Azure
    - [BizSpark](http://aka.ms/watk-bizspark) členové automaticky obdrží Azure benefit prostřednictvím svého Visual Studia Ultimate s předplatným MSDN
    - Členové [programu Microsoft Partner Network](http://aka.ms/watk-mpn) programu Cloud Essentials získáte měsíčně kredity Azure ve výši

<a id="Setup"></a>
### <a name="setup"></a>Instalace

Chcete-li spustit praktická cvičení v této praktické testovací prostředí, musíte nejdřív nastavit prostředí.

1. Otevřete Průzkumníka Windows a přejděte do testovacího prostředí **zdroj** složky.
2. Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a nainstalujte Visual Studio fragmenty kódu pro toto testovací prostředí.
3. Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, aby bylo možné pokračovat.

> [!NOTE]
> Ujistěte se, že jste zaškrtli všechny závislosti pro toto testovací prostředí před spuštěním instalace.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Používání fragmentů kódu

V celém dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu. Pro usnadnění práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, který se dá dostat z v rámci Visual Studio 2013, abyste ho nemuseli znovu přidat ručně.

> [!NOTE]
> Každý cvičení se sadou počáteční řešení nachází v **začít** složky výkonu, který umožňuje postupovat podle jednotlivých výkon nezávisle na ostatních. Uvědomte si, že chybí z těchto řešení od fragmenty kódu, které se přidávají během cvičení a nemusí fungovat, dokud nedokončíte výkonu. Uvnitř zdrojový kód pro cvičení, můžete také najdete **End** složku, která obsahuje řešení sady Visual Studio s kódem, který je výsledkem dokončení kroků v odpovídající cvičení. Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické vyzkoušení.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické testovací prostředí obsahuje následující praktická cvičení:

1. [Použití migrace Entity Framework](#Exercise1)
2. [Nasazení webové aplikace do přípravného prostředí](#Exercise2)
3. [V produkčním prostředí provádí vrácení nasazení zpět](#Exercise3)
4. [Škálování s využitím služby Azure Storage](#Exercise4)
5. [Použití automatického škálování pro Web Apps](#Exercise5) (volitelné pro Visual Studio 2013 Ultimate edition)

Odhadovaný čas dokončení tohoto testovacího prostředí: **75 minut**

> [!NOTE]
> Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce. Každé předdefinované kolekce je navržená tak, aby odpovídala konkrétním vývojářským styl a určuje rozložení oken, chování editoru, fragmenty kódu technologie IntelliSense a možnosti dialogového okna. Postupy v tomto testovacím prostředí jsou uvedené akce potřebné k provedení dané úlohy v sadě Visual Studio při použití **obecným vývojovým nastavením** kolekce. Pokud se rozhodnete různá nastavení kolekce pro vaše vývojové prostředí, mohou existovat rozdíly v krocích, které byste měli vzít v úvahu.


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Cvičení 1: Použití migrace Entity Framework

Při vývoji aplikace může časem měnit datový model. Tyto změny mohou ovlivnit existující model v databázi (Pokud vytváříte novou verzi) a je potřeba aktualizovat databázi zabráníte tak chybám.

Pro zjednodušení sledování tyto změny v modelu, **migrace Entity Framework Code First** automaticky zjišťuje změny porovnání modelu pomocí schématu databáze a generuje konkrétní kód k aktualizaci databáze, vytváří se nová *verze* vaší databáze.

V tomto cvičení se dozvíte, jak povolit **migrace** pro vaše aplikace a jak můžete snadno zjišťovat a generovat změny k aktualizaci databází.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Úloha 1 – povolení migrace

V této úloze bude projít kroky povolení **migrace Entity Framework Code First** k **kvíz Informatik** databáze, změna modelu a vysvětlení, jak se tyto změny projeví v databáze.

1. Otevřete Visual Studio a otevřete **GeekQuiz.sln** soubor řešení od **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.
2. Sestavte řešení, aby bylo možné stáhnout a nainstalovat **NuGet** balíček závislostí. Chcete-li to provést, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **sestavit řešení** nebo stiskněte klávesu **Ctrl + Shift + B**.
3. Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**a potom klikněte na tlačítko **Konzola správce balíčků**.
4. V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**. Vytvoří se při počáteční migraci na základě existující modelu.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Povolení migrace](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "povolení migrace")

    *Povolení migrace*

    > [!NOTE]
    > Tento příkaz přidá **migrace** složky do projektu se Informatik kvíz, který obsahuje soubor s názvem **Configuration.cs**. **Konfigurace** třída umožňuje konfigurovat chování migrace pro váš kontext.
5. Pomocí migrace povolená, je potřeba aktualizovat **konfigurace** třídy k naplnění databáze s počáteční data, která **kvíz Informatik** vyžaduje. V části **migrace**, nahraďte **Configuration.cs** soubor s tím, který se nachází v **Source\Assets** složka tohoto testovacího prostředí.

    > [!NOTE]
    > Protože **migrace** zavolá **počáteční hodnoty** metoda při každé aktualizaci databáze, je třeba mít jistotu, že nejsou duplicitní záznamy v databázi. **AddOrUpdate** metoda se pomůže zabránit duplicitní data.
6. Pokud chcete přidat počáteční migraci, zadejte následující příkaz a stiskněte klávesu **Enter**.

    > [!NOTE]
    > Ujistěte se, že neexistuje žádná databáze s názvem &quot;GeekQuizProd&quot; ve vaší instanci LocalDB.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Přidání základní schéma migrace](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "přidání základní schéma migrace")

    *Přidání základního schématu migrace*

    > [!NOTE]
    > **Přidejte migraci** bude generování uživatelského rozhraní další migrace na základě změn, které jste provedli pro váš model, od vytvoření posledního migrace. V takovém případě je to první migraci projekt, přidá skripty k vytvoření všech tabulek, které jsou definovány v **TriviaContext** třídy.
7. Spusťte migraci k aktualizaci databáze spuštěním následujícího příkazu. Pro tento příkaz můžete zadat **Verbose** příznak, chcete-li zobrazit příkazy SQL, zavádí do cílové databáze.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Vytvoření první databáze](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "vytváření počáteční databáze.")

    *Vytvoření první databáze*

    > [!NOTE]
    > **Aktualizace databáze** se použije čekající migrace do databáze. V takovém případě se vytvoří databázi pomocí připojovacího řetězce definovaný v souboru web.config.
8. Přejděte na **zobrazení** nabídce a otevřete **Průzkumník objektů systému SQL Server**.

    ![Otevřít v Průzkumníku objektů systému SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "otevřít v Průzkumníku objektů SQL serveru")

    *Otevřít v Průzkumníku objektů SQL serveru*
9. V **Průzkumník objektů systému SQL Server** okna, připojte se k instanci LocalDB kliknutím pravým tlačítkem myši **systému SQL Server** uzlu a vyberete **přidat SQL Server...**  možnost.

    ![Přidání Instance serveru SQL](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "přidání Instance serveru SQL")

    *Přidání instance serveru SQL Server do Průzkumník objektů systému SQL Server*
10. Nastavte **název serveru** k *(localdb) \v11.0* a nechat **ověřování Windows** jako režim ověřování. Klikněte na tlačítko **připojit** pokračujte.

    ![Připojování na instanci LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "připojení na instanci LocalDB")

    *Připojování na instanci LocalDB*
11. Otevřít **GeekQuizProd** databáze a rozbalte **tabulky** uzlu. Jak je vidět **aktualizace databáze** příkaz vygeneruje všechny tabulky, které jsou definovány v **TriviaContext** třídy. Vyhledejte **dbo. TriviaQuestions** tabulky a otevřete uzel sloupce. V dalším úkolem, přidáte nový sloupec do této tabulky a aktualizujte databáze s využitím **migrace**.

    ![Triviální prvek otázky, které sloupce](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "triviální prvek otázky, které sloupce")

    *Triviální prvek otázky, které sloupce*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Úloha 2 – aktualizace schématu databáze pomocí migrace

V této úloze budete používat **migrace Entity Framework Code First** zjistí změnu ve vašem modelu a generovat kód potřebná k aktualizaci databáze. Budete aktualizovat **TriviaQuestions** entity tak, že do ní přidáte novou vlastnost. Potom spustíte příkazy vytvoří novou migraci chcete zahrnout do nového sloupce v tabulce.

1. V **Průzkumníka řešení**, dvakrát klikněte **TriviaQuestion.cs** soubor umístěný uvnitř **modely** složky.
2. Přidat novou vlastnost s názvem **pomocný parametr**, jak je znázorněno v následujícím fragmentu kódu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**. Nová migrace se vytvoří, odrážející změnu v hodnotě náš model.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Přidejte migraci](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "přidat migrace")

    *Přidat migrace*

    > [!NOTE]
    > Soubor migrace se skládá ze dvou způsobů **nahoru** a **dolů**.
    > 
    > - **Nahoru** metoda se použije k určení, co se změní aktuální verze našich aplikací nutnost používat k databázi.
    > - **Dolů** používaných k vrácení změn, přidali jsme do **nahoru** metody.
    > 
    > Při migraci databáze zaktualizuje databázi, se aplikace spustí všechny migrace v pořadí, časové razítko a jenom na ty, které nebyly použity od poslední aktualizace ( \_MigrationHistory tabulka uchovává informace o migraci, které byly použity). **Nahoru** bude volána metoda všechny migrace a provede změny jsme zadali do databáze. Pokud se rozhodneme vrátit k předchozí migrace **dolů** volaná metoda vrátit ke svému změny v obráceném pořadí.
4. V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. Výstup **aktualizace databáze** příkaz vygeneruje **Alter Table** příkaz jazyka SQL, chcete-li přidat nový sloupec, **TriviaQuestions** tabulky, jak je znázorněno na následujícím obrázku.

    ![Přidat příkaz jazyka SQL sloupce generovány](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "přidat generované sloupce příkaz jazyka SQL")

    *Přidat generované sloupce příkaz jazyka SQL*
6. V **Průzkumník objektů systému SQL Server**, aktualizujte **dbo. TriviaQuestions** tabulky a zkontrolujte, že nový **pomocný parametr** je zobrazen sloupec.

    ![Zobrazuje nový sloupec pomocný parametr](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "zobrazuje nový sloupec pomocný parametr")

    *Zobrazuje nový sloupec pomocný parametr*
7. Zpátky **TriviaQuestion.cs** editoru přidejte **StringLength** omezení, které *pomocný parametr* vlastnost, jak je znázorněno v následujícím fragmentu kódu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. V **Konzola správce balíčků**, zadejte následující příkaz a stiskněte klávesu **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. Výstup **aktualizace databáze** příkaz vygeneruje **Alter Table** SQL. doje k aktualizaci *pomocný parametr* typ sloupce **TriviaQuestions** tabulky, jak je znázorněno na následujícím obrázku.

    ![Příkaz ALTER vygenerovat příkaz SQL sloupec](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter vygenerovat příkaz SQL sloupec")

    *Příkaz ALTER vygenerovat příkaz SQL sloupec*
11. V **Průzkumník objektů systému SQL Server**, aktualizujte **dbo. TriviaQuestions** tabulky a zkontrolujte, že **pomocný parametr** typ sloupce je **nvarchar(150)**.

    ![Zobrazení omezení new](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "znázorňující omezení new")

    *Zobrazuje nové omezení*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Cvičení 2: Nasazení webové aplikace do přípravného prostředí

**Webové aplikace ve službě Azure App Service** umožňuje provádět fázované publikování. Fázované publikování vytváří slot pro každému výchozímu produkčnímu webu fázi webu a umožňuje vám přepínat mezi těmito sloty bez časové prodlevy. To je velmi užitečné ověřit změny před uvolněním veřejně, postupně integrovat obsah webu a vrátit zpět, jestliže změny nefungují podle očekávání.

V tomto cvičení nasadíte **kvíz Informatik** aplikaci do přípravného prostředí vaší webové aplikace pomocí správy zdrojového kódu Gitu. K tomuto účelu bude vytvoření webové aplikace a zřízení požadovaných součástí na portálu pro správu, konfiguraci **Git** zdrojového kódu ze svého místního počítače do přípravného slotu úložiště a nabízených oznámení aplikace. Budete také aktualizovat vaši produkční databázi s **migrace Code First** jste vytvořili v předchozím cvičení. V tomto testovacím prostředí k ověření své činnosti pak spustí aplikaci. Jakmile budete spokojeni se pracuje podle vašich očekávání, bude podporovat aplikace do produkčního prostředí.

> [!NOTE]
> Chcete-li fázované publikování, musí být webové aplikace v **standardní režim**. Všimněte si, že další poplatky podle tarifu při změně vaší webové aplikace do standardního režimu. Další informace o cenách najdete v tématu [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Úloha 1 – Vytvoření webové aplikace ve službě Azure App Service

V této úloze vytvoříte webovou aplikaci v **služby Azure App Service** z portálu pro správu. Můžete také nakonfigurovat **SQL Database** uchování dat aplikací a konfigurace místní úložiště Git pro správu zdrojového kódu.

1. Přejděte [portál pro správu Azure](https://manage.windowsazure.com) a přihlaste se pomocí účtu Microsoft, který je spojen s vaším předplatným.

    ![Přihlaste se k portálu pro správu Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Přihlaste se k portálu pro správu Azure*
2. Klikněte na tlačítko **nový** na příkazovém řádku v dolní části stránky.

    ![Vytváří se nová webová aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "vytvoření nové webové aplikace")

    *Vytvoření nové webové aplikace*
3. Klikněte na tlačítko **Compute**, **webu** a potom **vytvořit vlastní**.

    ![Vytvoření nové webové aplikace pomocí vytvořit vlastní](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "vytvoření nové webové aplikace s použitím vlastní vytvoření")

    *Vytvoření nové webové aplikace s použitím vlastní vytvoření*
4. V **nový - vytvořit vlastní web** dialogového okna zadejte dostupného **adresy URL** (třeba *informatik kvíz*), vyberte umístění, do **oblasti** rozevírací seznam a vyberte **vytvořit novou databázi SQL** v **databáze** rozevíracího seznamu. Nakonec vyberte **publikování ze správy zdrojových kódů** zaškrtávací políčko a klikněte na tlačítko **Další**.

    ![Přizpůsobení novou webovou aplikaci](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Přizpůsobení novou webovou aplikaci*
5. Zadejte následující informace pro nastavení databáze:

   - V **název** textové pole, zadejte název databáze (třeba *geekquiz\_db*)
   - Na serveru **rozevíracího seznamu** seznamu vyberte **nový SQL server database**. Alternativně můžete vybrat existující server.
   - V **uživatelské jméno pro databázi** a **heslo databáze** polí, zadejte uživatelské jméno správce a heslo pro server služby SQL database. Pokud zvolíte možnost server jste již vytvořili, zobrazí se výzva k zadání hesla.

     ![Nastavení databáze](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Nastavení databáze*
6. Klikněte na tlačítko **Další** pokračujte.
7. Vyberte **místního úložiště Git** pro správu zdrojového kódu a klikněte na tlačítko **Další**.

    > [!NOTE]
    > Můžete být vyzváni k nasazení pověření (uživatelské jméno a heslo).

    ![Vytvoření úložiště Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Vytvoření úložiště Git*
8. Počkejte, až se vytvoří nová webová aplikace.

    > [!NOTE]
    > Ve výchozím nastavení, Azure poskytuje domén na *azurewebsites.net* ale také vám dává možnost nastavit vlastní domény pomocí portálu pro správu Azure. Pokud používáte režimy určité služby Azure App Service však můžete spravovat pouze vlastních domén.
    > 
    > Azure App Service je k dispozici v edicích Free, Shared, Basic, Standard a Premium. Všechny webové aplikace v režimu Free a Shared spustit v prostředí s více tenanty a kvóty pro využití procesoru, paměti a sítě. Maximální počet bezplatných aplikací může lišit podle vašeho plánu. Ve standardním režimu zvolte výpočetní prostředky, které aplikace spustit na vyhrazených virtuálních počítačích, které odpovídají na standardní Azure. Můžete najít v konfiguraci webové aplikace režim **škálování** nabídky vaší webové aplikace.
    > 
    > ![Azure App Service režimy](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "režimy služby Azure App Service")
    > 
    > Pokud používáte **Shared** nebo **standardní** režimu, bude možné spravovat vlastní domény pro webovou aplikaci tak, že přejdete do vaší aplikace **konfigurovat** nabídky a kliknutím na **Správa domén** pod *názvy domén*.
    > 
    > ![Spravovat domény](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "spravovat domény")
    > 
    > ![Správa vlastních domén](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "spravování vlastních domén")
9. Po vytvoření webové aplikace klikněte na odkaz v části **URL** sloupci zkontrolujte, že se nová webová aplikace spuštěná.

    ![Přejdete do nové webové aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Přejdete do nové webové aplikace*

    ![Webová aplikace spuštěná](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *Webová aplikace spuštěná*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Úloha 2 – Vytvoření produkční databáze SQL

V této úloze budete používat **migrace Entity Framework Code First** k vytvoření databáze cílení **Azure SQL Database** instanci, kterou jste vytvořili v předchozí úloze.

1. Na portálu Management Portal, přejděte do webové aplikace, kterou jste vytvořili v předchozí úloze a přejděte do jeho **řídicí panel**.
2. V **řídicí panel** klikněte na **zobrazit připojovací řetězce** odkaz pod **rychlý přehled** oddílu.

    ![Zobrazit připojovací řetězce](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "zobrazit připojovací řetězce")

    *Zobrazit připojovací řetězce*
3. Kopírovat **připojovací řetězec** hodnotu a zavřete dialogové okno.

    ![Připojovací řetězec v portálu pro správu Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "připojovací řetězec v portálu pro správu Azure")

    *Připojovací řetězec v portálu pro správu Azure*
4. Klikněte na tlačítko **databází SQL** zobrazíte seznam databází SQL v Azure

    ![Nabídka SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "nabídce databáze SQL")

    *Nabídka SQL Database*
5. Databáze, kterou jste vytvořili v předchozí úloze vyhledejte a klikněte na Server.

    ![Server služby SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "serveru služby SQL Database")

    *Server služby SQL Database*
6. V **rychlý Start** stránka serveru, klikněte na **konfigurovat**.

    ![Konfigurovat nabídky](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "konfigurovat nabídky")

    *Konfigurace nabídky*
7. V **povolené IP adresy** části, klikněte na **přidat do povolených IP adres** odkaz umožňující vaše IP adresa pro připojení k databázi SQL serveru.

    ![Povolené IP adresy](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "povolené IP adresy")

    *Povolené IP adresy*
8. Klikněte na tlačítko **Uložit** v dolní části stránky k dokončení kroku.
9. Přepněte zpět do sady Visual Studio.
10. V **Konzola správce balíčků**, spusťte následující příkaz nahrazení *[YOUR-CONNECTION-STRING]* zástupného symbolu připojovacím řetězcem, který jste zkopírovali z Azure

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Aktualizace databáze cílení na Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "aktualizovat databázi cílení na Windows Azure SQL Database")

    *Aktualizace databáze, které cílí na Azure SQL Database*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Úloha 3 – nasazení kvíz nadšence do přípravného prostředí pomocí Gitu

V této úloze povolíte fázované publikování ve webové aplikaci. Potom se pomocí Gitu publikovat aplikace Informatik kvíz přímo ze svého místního počítače do přípravného prostředí vaší webové aplikace.

1. Přejděte zpět na portál a klikněte na název webové aplikace v rámci **název** sloupec, který se zobrazí na stránkách správy.

    ![Otevření webové stránky správy aplikace](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Otevření webové stránky správy aplikace*
2. Přejděte **škálování** stránky. V části **Obecné** vyberte **standardní** konfigurace a klikněte na **Uložit** na panelu příkazů.

    > [!NOTE]
    > Spustit všechny webové aplikace v aktuální oblasti a předplatném v **standardní** režimu, ponechejte tuto položku **Vybrat vše** zaškrtnuto políčko **zvolit servery** konfigurace. Jinak, zrušte **Vybrat vše** zaškrtávací políčko.

    ![Upgrade webové aplikace do standardního režimu](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "upgrade webové aplikace na standardní režim")

    *Upgrade webové aplikace na standardní režim*
3. Klikněte na tlačítko **Ano** potvrďte provedené změny.

    ![Potvrzují se změny do standardního režimu](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "budete pokračovat se změnou režimu webové aplikace")

    *Potvrzují se změny do standardního režimu*
4. Přejděte na **řídicí panel** stránky a klikněte na tlačítko **fázované publikování na Povolit** pod **rychlý přehled** oddílu.

    ![Povolení fázované publikování](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "povolení fázované publikování")

    *Povolení fázované publikování*
5. Klikněte na tlačítko **Ano** umožňující fázované publikování.

    ![Fázované publikování na potvrzení](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "kliknete na Ano, povolit fázované publikování")

    *Potvrzují se fázované publikování*
6. V seznamu webových aplikací rozbalte na políčko nalevo od název vaší webové aplikace k zobrazení slot pro fázi webu. Obsahuje název vaší webové aplikace, za nímž následuje ***(pracovní)***. Klikněte na pracovním webu přejdete na stránku pro správu.

    ![Přejdete na pracovní aplikaci](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "přejdete do pracovní webové aplikace")

    *Přejděte do pracovní aplikace*
7. Všimněte si, že tuto stránku správy he vypadá jako jakoukoli jinou webovou aplikaci pro správu stránky. Přejděte **nasazení** stránky a zkopírujte **adresa URL pro Git** hodnotu. Použijete ji později v tomto cvičení.

    ![Kopírování hodnoty adresy URL pro Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Kopírování hodnoty adresy URL pro Git*
8. Otevřete novou **Git Bash** konzoly a spusťte následující příkazy. Aktualizace *[YOUR cesta aplikace]* zástupný symbol s cestou **GeekQuiz** řešení, umístěný ve **Source\Ex1 DeployingWebSiteToStaging\Begin** složky Toto testovací prostředí.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inicializace Git a první potvrzení](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Inicializace Git a první potvrzení*
9. Spusťte následující příkaz tak, aby nabízel webovou aplikaci do vzdáleného **Git** úložiště. Zástupný symbol nahraďte adresu URL, které jste získali z portálu pro správu. Zobrazí se výzva k zadání hesla nasazení.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Doručením (push) do Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Doručením (push) do Azure*

    > [!NOTE]
    > Při nasazení obsahu na serveru FTP hostitele nebo úložiště GIT webové aplikace musí ověřit pomocí **přihlašovací údaje pro nasazení** , kterou jste vytvořili z webové aplikace **rychlý Start** nebo **řídicího panelu**  správu stránek. Pokud si nejste jisti přihlašovací údaje pro nasazení je snadno obnovit pomocí portálu pro správu. Otevřít webovou aplikaci **řídicí panel** stránky a klikněte na tlačítko **resetovat přihlašovací údaje pro nasazení** odkaz. Zadejte nové heslo a klikněte na tlačítko **OK**. Přihlašovací údaje pro nasazení jsou platná pro použití s všech webových aplikací přidružených k vašemu předplatnému.
10. Pokud chcete ověřit, webové aplikace bylo úspěšně vloženo do Azure, přejděte zpět na portál pro správu a klikněte na tlačítko **Websites**.
11. Vyhledejte vaši webovou aplikaci a rozbalte položku zobrazíte slot pro fázi webu. Klikněte na jeho **název** přejdete na stránku pro správu.
12. Klikněte na tlačítko **nasazení** zobrazíte **historie nasazení**. Ověřte, zda je **aktivní nasazení** s vaší  *&quot;počáteční potvrzení&quot;*.

    ![Aktivní nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Aktivní nasazení*
13. Nakonec klikněte na tlačítko **Procházet** na příkazovém řádku přejděte do webové aplikace.

    ![Procházet webovou aplikaci](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Procházet webovou aplikaci*
14. Pokud aplikace je úspěšně nasazená, zobrazí se přihlašovací stránky dotazníku nadšence.

    > [!NOTE]
    > Adresa URL nasazené aplikace obsahuje název vaší webové aplikace, za nímž následuje *– pracovní*.

    ![Aplikace běžící v testovacím prostředí](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Aplikace běžící v testovacím prostředí*
15. Pokud chcete prozkoumat aplikace, klikněte na tlačítko **zaregistrovat** k registraci nového uživatele. Vyplňte podrobnosti účtu tak, že zadáte uživatelské jméno, e-mailovou adresu a heslo. V dalším kroku aplikace ukazuje na první otázku, kvíz. Odpovězte na několik otázek, abyste měli jistotu, že funguje podle očekávání.

    ![Aplikace je připravená k použití](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Aplikace je připravená k použití*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Úloha 4 – Podpora webové aplikace do produkčního prostředí

Teď, když si ověříte, že webová aplikace pracuje správně v testovacím prostředí, budete chtít přesunout do výroby. V této úloze se Prohodit slot pro fázi webu s produkční slot webu.

1. Přejděte zpět na portál pro správu a vyberte slot pro fázi webu. Klikněte na tlačítko **Prohodit** na panelu příkazů.

    ![Zaměnit do produkčního prostředí](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Zaměnit do produkčního prostředí*
2. Klikněte na tlačítko **Ano** v potvrzovacím dialogovém okně, aby bylo možné pokračovat v operaci prohození. Azure bude okamžitě Prohodit obsah pracoviště s obsahem na pracovním webu.

    > [!NOTE]
    > Některá nastavení z připravenou verzi se automaticky zkopírují do produkční verze (například připojovací řetězec přepsání, mapování obslužných rutin, atd.), ale ostatní nastavení nedojde ke změně (např. koncové body DNS, vazby SSL atd.).

    ![Potvrzení operace prohození](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Potvrzení operace prohození*
3. Po dokončení prohození vyberte slot produkčního prostředí a klikněte na tlačítko **Procházet** na panelu příkazů otevřete pracoviště. Všimněte si, že adresa URL do adresního řádku.

    > [!NOTE]
    > Vám může být nutné aktualizovat prohlížeč vymazat mezipaměť. V aplikaci Internet Explorer, provedete stisknutím kombinace kláves **CTRL + R**.

    ![Webová aplikace spuštěná v produkčním prostředí](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. V **GitBash** konzole, aktualizovat vzdálenou adresu URL pro místní úložiště Git cílit na produkční slot. Chcete-li to provést, spusťte následující příkaz zástupné texty nahraďte uživatelské jméno pro nasazení a název vaší webové aplikace.

    > [!NOTE]
    > V následujícím cvičení vložíte změny do produkční lokality místo pracovní pouze pro zjednodušení tohoto prostředí. Ve skutečném scénáři se doporučuje ověřit změny v testovacím prostředí před zvýšením úrovně do produkčního prostředí.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Cvičení 3: Provedení vrácení nasazení zpět v produkčním prostředí

Existují scénáře, ve kterém nemáte přípravný slot provádět horké prohození mezi přípravným a produkčním prostředím, například pokud pracujete s **Free** nebo **Shared** režimu. V těchto scénářích měli byste otestovat vaši aplikaci v testovacím prostředí – místně nebo ve vzdálené lokalitě – před nasazením do produkčního prostředí. Je však možné, že problém nebyl zjištěn během fáze testování mohou nastat v produkční lokality. V takovém případě je důležité mít mechanismus pro snadno přepínat na předchozí a více stabilní verzi aplikace co nejrychleji.

V **služby Azure App Service**, toto je to možné díky díky průběžné nasazování ze správy zdrojového kódu **znovu nasadit** akce, které jsou k dispozici na portálu management portal. Uchovává informace o nasazení přidružená k potvrzení změn do úložiště Azure a nabízí možnost znovu nasadit aplikaci pomocí kteréhokoli z předchozích nasazení, kdykoli.

V tomto cvičení budete provádět změny kódu v **Informatik kvíz** aplikaci, která záměrně vkládá *chyb*. Budete nasazovat aplikaci do produkčního prostředí, abyste viděli chybu a pak bude využít výhod funkce opětovného nasazení se vrátíte do předchozího stavu.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Úloha 1 – aktualizace aplikace kvíz Informatik

V této úloze bude Refaktorovat malou část kódu **TriviaController** třídy k extrakci část logiky, který načte kvíz vybranou možnost z databáze do nové metody.

1. Přepnout na instanci aplikace Visual Studio s **GeekQuiz** řešení v předchozím cvičení.
2. V **Průzkumníka řešení**, otevřete **TriviaController.cs** soubor uvnitř **řadiče** složky.
3. Vyhledejte **StoreAsync** metody a vyberte zvýrazněný kód na následujícím obrázku.

    ![Vyberte kód](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Vyberte kód*
4. Klikněte pravým tlačítkem na vybraný kód, rozbalte **Refaktorovat** nabídky a vybereme **extrahovat metodu...** .

    ![Extrahování kód jako novou metodu](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Výběr metody extrahování*
5. V **extrahovat metodu** dialogové okno, název nové metody *MatchesOption* a klikněte na tlačítko **OK**.

    ![Zadání názvu – metoda](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Zadejte název pro extrahovaný – metoda*
6. Vybraný kód je pak extrahován do **MatchesOption** metody. Výsledný kód je znázorněno v následujícím fragmentu kódu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Úloha 2 – opětovného nasazení aplikace kvíz Informatik

Nyní budou nabízená oznámení změn, které jste provedli v předchozí úloze úložiště, které aktivují nové nasazení do produkčního prostředí. Potom se potíže, problém pomocí **vývojářské nástroje F12** poskytované Internet Exploreru a pak proveďte vrácení zpět na předchozí nasazení z portálu pro správu Azure.

1. Otevřete novou **Git Bash** konzoly nasaďte aktualizovanou aplikaci do služby Azure App Service.
2. Spusťte následující příkazy, které odešlete změny do Azure. Aktualizace *[YOUR cesta aplikace]* zástupný symbol s cestou **GeekQuiz** řešení. Zobrazí se výzva k zadání hesla nasazení.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Nahráním refaktorovaný kódu do Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Nahráním refaktorovaný kódu do Azure*
3. Otevřete aplikaci Internet Explorer a přejděte do své webové aplikace (například `http://<your-web-site>.azurewebsites.net`). Přihlaste se pomocí dříve vytvořeného přihlašovacích údajů.
4. Stisknutím klávesy **F12** ke spuštění nástroje pro vývoj, vyberte **sítě** kartě a klikněte na tlačítko **Přehrát** tlačítko Spustit záznam.

    ![Spouštění nahrávání sítě](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "od sítě záznam")

    *Spouští se záznam sítě*
5. Vyberte všechny možnosti kvíz. Zobrazí se, že se nic nestane.
6. V **F12** okno, zobrazuje položku odpovídající požadavek POST HTTP protokolu HTTP **500** výsledek.

    ![HTTP 500 chyb](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 chyb*
7. Vyberte **konzoly** kartu. Podrobnosti o příčině je zaznamenána chyba.

    ![Zaznamenané chyby](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Zaznamenané chyby*
8. Vyhledejte část Podrobnosti o této chybě. Je zřejmé tato chyba je způsobena kódu, refaktoring jste v předchozích krocích.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. Nezavírejte prohlížeče.
10. V nové instanci prohlížeče, přejděte [portál pro správu Azure](https://manage.windowsazure.com) a přihlaste se pomocí účtu Microsoft, který je spojen s vaším předplatným.
11. Vyberte **Websites** a klikněte na webovou aplikaci, kterou jste vytvořili v cvičení 2.
12. Přejděte **nasazení** stránky. Všimněte si, že provádí všechna potvrzení změn jsou uvedeny v historii nasazení.

    ![Seznam existujících nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Seznam existujících nasazení*
13. Vybrat předchozí potvrzení změn a klikněte na tlačítko **znovu nasadit** na panelu příkazů.

    ![Znovu se nasazuje předchozí potvrzení změn](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Znovu se nasazuje předchozí potvrzení změn*
14. Po zobrazení výzvy k potvrzení klikněte na tlačítko **Ano**.

    ![Potvrzují se opětovné nasazení](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Po dokončení nasazení se přepněte zpět do instance prohlížeče s vaší webové aplikace a stisknutím klávesy **CTRL + F5**.
16. Klikněte na některou z možností. Překlopit animace bude nyní provést a výsledek (*opravte nesprávné*) se zobrazí.
17. (Volitelné) Přepněte **Git Bash** konzoly a spusťte následující příkazy se vrátit k předchozí potvrzení změn.

    > [!NOTE]
    > Tyto příkazy vytvoří nové potvrzení, který vrátí zpět všechny změny v úložišti Git, které byly provedeny v chybný potvrzení. Azure pak znovu nasadit aplikaci pomocí nové potvrzení.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Cvičení 4: Škálování s využitím služby Azure Storage

**Objekty BLOB** představují nejjednodušší způsob, jak ukládat velké objemy nestrukturovaných textových nebo binárních dat jako jsou videa, zvukové soubory a obrázky. Přesunutí statický obsah aplikace do úložiště, umožňuje škálovat aplikaci podle poskytování obrázků nebo dokumentů přímo do prohlížeče.

V tomto cvičení se přesune statický obsah aplikace do kontejneru objektů Blob. Pak se konfigurace aplikace pro přidání **přepisu adresy URL technologie ASP.NET pravidlo** v **Web.config** přesměrovat obsah do kontejneru objektů Blob.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Úloha 1 – Vytvoření účtu služby Azure Storage

V této úloze se dozvíte, jak vytvořit nový účet úložiště pomocí portálu pro správu.

1. Přejděte [portál pro správu Azure](https://manage.windowsazure.com) a přihlaste se pomocí účtu Microsoft, který je spojen s vaším předplatným.
2. Vyberte **nové | Data Services | Úložiště | Příkaz pro rychlé vytvoření** a začněte vytvářet nový účet úložiště. Zadejte jedinečný název pro účet a vyberte **oblasti** ze seznamu. Klikněte na tlačítko **vytvořit účet úložiště** pokračujte.

    ![Vytváří se nový účet úložiště](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "vytváření nového účtu úložiště")

    *Vytváří se nový účet úložiště*
3. V **úložiště** části, počkejte, dokud stav nového účtu úložiště se změní na *Online* budete pokračovat následujícím krokem.

    ![Účet úložiště vytvořený](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "vytvořený účet úložiště")

    *Vytvoření účtu úložiště*
4. Klikněte na název účtu úložiště a pak klikněte na tlačítko **řídicí panel** odkazu v horní části stránky. **Řídicí panel** stránce vám poskytne informace o stavu účtu a koncové body služby, které lze použít v rámci vašich aplikací.

    ![Zobrazení řídicího panelu úložiště účtu](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "zobrazení řídicího panelu účtu úložiště")

    *Zobrazení řídicí panel účtu úložiště*
5. Klikněte na tlačítko **spravovat přístupové klíče** tlačítko v navigačním panelu.

    ![Tlačítko Spravovat přístupové klíče](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "tlačítko Spravovat přístupové klíče")

    *Správa přístupových klíčů tlačítko*
6. V **spravovat přístupové klíče** dialogové okno, kopie **název účtu úložiště** a **primární přístupový klíč** , kdykoli budete potřebovat v následujícím cvičení. Zavřete dialogové okno.

    ![Přístupový klíč dialogového okna Spravovat](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "dialogové okno Spravovat přístupový klíč")

    *Přístupový klíč dialogového okna Spravovat*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Úloha 2 – nahrání prostředek do úložiště objektů Blob v Azure

V této úloze použijete okno Průzkumníka serveru ze sady Visual Studio pro připojení k účtu úložiště. Pak vytvoříte kontejner objektů blob a odeslat soubor s logem kvíz nadšence do kontejneru.

1. Přepnout na instanci aplikace Visual Studio s **GeekQuiz** řešení v předchozím cvičení.
2. V panelu nabídky vyberte **zobrazení** a potom klikněte na tlačítko **Průzkumníka serveru**.
3. V **Průzkumníka serveru**, klikněte pravým tlačítkem myši **Azure** uzel a vyberte možnost **připojit se k Azure...** . Přihlaste se pomocí účtu Microsoft, který je spojen s vaším předplatným.

    ![Připojte se k Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Připojení k Azure*
4. Rozbalte **Azure** uzel, klikněte pravým tlačítkem na **úložiště** a vyberte **připojit externí úložiště...** .
5. V **přidat nový účet úložiště** dialogového okna zadejte **název účtu** a **klíč účtu** jste získali v předchozí úloze a klikněte na **OK**.

    ![Přidat nový účet úložiště – dialogové okno](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Přidat nový účet úložiště – dialogové okno*
6. Váš účet úložiště by se měla zobrazit v části **úložiště** uzlu. Rozbalte účet úložiště, klikněte pravým tlačítkem na **objekty BLOB** a vyberte **vytvořit kontejner objektů Blob...** .

    ![Vytvořit kontejner objektů Blob](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "vytvořit kontejner objektů Blob")

    *Vytvořit kontejner objektů Blob*
7. V **vytvořit kontejner objektů Blob** dialogového okna zadejte název kontejneru objektů blob a klikněte na tlačítko **OK**.

    ![Dialogové okno vytvořit kontejner objektů Blob](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "dialogové okno vytvořit kontejner objektů Blob")

    *Vytvoření dialogového okna kontejner objektů Blob*
8. Nový kontejner objektů blob měla být přidána do **objekty BLOB** uzlu. Změňte přístupová oprávnění v kontejneru zveřejněte kontejneru. Chcete-li to provést, klikněte pravým tlačítkem **image** kontejner a vyberte **vlastnosti**.

    ![Image kontejneru vlastnosti](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "Image vlastnosti kontejneru")

    *Vlastnosti kontejneru obrázků*
9. V **vlastnosti** okno, nastaveno **veřejné oprávnění ke čtení** k **kontejneru**.

    ![Změna vlastnosti veřejné oprávnění ke čtení](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "při změně hodnoty vlastnosti veřejné oprávnění ke čtení")

    *Změna vlastnosti veřejné oprávnění ke čtení*
10. Po zobrazení výzvy, pokud jste si jistí, kterou chcete změnit vlastnost veřejného přístupu, klikněte na tlačítko **Ano**.

    ![Microsoft Visual Studio upozornění](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "upozornění Microsoft Visual Studio")

    *Microsoft Visual Studio upozornění*
11. V **Průzkumníka serveru**, klikněte pravým tlačítkem **image** kontejner objektů blob a vyberte **kontejner objektů Blob zobrazení**.

    ![Zobrazit kontejner objektů Blob](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "zobrazení kontejner objektů Blob")

    *Zobrazit kontejner objektů Blob*
12. Kontejner imagí by se otevřít v novém okně a legenda s žádné položky, které se má zobrazit. Klikněte na tlačítko **nahrát** ikonu pro nahrání souboru do kontejneru objektů blob.

    ![Image kontejneru s žádné položky](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Image kontejneru s žádné položky")

    *Image kontejneru s žádné položky*
13. V **nahrát objekt Blob** dialogové okno, přejděte na **prostředky** složky testovacího prostředí. Vyberte **logo big.png** souboru a klikněte na tlačítko **otevřít**.
14. Počkejte, až se soubor nahraje. Po dokončení nahrávání souboru by měl nacházet na Image kontejneru. Klikněte pravým tlačítkem na položku soubor a vyberte **kopírování adresy URL**.

    ![Zkopírujte adresu URL objektu blob](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "zkopírujte adresu URL souboru objektu blob")

    *Zkopírujte adresu URL objektu blob*
15. Spusťte aplikaci Internet Explorer a vložte adresu URL. Na následujícím obrázku by měla být zobrazená v prohlížeči.

    ![Obrázek loga big.png z úložiště objektů Blob Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo big.png image ze služby storage")

    *Logo big.png image z Azure Blob Storage*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Úloha 3 – aktualizuje se řešení využívají statický obsah z úložiště objektů Blob v Azure

V této úloze nakonfigurujete **GeekQuiz** řešení, které využívají obrázek nahraný do Azure Blob Storage (a nikoli obraz umístěný ve webové aplikaci) tak, že přidáte pravidlo pro přepis adres URL technologie ASP.NET, v **web.config**souboru.

1. V sadě Visual Studio, otevřete **Web.config** soubor uvnitř **GeekQuiz** projektu a vyhledejte **&lt;system.webServer&gt;** elementu.
2. Přidejte následující kód pro přidání přepisování adres URL pravidla aktualizace zástupného textu s názvem vašeho účtu úložiště.

    (Fragment - kódu *WebSitesInProduction - Ex4 - UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > Přepisování adres URL je proces zachycení příchozí webové žádosti a požadavku přesměrování na jiný prostředek. Adresa URL přepsání pravidel říká přepis adres modul, když se požadavek musí přesměrovat a kde by měl být přesměrován. Pravidlo pro přepis adres se skládá ze dvou řetězců: model v požadované adrese URL (obvykle, pomocí regulárních výrazů), a řetězec pro nahrazení vzoru, pokud nalezen. Další informace najdete v tématu [přepisování adres URL v technologii ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).
3. Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.
4. Otevřete novou **Git Bash** konzoly nasaďte aktualizovanou aplikaci do služby Azure App Service.
5. Spusťte následující příkazy, které odešlete změny do Azure. Aktualizace *[YOUR cesta aplikace]* zástupný symbol s cestou **GeekQuiz** řešení. Zobrazí se výzva k zadání hesla nasazení.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Nasazení aktualizací do Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Nasazení aktualizací do Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Úloha 4 – ověření

V této úloze budete používat **aplikace Internet Explorer** a přejděte **Informatik kvíz** aplikace a zkontrolujte, že adresa URL přepsat pravidlo pro Image funguje a můžete se přesměrují na image hostovanou v **objektů Blob v Azure Úložiště**.

1. Otevřete aplikaci Internet Explorer a přejděte do své webové aplikace (například `http://<your-web-site>.azurewebsites.net`). Přihlaste se pomocí dříve vytvořeného přihlašovacích údajů.

    ![Zobrazuje webovou aplikaci Informatik kvíz s imagí](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "zobrazující webovou aplikaci Informatik kvíz s imagí")

    *Zobrazuje webovou aplikaci Informatik kvíz s imagí*
2. Stisknutím klávesy **F12** ke spuštění nástroje pro vývoj, vyberte **sítě** kartu a spusťte záznam.

    ![Spouštění nahrávání sítě](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "od sítě záznam")

    *Spouští se záznam sítě*
3. Stisknutím klávesy **CTRL + F5** aktualizujte webovou stránku.
4. Po dokončení načítání stránky byste měli vidět požadavku HTTP pro **/img/logo-big.png** adresu URL pomocí protokolu HTTP **301** výsledek (přesměrování) a další žádost o `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` adresu URL s protokolem HTTP **200** výsledek.

    ![Ověřuje se adresa URL přesměrování](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "zobrazující přesměrování v nástroje pro vývojáře")

    *Ověřuje se adresa URL pro přesměrování*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Cvičení 5: Použití automatického škálování pro webové aplikace

> [!NOTE]
> Tento postup je volitelné, protože vyžaduje podporu pro zatížení webové &amp; testování výkonu, která je dostupná jenom pro **Visual Studio 2013 Ultimate Edition**. Další informace o konkrétní součásti, které Visual Studio 2013, porovnat verze [tady](https://www.microsoft.com/visualstudio/eng/products/compare).


**Azure App Service Web Apps** poskytuje funkci automatického škálování pro web apps spouštěná ve **standardní režim**. Automatické škálování umožňuje Azure se dá automaticky škálovat počet instancí webové aplikace v závislosti na zatížení. Když je povolené automatické škálování, Azure kontroluje každých pět minut procesoru vaší webové aplikace a přidá instancí podle potřeby v daném okamžiku v čase. Pokud bude malé využití procesoru, Azure odebere instance každé dvě hodiny zajistit, že není snížený výkon webové aplikace.

V tomto cvičení jste projít kroky potřebnými ke konfiguraci **automatického škálování** funkce pro **kvíz Informatik** webové aplikace. Tato funkce bude ověřte spuštěním zátěžového testu sady Visual Studio pro generování dostatečného zatížení procesoru v aplikaci k aktivaci instance upgrade.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Úloha 1 – konfigurace automatického škálování na základě metriky využití procesoru

V této úloze bude používat portál pro správu Azure k povolení této funkce automatického škálování pro webovou aplikaci, kterou jste vytvořili v cvičení 2.

1. V [portál pro správu Azure](https://manage.windowsazure.com/)vyberte **Websites** a klikněte na webovou aplikaci, kterou jste vytvořili v cvičení 2.
2. Přejděte **škálování** stránky. V části **kapacity** vyberte **procesoru** pro **škálování podle metriky** konfigurace.

    > [!NOTE]
    > Při horizontálním škálování podle využití procesoru, Azure dynamicky přizpůsobí počet instancí, které aplikace používá, pokud se změní využití procesoru.

    ![Výběr škálování podle využití procesoru](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "výběrem procesoru metriky pro automatické škálování")

    *Výběr škálování podle využití procesoru*
3. Změnit **cílový procesor** konfiguraci **20**-**40** procent.

    > [!NOTE]
    > Tento rozsah představuje průměrné využití procesoru pro vaši webovou aplikaci. Azure bude přidávat nebo odebírat instance, které chcete zachovat svou webovou aplikaci v tomto rozsahu. Zadat minimální a maximální počet instancí používaných pro škálování v **počet instancí** konfigurace. Azure se nikdy dostanou nad nebo nad rámec tohoto limitu.
    > 
    > Výchozí hodnota **cílový procesor** hodnoty jsou změněny jen pro účely tohoto testovacího prostředí. Nakonfigurováním rozsahu procesoru malé hodnotami jsou zvyšuje šance na aktivační událost automatického škálování při moderování zatížení je umístěn na aplikaci.

    ![Změna cílového procesoru chcete být 20 až 40 procent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "změna cíl CPU, který má být 20 až 40 procent")

    *Změna cílový procesor bude 20 až 40 procent*
4. Klikněte na tlačítko **Uložit** na panelu příkazů a uložte změny.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Úloha 2 – zátěžové testování pomocí sady Visual Studio

Teď, když **automatického škálování** byl nakonfigurován, musíte vytvořit **načíst projekt testu výkonu webu a** v sadě Visual Studio k vygenerování nějaké zatížení CPU na webovou aplikaci.

1. Otevřít **Visual Studio Ultimate 2013** a vyberte **soubor | Nové | Projekt...**  spusťte nové řešení.

    ![Vytvoření nového projektu](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "vytvoření nového projektu")

    *Vytvoření nového projektu*
2. V **nový projekt** dialogu **webový výkon a projekt zátěžového testu** pod **Visual C# | Test** kartu. Ujistěte se, že **rozhraní .NET Framework 4.5** je vybrána, pojmenujte projekt *WebAndLoadTestProject*, zvolte **umístění** a klikněte na tlačítko **OK**.

    ![Vytvoření nového projektu webového a zátěžového testu](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "vytvoření nového projektu webového a zátěžového testu")

    *Vytvoření nového projektu webového a zátěžového testu*
3. V **WebTest1.webtest** klikněte pravým tlačítkem na **WebTest1** uzel a klikněte na tlačítko **přidejte žádosti**.

    ![Žádost o přidání do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "přidáním požadavku do WebTest1")

    *Žádost o přidání do WebTest1*
4. V **vlastnosti** okno nového uzlu žádost o aktualizaci **Url** vlastnost tak, aby odkazoval na adresu URL webové aplikace (například *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).

    ![Změna vlastnosti adresy Url](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "při změně hodnoty vlastnosti adresy Url")

    *Změna vlastnosti adresy Url*
5. V **WebTest1.webtest** okna, klikněte pravým tlačítkem na **WebTest1** a klikněte na tlačítko **přidejte smyčku...** .

    ![Přidání smyčky do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "přidání smyčky do WebTest1")

    *Přidání smyčky do WebTest1*
6. V **přidat podmíněné pravidlo a položky do cyklu** dialogové okno, vyberte **smyčky For Loop** pravidla a upravit následující vlastnosti.

   1. **Ukončovací hodnota:** 1000
   2. **Název parametru kontextu:** iterátoru
   3. **Přírůstková hodnota:** 1

      ![Vyberte pravidlo smyčku For a aktualizace vlastností](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "vyberte pravidlo smyčku For a aktualizace vlastností")

      *Vyberte pravidlo smyčku For a aktualizace vlastností*
7. V části **položky ve smyčce** vyberte žádosti, které jste předtím vytvořili pro první a poslední položka smyčky. Klikněte na tlačítko **OK** pokračujte.

    ![Výběr položek první a poslední smyčky](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "výběr položek první a poslední smyčky")

    *Výběr položek první a poslední smyčky*
8. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **WebAndLoadTestProject** projektu, rozbalte položku **přidat** nabídky a vybereme **zátěžového testu...** .

    ![Zátěžový Test do projektu přidáte, WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "zátěžový Test do projektu přidáte, WebAndLoadTestProject")

    *Zátěžový Test do projektu přidáte, WebAndLoadTestProject*
9. V **Průvodce novým zátěžovým testem** dialogové okno, klikněte na tlačítko **Další**.

    ![Průvodce novým zátěžovým testem](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Průvodce novým zátěžovým testem")

    *Průvodce novým zátěžovým testem*
10. V **scénář** stránce **nepoužívat čas přemýšlení** a klikněte na tlačítko **Další**.

    ![Výběr nepoužívat čas přemýšlení](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "výběr nepoužívat čas přemýšlení")

    *Výběr nepoužívat čas přemýšlení*
11. V **vzor zatížení** stránky, ujistěte se, že **konstantní zatížení** je vybraná možnost. Změnit **počet uživatelů** nastavení **250** uživatele a klikněte na tlačítko **Další**.

    ![Změna počtu uživatelů na 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "změnu počtu uživatelů na 250")

    *Změna počtu uživatelů na 250*
12. V **testovat Model kombinace** stránce **na základě pořadí sekvenčního testu** a klikněte na tlačítko **Další**.

    ![Výběr modelu kombinace testů](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Výběr modelu kombinace testů")

    *Výběr modelu kombinace testů*
13. V **Model kombinace testů** klikněte na **přidat...**  přidat test do kombinaci.

    ![Přidání testu do poměru testů](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "přidání testu do poměru testů")

    *Přidání testu do poměru testů*
14. V **přidat testy** dialogové okno, klikněte dvakrát na **WebTest1** přidat test **vybrané testy** seznamu. Klikněte na tlačítko **OK** pokračujte.

    ![Přidat WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "přidání WebTest1 testu")

    *Přidat WebTest1 test*
15. Zpátky **poměru testů** klikněte na **Další**.

    ![Dokončení stránce poměru testů](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "dokončení stránce poměru testů")

    *Dokončení stránce poměru testů*
16. V **kombinaci sítí** klikněte na **Další**.

    ![Kliknete na další na stránce kombinaci sítí](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "kliknete na další na stránce poměr sítí")

    *Kliknutím na další stránce poměr sítí*
17. V **kombinace prohlížečů** stránce **Internet Explorer 10.0** jako typ prohlížeče a klikněte na **Další**.

    ![Výběr typu prohlížeče](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "vyberete typ prohlížeče")

    *Vyberte typ prohlížeče*
18. V **sady čítačů** klikněte na **Další**.

    ![Kliknutím na další stránce sady čítačů](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "kliknutím na další stránce sady čítačů")

    *Kliknutím na další stránce sady čítačů*
19. V **parametrů běhu** nastavte **trvání zátěžového testu** k **5 minut** a klikněte na tlačítko **Dokončit**.

    ![Nastavení trvání zátěžového testu na 5 minut](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "nastavení trvání zátěžového testu na 5 minut")

    *Nastavení trvání zátěžového testu na 5 minut*
20. V **Průzkumníka řešení**, dvakrát klikněte **Local.settings** souboru prozkoumat nastavení testu. Ve výchozím nastavení používá Visual Studio místního počítače ke spuštění testů.

    > [!NOTE]
    > Alternativně můžete nakonfigurovat testovacího projektu pro spuštění zátěžových testů v cloudu pomocí **Visual Studio Online (VSO)**. VSO poskytuje cloudové zátěžové testování služba, která simuluje zatížení realističtější, jak se vyhnout omezení místní prostředí, jako je kapacita procesoru, paměti a šířky pásma sítě. Další informace o spouštění zátěžových testů pomocí VSO, naleznete v tématu [v tomto článku](https://www.visualstudio.com/get-started/load-test-your-app-vs).

    ![Nastavení testu](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Úloha 3 – ověření automatického škálování

Nyní spusťte zátěžový test, který jste vytvořili v předchozí úloze a naleznete v tématu jak webové aplikace chová při zatížení.

1. V **Průzkumníka řešení**, dvakrát klikněte na panel **LoadTest1.loadtest** otevřete zátěžový test.

    ![Otevírání LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "otevírání LoadTest1.loadtest")

    *Otevírání LoadTest1.loadtest*
2. V **LoadTest1.loadtest** okna, klikněte na první tlačítko na panelu nástrojů ke spuštění zátěžového testu.

    ![Spuštění zátěžového testu](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "spuštění zátěžového testu")

    *Spuštění zátěžového testu*
3. Počkejte na dokončení zátěžového testu.

    > [!NOTE]
    > Zátěžový test simuluje několik uživatelů, kteří současně zasílat požadavky do webové aplikace. Když test běží, můžete sledovat dostupné čítače, které chcete zjistit všechny chyby, upozornění nebo jiné informace související se zátěžovým testem.

    ![Zátěžový test spuštěn](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "čekání, až po dokončení zátěžového testu")

    *Spuštění zátěžového testu*
4. Po dokončení testu, vraťte se do portálu pro správu a přejděte **škálování** stránce vaší webové aplikace. V části **kapacity** oddílu, měli byste vidět v grafu, že se automaticky nasadí novou instanci.

    ![Automaticky nasadit novou instanci](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Automaticky nasadit novou instanci*

    > [!NOTE]
    > Může trvat několik minut, než se změna projeví v grafu (stiskněte **CTRL + F5** pravidelně obnovíte stránku). Pokud se nezobrazí žádné změny, zkuste následující:
    > 
    > - Prodloužit dobu trvání zátěžového testu (například na **10 minut**)
    > - Omezit maximální a minimální hodnoty **cílový procesor** rozsah v konfiguraci automatického škálování webové aplikace
    > - Spusťte zátěžový test v cloudu s využitím **Visual Studio Online**. Další informace o [zde](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)

* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

V této praktické laboratoři jste zjistili, jak nastavit a nasadit aplikaci do produkční webové aplikace v Azure. Jste spustili pomocí detekce a aktualizaci vašich databází pomocí **migrace Entity Framework Code First**, pak navázat tak, že nasazování nových verzí vašeho webu pomocí **Git** a vrácení zpět k provádění nejnovější stabilní verze vaší lokality. Kromě toho jste zjistili, jak škálovat aplikace pomocí úložiště přesunout statického obsahu do kontejneru objektů Blob.
