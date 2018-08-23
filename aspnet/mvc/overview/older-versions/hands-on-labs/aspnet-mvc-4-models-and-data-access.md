---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 – modely a přístup k datům | Dokumentace Microsoftu
author: rick-anderson
description: 'Poznámka: Tento praktického testovacího prostředí se předpokládá, že máte základní znalosti ASP.NET MVC. Pokud jste ještě nepoužívali ASP.NET MVC před, doporučujeme si projít ASP.NET MVC 4...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 26896e6ee3c02e8f939296ecbfb8b7d500940765
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756716"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4 – modely a přístup k datům

podle [Campy Web týmu](https://twitter.com/webcamps)

[Stáhněte si Web Campy školení Kit](https://aka.ms/webcamps-training-kit)

Tohoto praktického testovacího prostředí předpokládá, že máte základní znalosti o **ASP.NET MVC**. Pokud jste ještě nepoužívali **ASP.NET MVC** před, doporučujeme si projít **základy ASP.NET MVC 4** praktického testovacího prostředí.

Tato laboratoř vás provede vylepšení a nových funkcí popsaných dříve použitím menší změny na ukázkovou webovou aplikaci ve zdrojové složce k dispozici.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 modely a přístup k datům](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

V **základy ASP.NET MVC** praktického testovacího prostředí, budete mít se předávání pevně zakódované dat z řadičů zobrazit šablony. Ale, aby bylo možné sestavit aplikace skutečný webu, můžete chtít použít skutečné databázi.

Použití databázového stroje, aby bylo možné ukládat a načítat data potřebná pro aplikace Music Store se zobrazí tohoto praktického testovacího prostředí. Chcete-li provést tuto akci, bude začínat existující databázi a vytvoření modelu Entity Data Model z něj. V tomto testovacím prostředí, se bude splňovat **Database First** přístup také **Code First** přístup.

Ale můžete také použít **první Model** přístup, vytvořit stejný model pomocí nástrojů a pak z něj vygenerovala databáze.

![První vs databáze. Model první](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. První model")

*První vs databáze. První model*

Po vygenerování modelu, budou v StoreController poskytnout dat získaných z databáze, místo použití pevně zakódované data Store zobrazení správné nastavení. Nebudete muset provádět změny do zobrazení šablon vzhledem k tomu, že StoreController se vrací stejný modely ViewModels do zobrazení šablon, ale tentokrát data budou přicházet z databáze.

**Přístupu Code First**

Přístupu Code First umožňuje definovat model z kódu bez generování třídy, které jsou obvykle svázány s použitím rozhraní framework.

V kódu nejprve modelu objekty jsou definovány pomocí POCOs, &quot;prostý starší objekty CLR&quot;. POCOs jsou jednoduché prostý třídy, které jste žádné dědičnosti a neimplementují rozhraní. Databázi jsme můžete vygenerovat automaticky z nich, nebo můžeme použít stávající databázi a generovat mapování tříd z kódu.

Výhody použití tohoto přístupu je, že zůstane modelu nezávisle na stálost framework (v tomto případě Entity Framework), jako POCOs třídy nejsou doplňuje rozhraní mapování.

> [!NOTE]
> Toto testovací prostředí je založena na technologii ASP.NET MVC 4 a verzi vzorové aplikace Music Store přizpůsobit a minimalizaci podle pouze funkce, které jsou zobrazené ve tohoto praktického testovacího prostředí.
> 
> Pokud chcete prozkoumat celé **Music Store** aplikace se nachází v [MVC. Music Store](https://github.com/evilDave/MVC-Music-Store).


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

Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha C:](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Podle následující praktická cvičení se skládá tohoto praktického testovacího prostředí:

1. [Cvičení 1: Přidání databáze](#Exercise1)
2. [Cvičení 2: Vytvoření databáze pomocí Code First](#Exercise2)
3. [Cvičení 3: Dotazování na databázi s parametry](#Exercise3)

> [!NOTE]
> Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.


Odhadovaný čas dokončení tohoto testovacího prostředí: **35 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Cvičení 1: Přidání databáze

V tomto cvičení se dozvíte, jak přidat databázi s tabulkami aplikace MusicStore k řešení, aby bylo možné využívat svoje data. Jakmile databáze je generována s modelem a přidán do řešení, upravíte třídy StoreController poskytli šablonu zobrazení s daty z databáze, místo pevně definovaných hodnot.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Úloha 1 – přidání databáze

V této úloze přidá již vytvořené databáze s hlavním tabulkami MusicStore aplikace do řešení.

1. Otevřít **začít** řešení nachází v **zdroj/Ex1-AddingADatabaseDBFirst/počáteční/** složky.

   1. Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Přidat **MvcMusicStore** databázový soubor. Ve tohoto praktického testovacího prostředí, budete používat již vytvořené databázi s názvem **MvcMusicStore.mdf**. Pokud chcete udělat, klikněte pravým tlačítkem na **aplikace\_Data** složku, přejděte na **přidat** a potom klikněte na tlačítko **existující položku**. Přejděte do **\Source\Assets** a vyberte **MvcMusicStore.mdf** souboru.

    ![Přidání existující položky](aspnet-mvc-4-models-and-data-access/_static/image2.png "přidat existující položku")

    *Přidat existující položku*

    ![Soubor databáze MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf databázový soubor")

    *MvcMusicStore.mdf databázový soubor*

    Databáze je přidaný do projektu. I v případě, že databáze nachází v řešení, můžete dotazovat a aktualizovat ji podle hostovaný v jiném databázovém serveru.

    ![MvcMusicStore databázi v Průzkumníku řešení](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore databázi v Průzkumníku řešení")

    *MvcMusicStore databázi v Průzkumníku řešení*
3. Ověření připojení k databázi. Chcete-li to provést, dvakrát klikněte na **MvcMusicStore.mdf** k navázání připojení.

    ![Připojení k MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "propojíte MvcMusicStore.mdf")

    *Připojení k MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Úloha 2 – Vytvoření datového modelu

V této úloze vytvoříte datový model pro interakci s databází přidali v předchozím úkolu.

1. Vytvoření datového modelu, která bude představovat databáze. Chcete-li to provést, v Průzkumníku řešení klikněte pravým tlačítkem **modely** složku, přejděte na příkaz **přidat** a potom klikněte na tlačítko **nová položka**. V **přidat novou položku** dialogového okna, vyberte **Data** šablony a potom **datový Model Entity ADO.NET** položky. Změňte název modelu dat na **StoreDB.edmx** a klikněte na tlačítko **přidat**.

    ![Přidání datového modelu ADO.NET Entity StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "přidání StoreDB ADO.NET Entity Data Model")

    *Přidání StoreDB ADO.NET Entity Data Model*
2. **Průvodce datovým modelem Entity** se zobrazí. Tento průvodce vás provede vytvořením modelu vrstvy. Protože model by měl vytvořit podle existující databáze recentyl přidali, vyberte **Generovat z databáze** a klikněte na tlačítko **Další**.

    ![Výběr obsahu modelu](aspnet-mvc-4-models-and-data-access/_static/image7.png "výběr obsahu modelu")

    *Výběr modelu obsahu*
3. Protože jsou generování modelu z databáze, je potřeba zadat připojení se má používat. Klikněte na tlačítko **nové připojení**.
4. Vyberte **soubor databáze Microsoft SQL Server** a klikněte na tlačítko **pokračovat**.

    ![Zvolit zdroj dat](aspnet-mvc-4-models-and-data-access/_static/image8.png "zvolit zdroj dat")

    *Vyberte zdroj dat dialogového okna*
5. Klikněte na tlačítko **Procházet** a vyberte databázi **MvcMusicStore.mdf** jste vyhledali v **aplikace\_Data** složky a klikněte na tlačítko **OK**.

    ![Vlastnosti připojení](aspnet-mvc-4-models-and-data-access/_static/image9.png "vlastnosti připojení")

    *Vlastnosti připojení*
6. Generované třídy by měly mít stejný název jako připojovací řetězec entity, proto změňte její název na **MusicStoreEntities** a klikněte na tlačítko **Další**.

    ![Výběr datové připojení](aspnet-mvc-4-models-and-data-access/_static/image10.png "volba připojení dat")

    *Výběr datové připojení*
7. Zvolte objekty databáze, které chcete použít. Jako Entity Model se použít jenom databázové tabulky, vyberte **tabulky** možnosti a ujistěte se, že **zahrnout do modelu sloupce cizích klíčů** a **Pluralize jednotné nebo množné číslo vygenerována názvy objektů** jsou vybrané možnosti. Změňte Model Namespace k **MvcMusicStore.Model** a klikněte na tlačítko **Dokončit**.

    ![Volba databázové objekty](aspnet-mvc-4-models-and-data-access/_static/image11.png "výběr databázových objektů")

    *Volba databázové objekty*

    > [!NOTE]
    > Pokud se zobrazí dialogové okno upozornění zabezpečení, klikněte na tlačítko **OK** danou šablonou spustit a generovat třídy pro model entity.
8. Diagramu entity pro databáze se zobrazí, když se vytvoří samostatné třídy, která mapuje každé tabulky do databáze. Například **alb** tabulky budou mít stejné **alba** třídy, ve kterém bude každý sloupec v tabulce mapovat na vlastnosti třídy. To vám umožní dotazovat a práci s objekty, které představují řádků v databázi.

    ![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")

    *Entity diagram*

    > [!NOTE]
    > Šablony T4 (.tt) spustit kód pro generování tříd entit a přepíše existující třídy se stejným názvem. V tomto příkladu třídy &quot;alba&quot;, &quot;žánr&quot; a &quot;interpreta&quot; byly přepsat vygenerovaný kód.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Úloha 3 – vytvoření aplikace

V této úloze bude zaškrtnete, i když generování modelu odebraly **alba**, **žánr** a **interpreta** tříd modelu projektu sestavení úspěšně pomocí nové tříd datových modelů.

1. Sestavte projekt výběrem **sestavení** položky nabídky a pak **sestavení MvcMusicStore**.

    ![Sestavení projektu](aspnet-mvc-4-models-and-data-access/_static/image13.png "sestavení projektu")

    *Sestavení projektu*
2. Úspěšně se sestavení projektu. Proč to pořád funguje? To funguje, protože databázové tabulky obsahuje pole, která zahrnují vlastnosti, které jste používali ve třídách odebrané **alba** a **žánr**.

    ![Sestavení proběhlo úspěšně](aspnet-mvc-4-models-and-data-access/_static/image14.png "sestavení proběhlo úspěšně")

    *Sestavení bylo úspěšné.*
3. Návrhář zobrazí entity ve formátu diagramu, ale jsou ve skutečnosti třídy jazyka C#. Rozbalte **StoreDB.edmx** uzlu v Průzkumníku řešení a potom **StoreDB.tt**, zobrazí se nové entity vygenerované.

    ![Generované soubory](aspnet-mvc-4-models-and-data-access/_static/image15.png "generovaných souborů")

    *Generované soubory*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Úloha 4 – dotazování na databázi

V této úloze budete aktualizovat StoreController třídy tak, aby místo použití pevně zakódované data, dotáže se na databázi a načte informace.

1. Otevřít **Controllers\StoreController.cs** a přidejte následující pole do třídy pro uložení instance **MusicStoreEntities** třídu s názvem **storeDB**:

    (Fragment - kódu *modely a přístup k datům – Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. **MusicStoreEntities** třída zveřejňuje vlastnosti kolekce pro každou tabulku v databázi. Aktualizace **Procházet** metody akce k načtení žánr se všemi **alb**.

    (Fragment - kódu *modely a přístup k datům – procházení Store Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Používáte funkce .NET volá **LINQ** (jazyka integrované dotazu) k zápisu výrazy dotazu silného typu pomocí těchto kolekcí – které spustí kód na databázi a vrátí objekty, které můžete naprogramovat proti.
    > 
    > Další informace o dotazech technologie LINQ, navštivte prosím [web msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Aktualizace **Index** metody akce k načtení všech žánrů.

    (Fragment - kódu *modely a přístup k datům – Ex1 Store Index*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Aktualizace **Index** metody akce k načtení všech žánry a transformovat kolekci do seznamu.

    (Fragment - kódu *modely a přístup k datům – Ex1 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Úloha 5: spuštění aplikace

V této úloze zkontroluje, že Store indexovou stránku se teď budou zobrazovat žánry uložených v databázi namísto pevně zakódované značky. Není nutné měnit zobrazit šablonu, protože **StoreController** vrací stejné entity jako předtím, ale tentokrát data budou přicházet z databáze.

1. Znovu sestavte řešení a stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Ověřte, že v nabídce **žánry** už není pevně zakódované seznamu a data se načtou přímo z databáze.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Procházení žánry z databáze*
3. Teď přejděte do jakékoli žánr a ověřte, že že alb budou naplněny z databáze.

    ![Z databáze procházení alb](aspnet-mvc-4-models-and-data-access/_static/image17.png "procházení alb z databáze")

    *Procházení alb z databáze*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Cvičení 2: Vytvoření databáze nejprve pomocí kódu

V tomto cvičení se dozvíte, jak vytvořit databázi s tabulkami aplikace MusicStore pomocí přístupu Code First a jak přistupovat k jeho datům.

Po vygenerování modelu upravíte StoreController poskytnout dat získaných z databáze, místo hodnot pevně zakódované zobrazit šablonu.

> [!NOTE]
> Pokud dokončení cvičení 1 a už pracovali s Database First přístup, se teď naučíte stejné výsledky s jiným procesem. Úlohy, které jsou společné s cvičení 1 byly označeny pro usnadnění vaší čtení. Pokud jste nedokončili vykonávat 1 ale chtěli dozvědět přístupu Code First, můžete spustit z tohoto cvičení a získat úplné vysvětlení tohoto tématu.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Úloha 1 – naplnění ukázkových dat

V této úloze naplníte databázi s ukázkovými daty při výchozímu vytvořené využitím založeno na kódu.

1. Otevřít **začít** řešení nachází v **zdroj/Ex2-CreatingADatabaseCodeFirst/počáteční/** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Přidat **SampleData.cs** do souboru **modely** složky. Pokud chcete udělat, klikněte pravým tlačítkem na **modely** složku, přejděte na příkaz **přidat** a potom klikněte na tlačítko **existující položku**. Přejděte do **\Source\Assets** a vyberte **SampleData.cs** souboru.

    ![Ukázková data naplnit kód](aspnet-mvc-4-models-and-data-access/_static/image18.png "kód naplnění ukázkových dat")

    *Ukázková data naplnit kódu*
3. Otevřít **Global.asax.cs** soubor a přidejte následující *pomocí* příkazy.

    (Fragment - kódu *modely a přístup k datům – Ex2 globální direktivy using Asax*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. V **aplikace\_Start()** metody přidejte následující řádek nastavit inicializátor databáze.

    (Fragment - kódu *modely a přístup k datům – globální Asax SetInitializer Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Úloha 2 – konfigurace připojení k databázi

Teď, když jste už přidali do databáze na našem projektu, budete psát **Web.config** souboru připojovací řetězec.

1. Přidat připojovací řetězec na **Web.config**. Chcete-li to mohli udělat, otevřete **Web.config** v kořenu projektu a nahraďte připojovací řetězec s názvem možnost DefaultConnection se tento řádek v **&lt;connectionStrings&gt;** části:

    ![Umístění souboru Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "umístění souboru Web.config")

    *Umístění souboru Web.config*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Úloha 3 – práce s modelem

Teď, když jste už nakonfigurovali připojení k databázi, bude propojovat modelu s tabulkami databáze. V této úloze vytvoříte třídu, která se propojí s databáze s Code First. Mějte na paměti, že je existující třídy modelu POCO, který by měl být upraven.

> [!NOTE]
> Pokud jste dokončili cvičení 1, Všimněte si, že byl tento krok provést pomocí průvodce. Tímto způsobem Code First, vytvořte ručně třídy, které budou připojeny k datovým entitám.

1. Otevřete třídu modelu POCO **žánr** z **modely** složce projektu a obsahovat identifikátor. Použijte int vlastnost s názvem **GenreId**.

    (Fragment - kódu *modely a přístup k datům – první žánr Ex2 kód*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Pro práci s Code First vytváření názvů, třída rozšířením podle tematických musí mít vlastnost primárního klíče, který bude automaticky zjistit.
    > 
    > Další informace o první konvence kódu v tomto [článku na webu msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. Nyní, otevřete třídu modelu POCO **alba** z **modely** složce projektu a zahrnují cizí klíče, vytvoření vlastnosti s názvy **GenreId** a  **ArtistId**. Tato třída již **GenreId** pro primární klíč.

    (Fragment - kódu *modely a přístup k datům – první alba Ex2 kód*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Otevřete třídu modelu POCO **interpreta** a zahrnout **ArtistId** vlastnost.

    (Fragment - kódu *modely a přístup k datům – první interpreta Ex2 kód*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Klikněte pravým tlačítkem myši **modely** složky projektu a vyberte **přidat | Třída**. Název souboru **MusicStoreEntities.cs**. Potom klikněte na **přidat.**

    ![Přidání třídy](aspnet-mvc-4-models-and-data-access/_static/image20.png "přidání třídy")

    *Přidání nové položky*

    ![Přidávání class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "přidávání class2")

    *Přidání třídy*
5. Otevřete třídu, kterou jste právě vytvořili, **MusicStoreEntities.cs**a obory názvů **System.Data.Entity** a **System.Data.Entity.Infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Nahraďte deklaraci třídy pro rozšíření **DbContext** třídy: deklarovat veřejnou **DBSet** a přepsat **OnModelCreating** metody. Po provedení tohoto kroku získáte doménovou třídou, která propojí váš model s rozhraním Entity Framework. Aby bylo možné provést, nahraďte kód třídy následujícími způsoby:

    (Fragment - kódu *modely a přístup k datům – první MusicStoreEntities kód Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> S Entity Framework **DbContext** a **DBSet** budete moci dotaz na třídu POCO žánr. Tím, že rozšíří **OnModelCreating** metoda, zadáváte v **kód** mapovány žánr do databázové tabulky. Další informace o kontext databáze a DBSet najdete v článku na webu msdn: [odkaz](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Úloha 4 – dotazování na databázi

V této úloze budete aktualizovat StoreController třídy tak, aby místo použití pevně zakódované dat, načte se z databáze.

> [!NOTE]
> Tato úloha je společné s cvičení 1.
> 
> Pokud jste dokončili cvičení 1 se nezapomeňte tyto kroky jsou stejné v obou metod (první databáze nebo kód nejprve). Jsou odlišné v jak se data propojené s modelem, ale přístup k datovým entitám se ještě transparentní z kontroleru.


1. Otevřít **Controllers\StoreController.cs** a přidejte následující pole do třídy pro uložení instance **MusicStoreEntities** třídu s názvem **storeDB**:

    (Fragment - kódu *modely a přístup k datům – Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. **MusicStoreEntities** třída zveřejňuje vlastnosti kolekce pro každou tabulku v databázi. Aktualizace **Procházet** metody akce k načtení žánr se všemi **alb**.

    (Fragment - kódu *modely a přístup k datům – procházení Store Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Používáte funkce .NET volá **LINQ** (jazyka integrované dotazu) k zápisu výrazy dotazu silného typu pomocí těchto kolekcí – které spustí kód na databázi a vrátí objekty, které můžete naprogramovat proti.
    > 
    > Další informace o dotazech technologie LINQ, navštivte prosím [web msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Aktualizace **Index** metody akce k načtení všech žánrů.

    (Fragment - kódu *modely a přístup k datům – Ex2 Store Index*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Aktualizace **Index** metody akce k načtení všech žánry a transformovat kolekci do seznamu.

    (Fragment - kódu *modely a přístup k datům – Ex2 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Úloha 5: spuštění aplikace

V této úloze zkontroluje, že Store indexovou stránku se teď budou zobrazovat žánry uložených v databázi namísto pevně zakódované značky. Není nutné měnit zobrazit šablonu, protože **StoreController** vrací stejný **StoreIndexViewModel** stejně jako předtím, ale tentokrát budou data přicházet z databáze.

1. Znovu sestavte řešení a stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Ověřte, že v nabídce **žánry** už není pevně zakódované seznamu a data se načtou přímo z databáze.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Procházení žánry z databáze*
3. Teď přejděte do jakékoli žánr a ověřte, že že alb budou naplněny z databáze.

    ![Z databáze procházení alb](aspnet-mvc-4-models-and-data-access/_static/image23.png "procházení alb z databáze")

    *Procházení alb z databáze*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Cvičení 3: Dotazování na databázi s parametry

V tomto cvičení se dozvíte, jak zadávat dotazy na databázi pomocí parametrů a jak používat tvarování výsledku dotazu, funkce, která snižuje počet databázi přistupuje k načítání dat v efektivnější.

> [!NOTE]
> Další informace o strukturování výsledku dotazu, navštivte následující [článku na webu msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Úloha 1 - změna StoreController alb načíst z databáze

V této úloze se změní **StoreController** třídy pro přístup do databáze načíst z konkrétní žánr alb.

1. Otevřít **začít** umístění řešení **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** složky, pokud chcete použít přístup Code-First nebo **Source\ EX3. QueryingTheDatabaseWithParametersDBFirst\Begin** složky, pokud chcete použít první databázi přístup. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřít **StoreController** třídy změnit **Procházet** metody akce. Chcete-li to provést, v **Průzkumníka řešení**, rozbalte **řadiče** složky a dvojím kliknutím **StoreController.cs**.
3. Změnit **Procházet** metody akce k načtení alb pro konkrétní žánr. Chcete-li to provést, nahraďte následujícím kódem:

    (Fragment - kódu *modely a přístup k datům – EX3. StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> K naplnění kolekce entit, budete muset použít **zahrnout** metodu k určení, které chcete načíst alb příliš. Můžete použít. **Metoda Single()** rozšíření v technologii LINQ vzhledem k tomu, že v tomto případě se očekává pouze jeden žánr pro alba. **Metoda Single()** metoda má výraz Lambda jako parametr, který v tomto případě Určuje jeden objekt žánr tak, aby jeho název odpovídá hodnota definovaná.
> 
> Bude využít výhod funkce, která umožňuje určit další související entity, které chcete načíst i když je načten objekt žánr. Tato funkce se nazývá **tvarování výsledek dotazu**a umožňuje zkrátit dobu potřebnou pro přístup do databáze k načtení informací. V tomto scénáři budete chtít předběžného načítání pro žánr načtete alb.
> 
> Dotaz obsahuje **Genres.Include (&quot;alb&quot;)** k označení, má také související alb. Výsledkem bude aplikace efektivnější, protože ho se načtou data žánr a alb v požadavku izolované databáze.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Úloha 2 – spuštění aplikace

V této úloze se spustit aplikaci a alb konkrétní žánr načíst z databáze.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/Store/procházet? žánr = Pop** k ověření, že se výsledky načtou z databáze.

    ![Procházení podle žánru](aspnet-mvc-4-models-and-data-access/_static/image24.png "procházením podle žánru")

    *Procházení/Store/procházet? žánr = Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Úloha 3 – přístup k alb podle Id

V této úloze bude opakujte předchozí postup pro získání alb podle jejich Id.

1. Zavřete prohlížeč v případě potřeby se vraťte do sady Visual Studio. Otevřít **StoreController** třídy změnit **podrobnosti** metody akce. Chcete-li to provést, v **Průzkumníka řešení**, rozbalte **řadiče** složky a dvojím kliknutím **StoreController.cs**.
2. Změnit **podrobnosti** metody akce k načtení podrobností alb na základě jejich **Id**. Chcete-li to provést, nahraďte následujícím kódem:

    (Fragment - kódu *modely a přístup k datům – EX3. StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Úloha 4 – spouštění aplikace

V této úloze spusťte aplikaci ve webovém prohlížeči a získání podrobných informací o alba podle jejich Id.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/Store/Details/51** nebo žánry procházením vyberte alba k ověření, že se výsledky načtou z databáze.

    ![Procházení podrobností](aspnet-mvc-4-models-and-data-access/_static/image25.png "procházení podrobností")

    *Procházení /Store/Details/51*

> [!NOTE]
> Kromě toho můžete nasadit tuto aplikaci následující weby Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu](#AppendixB).

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Za dokončení tohoto praktického testovacího prostředí jste se naučili základy ASP.NET MVC modely a přístup k datům, pomocí **Database First** přístup také **Code First** přístup:

- Jak do řešení přidat databáze, aby bylo možné využívat svoje data
- Postup aktualizace řadiče poskytnout dat získaných z databáze místo pevně zakódované zobrazení šablony
- Jak dotazovat databázi pomocí parametrů
- Použití dotazu výsledek tvarování, funkce, která snižuje počet přístupů databáze, načítání dat v efektivnější způsob
- Použití Database First a Code First pracuje Microsoft Entity Framework, která propojí databázi s modelem

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.

    ![Instalace sady Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "instalace sady Visual Studio Express")

    *Instalace sady Visual Studio Express*
4. Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Přijetí podmínek licence](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Přijetí podmínek licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Průběh instalace*
6. Až instalace skončí, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.

    ![VS Express for Web dlaždice](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    *VS Express for Web dlaždice*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu

Tento dodatek se ukazují, jak vytvořit nový web z portálu správy Windows Azure a publikovat aplikace, kterou jste získali podle testovacího prostředí, využít Webdeploy funkce publikování ve Windows Azure k dispozici.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Úloha 1 – Vytvoření nového webu z Windows webu Azure Portal

1. Přejděte [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů Microsoft spojených s vaším předplatným.

    > [!NOTE]
    > Windows Azure můžete zadarmo hostovat 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu. Můžete se zaregistrovat [tady](http://aka.ms/aspnet-hol-azure).

    ![Přihlaste se k portálu Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k portálu pro správu Azure Windows*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](aspnet-mvc-4-models-and-data-access/_static/image32.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **Compute** | **webu**. Potom vyberte **rychlé vytvoření** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvořit web**.

    > [!NOTE]
    > Hostitel pro webovou aplikaci spuštěnou v cloudu, který může řídit a spravovat je web Windows Azure. Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál. Nezahrnuje kroky pro vytvoření databáze.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](aspnet-mvc-4-models-and-data-access/_static/image33.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** se vytvoří.
5. Po vytvoření webové stránky, klikněte na odkaz v části **URL** sloupce. Zkontrolujte, jestli funguje nový web.

    ![Na nový web](aspnet-mvc-4-models-and-data-access/_static/image34.png "přechodu na nový web")

    *Procházení na nový web*

    ![Spuštění webu](aspnet-mvc-4-models-and-data-access/_static/image35.png "spuštění webové stránky")

    *Spuštění webové stránky*
6. Přejděte zpět na portál a klikněte na název webové stránky v části **název** sloupec, který se zobrazí na stránkách správy.

    ![Otevřete správu webových stránek](aspnet-mvc-4-models-and-data-access/_static/image36.png "otevřete správu webových stránek")

    *Otevřete správu webových stránek*
7. V **řídicí panel** stránce v části **rychlý přehled** klikněte na tlačítko **stáhnout profil publikování** odkaz.

    > [!NOTE]
    > *Profil publikování* obsahuje všechny informace požadované pro publikování webových aplikací na webu Windows Azure pro každou metodu povoleno publikování. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a databázové řetězce požadované k připojení a ověřování na základě jednotlivých koncových bodů, u kterých je povolena metoda publikace. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** a **Microsoft Visual Studio 2012** podporují čtení publikační profily k automatizaci konfigurace z těchto programů pro publikování webových aplikací na Windows Azure websites.

    ![Stahování webové stránky publikovat profil](aspnet-mvc-4-models-and-data-access/_static/image37.png "stahování webové stránky profil publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profilu publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak publikovat webovou aplikaci na weby Windows Azure ze sady Visual Studio pomocí tohoto souboru.

    ![Ukládání souboru profilu publikování](aspnet-mvc-4-models-and-data-access/_static/image38.png "ukládá se profil publikování")

    *Ukládá se profil publikování*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá SQL Server databáze, budete muset vytvořit server služby SQL Database. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá SQL Server může tuto úlohu přeskočit.

1. Pro uložení databáze aplikace budete potřebovat databázi SQL serveru. Servery SQL Database můžete zobrazit ze svého předplatného na portálu správy Windows Azure na **databází Sql** | **servery** | **serveru Řídicí panel**. Pokud nemáte server vytvořili, můžete vytvořit jednu **přidat** tlačítko na panelu příkazů. Poznamenejte si **název serveru a adresu URL, správce přihlašovací jméno a heslo**, jak je použijete v další úkoly. Nevytvářet databáze, protože vytvoří se v pozdější fázi.

    ![Řídicí panel serveru SQL Database](aspnet-mvc-4-models-and-data-access/_static/image39.png "řídicího panelu serveru SQL Database")

    *Řídicí panel serveru SQL Database*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio z tohoto důvodu je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. Chcete-li to mohli udělat, klikněte na tlačítko **konfigurovat**, vyberte IP adresu z **aktuální IP adresa klienta** a vložte ho na **počáteční IP adresa** a **Koncová IP adresa** textová pole a klikněte na tlačítko ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) tlačítko.

    ![Přidat IP adresu klienta](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Přidat IP adresu klienta*
3. Jednou **IP adresa klienta** je přidat do povolených IP adres klikněte na tlačítko na **Uložit** potvrďte provedené změny.

    ![Potvrzení změn](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Potvrzení změn*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nasazení webu

1. Vraťte se do řešení ASP.NET MVC 4. V **Průzkumníka řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](aspnet-mvc-4-models-and-data-access/_static/image43.png "publikování aplikace")

    *Publikování na webu*
2. Importujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](aspnet-mvc-4-models-and-data-access/_static/image44.png "import profilu publikování")

    *Import publikačního profilu*
3. Klikněte na tlačítko **ověřit připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření bylo dokončeno, jakmile se zobrazí zelené zaškrtnutí vedle tlačítka ověřit připojení.

    ![Ověřuje se připojení](aspnet-mvc-4-models-and-data-access/_static/image45.png "ověřuje se připojení")

    *Ověřuje se připojení*
4. V **nastavení** stránce v části **databází** klikněte na tlačítko vedle textového pole připojení k databázi (to znamená **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](aspnet-mvc-4-models-and-data-access/_static/image46.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Konfigurace připojení k databázi následujícím způsobem:

   - V **název serveru** zadejte vaše pomocí adresy URL databáze SQL serveru *tcp:* předponu.
   - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
   - V **heslo** zadejte heslo pro přihlašovací jméno správce serveru.
   - Zadejte nový název databáze.

     ![Konfigurace cílový připojovací řetězec](aspnet-mvc-4-models-and-data-access/_static/image47.png "konfigurace cílový připojovací řetězec")

     *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze klikněte na **Ano**.

    ![Vytvoření databáze](aspnet-mvc-4-models-and-data-access/_static/image48.png "vytvoření řetězce databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL ve službě Windows Azure se zobrazí v textovém poli výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "připojovací řetězec odkazující na SQL Database")

    *Připojovací řetězec odkazující na SQL Database*
8. V **ve verzi Preview** klikněte na **publikovat**.

    ![Publikování webové aplikace](aspnet-mvc-4-models-and-data-access/_static/image50.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Až se proces publikování dokončí, otevře se váš výchozí prohlížeč publikovaného webu.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Příloha C: používání fragmentů kódu

Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky. Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-models-and-data-access/_static/image51.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")

*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.
4. Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).
5. Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.

![Začněte psát název fragmentu kódu](aspnet-mvc-4-models-and-data-access/_static/image52.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](aspnet-mvc-4-models-and-data-access/_static/image53.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")

*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*

![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](aspnet-mvc-4-models-and-data-access/_static/image54.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")

*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.
2. Kliknutím na vyberte relevantní fragment kódu ze seznamu.

![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-models-and-data-access/_static/image55.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-models-and-data-access/_static/image56.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")

*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*
