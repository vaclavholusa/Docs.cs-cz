---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 modely a přístup k datům | Microsoft Docs
author: rick-anderson
description: 'Poznámka: Toto testovací prostředí Hands-on předpokládá, že máte základní znalosti o architektuře ASP.NET MVC. Pokud jste nepoužili ASP.NET MVC před, doporučujeme si projít ASP.NET MVC 4...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 081a71ef67a6eee6c84058c30f9e15301afbed23
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>ASP.NET MVC 4 modely a přístup k datům

Podle [webové táborech Team](https://twitter.com/webcamps)

[Stažení webové táborech cvičení Kit](https://aka.ms/webcamps-training-kit)

Toto testovací prostředí Hands-on předpokládá, že máte základní znalosti o **ASP.NET MVC**. Pokud jste nepoužili **ASP.NET MVC** před, doporučujeme si projít **ASP.NET MVC 4 Základy** Hands-on testovacího prostředí.

Tato laboratoř vás provede procesem vylepšení a nových funkcí popsaných výše použitím malých změn na ukázkové webové aplikaci ve zdrojové složce zadané.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [verze Microsoft-webové/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 modely a přístup k datům](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

V **ASP.NET MVC Základy** Hands-on testovací prostředí, můžete mít byla předávání pevně dat z řadičů šablon zobrazení. Ale, aby bylo možné vytvořit skutečné webové aplikace, můžete chtít použít skutečné databázi.

Toto testovací prostředí Hands-on vám ukáže, jak používat databázový stroj, aby bylo možné ukládají a načítají data potřebná pro aplikaci služby obchodu Hudba. Provést tuto akci, bude začínat existující databázi a z něj vytvořit datového modelu Entity. V tomto testovacím prostředí bude vyhovovat **Database First** přístup a taky **Code First** přístup.

Ale můžete také použít **Model First** přístupu, vytvořte stejný model pomocí nástroje a potom z něj generovat databázi.

![První vs databáze. Model první](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Nejprve modelu")

*První vs databáze. Nejprve modelu*

Po generování modelu, budou správné úpravy v StoreController poskytnout zobrazení úložiště dat získaných z databáze, místo použití pevně data. Nebudete muset provádět všechny změny šablony zobrazení protože StoreController se vrátí stejné ViewModels zobrazení šablony, ale tentokrát data budou pocházet z databáze.

**Přístupu Code First**

Code First přístup umožňuje definovat model z kódu bez generování tříd, které jsou obecně kombinaci s rozhraní.

V kódu nejprve objekty modelu jsou definovány s POCOs, &quot;prostý staré objekty CLR&quot;. POCOs jsou jednoduché prostý třídy, které mají žádné dědičnosti a neimplementuje rozhraní. Jsme může automaticky generovat databázi z nich nebo můžeme použít existující databázi a generovat mapování třídy z kódu.

Výhody použití tohoto přístupu je, aby zůstala modelu nezávisle trvalost framework (v tomto případě Entity Framework), jak POCOs třídy nejsou kombinaci s rozhraní mapování.

> [!NOTE]
> Toto testovací prostředí je založena na technologii ASP.NET MVC 4 a verzi Hudba úložiště ukázkovou aplikaci přizpůsobit a minimalizuje podle pouze funkce uvedené v tomto testovacím prostředí Hands-On.
> 
> Pokud chcete prozkoumat celek **Hudba úložiště** aplikace najdete ji v [MVC. Hudba úložiště](https://github.com/evilDave/MVC-Music-Store).


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

Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha C:](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto testovací prostředí Hands-on se skládá ve cvičeních následující:

1. [Cvičení 1: Přidání databáze](#Exercise1)
2. [Cvičení 2: Vytvoření databáze pomocí Code First](#Exercise2)
3. [Cvičení 3: Dotaz na databázi s parametry](#Exercise3)

> [!NOTE]
> Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.


Odhadovaný čas dokončení tohoto testovacího prostředí: **35 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Cvičení 1: Přidání databáze

V tomto cvičení se dozvíte, jak přidat databáze s tabulkami aplikace MusicStore k řešení, aby bylo možné využívat jeho data. Jakmile databáze je generována s modelem a k řešení přidat, upraví třídě StoreController zobrazit šablonu poskytnout dat získaných z databáze, místo pevně definovaných hodnot.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Úloha 1 – přidání databáze

V této úloze budete přidávat již vytvořené databáze s hlavní tabulky MusicStore aplikace k řešení.

1. Otevřete **začít** řešení nacházející se v **zdroj/Ex1-AddingADatabaseDBFirst/počáteční/** složky.

   1. Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Přidat **MvcMusicStore** soubor databáze. V tomto testovacím prostředí Hands-on budete používat již vytvořené databáze názvem **MvcMusicStore.mdf**. To lze provést, klikněte pravým tlačítkem na **aplikace\_Data** složku, přejděte na příkaz **přidat** a pak klikněte na **existující položka**. Přejděte do **\Source\Assets** a vyberte **MvcMusicStore.mdf** souboru.

    ![Přidat existující položku](aspnet-mvc-4-models-and-data-access/_static/image2.png "přidání existující položky")

    *Přidání existující položky*

    ![Soubor databáze MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf databázový soubor")

    *Soubor databáze MvcMusicStore.mdf*

    Databáze byla přidána do projektu. I v případě, že databáze se nachází uvnitř řešení, se můžete dotazovat a aktualizovat ji jako byla hostované v jiné databázi serveru.

    ![MvcMusicStore databáze v Průzkumníku řešení](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore databáze v Průzkumníku řešení")

    *MvcMusicStore databáze v Průzkumníku řešení*
3. Ověřte připojení k databázi. Chcete-li to provést, dvakrát klikněte na **MvcMusicStore.mdf** k navázání připojení.

    ![Připojení k MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "připojení k MvcMusicStore.mdf")

    *Připojení k MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Úloha 2 – vytváření datového modelu

V této úloze vytvoří datový model pro interakci s databází přidali v předchozí úloze.

1. Vytvoření datového modelu, která bude představovat databáze. Chcete-li to provést, v Průzkumníku řešení klikněte pravým tlačítkem **modely** složku, přejděte na příkaz **přidat** a pak klikněte na **novou položku**. V **přidat novou položku** dialogovém okně, vyberte **Data** šablony a potom **ADO.NET Entity Data Model** položky. Změňte název modelu dat **StoreDB.edmx** a klikněte na tlačítko **přidat**.

    ![Přidání StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "přidání StoreDB ADO.NET Entity Data Model")

    *Přidání StoreDB ADO.NET Entity Data Model*
2. **Entity Data Model Wizard** se zobrazí. Tento průvodce vás provede vytvoření vrstvy modelu. Vzhledem k tomu, že model by měl být na základě vytvořit existující databáze recentyl přidat, vyberte **generování z databáze** a klikněte na tlačítko **Další**.

    ![Výběr obsah modelu](aspnet-mvc-4-models-and-data-access/_static/image7.png "výběr obsah modelu")

    *Výběr obsah modelu*
3. Vzhledem k tomu, že chcete generovat model z databáze, musíte zadat připojení používat. Klikněte na tlačítko **nové připojení**.
4. Vyberte **soubor databáze Microsoft SQL Server** a klikněte na tlačítko **pokračovat**.

    ![Vyberte zdroj dat](aspnet-mvc-4-models-and-data-access/_static/image8.png "vybrat zdroj dat")

    *Vyberte zdroj dat dialogové okno*
5. Klikněte na tlačítko **Procházet** a vyberte databázi **MvcMusicStore.mdf** jste vyhledali v **aplikace\_Data** složky a klikněte na tlačítko **OK**.

    ![Vlastnosti připojení](aspnet-mvc-4-models-and-data-access/_static/image9.png "vlastnosti připojení")

    *Vlastnosti připojení*
6. Generovaná třída by měla mít stejný název jako připojovací řetězec entity, takže změňte její název **MusicStoreEntities** a klikněte na tlačítko **Další**.

    ![Výběr datové připojení](aspnet-mvc-4-models-and-data-access/_static/image10.png "vybrat datové připojení")

    *Výběr datové připojení*
7. Vyberte objekty databáze, které chcete použít. Entity Model používat jenom databázové tabulky, vyberte **tabulky** možnost a ujistěte se, že **zahrnout do modelu sloupce cizích klíčů** a **množně nebo singularizovat generované názvy objektů** jsou vybrané možnosti. Změňte Model Namespace k **MvcMusicStore.Model** a klikněte na tlačítko **Dokončit**.

    ![Výběr databázové objekty](aspnet-mvc-4-models-and-data-access/_static/image11.png "výběr databázové objekty")

    *Výběr databázové objekty*

    > [!NOTE]
    > Pokud se zobrazí dialogové okno upozornění zabezpečení, klikněte na tlačítko **OK** spustit šablonu a vygenerovat třídy pro model entity.
8. Diagramu entity pro databázi se zobrazí, když se vytvoří samostatnou třídu, která mapuje každou tabulku do databáze. Například **alb** budou odpovídat tabulky **Album** třídy, kde bude každý sloupec v tabulce mapovat vlastnosti třídy. To vám umožní pro dotazování a práce s objekty, které představují řádků v databázi.

    ![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagramu Entity")

    *Entity diagram*

    > [!NOTE]
    > Šablony T4 (.tt) spustit kód vygenerovat třídy entity a přepíše existující třídy se stejným názvem. V tomto příkladu třídy &quot;Album&quot;, &quot;Genre&quot; a &quot;umělcem&quot; měla přepsat generovaného kódu.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Úloha 3 – vytvoření aplikace

V této úloze bude zaškrtnete, i když generování modelu odebraly **Album**, **Genre** a **umělcem** třídy modelu, sestavení projektu úspěšně pomocí nové třídy datového modelu.

1. Sestavení projektu výběrem **sestavení** položky nabídky a pak **sestavení MvcMusicStore**.

    ![Sestavení projektu](aspnet-mvc-4-models-and-data-access/_static/image13.png "sestavení projektu")

    *Sestavení projektu*
2. Sestavení projektu úspěšně. Proč to stále funguje? Funguje, protože tabulky databáze, které mají pole obsahující vlastnosti, které jste používali v odstraněné třídy **Album** a **Genre**.

    ![Sestavení bylo úspěšné](aspnet-mvc-4-models-and-data-access/_static/image14.png "sestavení bylo úspěšné")

    *Sestavení bylo úspěšné*
3. Při návrháře zobrazí entity ve formátu diagramu, jsou skutečně třídy jazyka C#. Rozbalte **StoreDB.edmx** uzlu v Průzkumníku řešení a potom **StoreDB.tt**, zobrazí se nové generovaného entity.

    ![Generované soubory](aspnet-mvc-4-models-and-data-access/_static/image15.png "generované soubory")

    *Generované soubory*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Úloha 4 – dotazování databáze

V této úloze tak, aby místo použití pevně zakódované data, se bude dotazovat databáze načíst informace o aktualizujte StoreController třídy.

1. Otevřete **Controllers\StoreController.cs** a přidejte následující pole do třídy pro uložení instance **MusicStoreEntities** třídy s názvem **storeDB**:

    (Code fragment kódu - *modely a přístup k datům - Ex1 storeDB*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
~~~
2. **MusicStoreEntities** třída zpřístupňuje vlastnost kolekce pro každou tabulku v databázi. Aktualizace **Procházet** metody akce k načtení Genre se všemi **alb**.

    (Code fragment kódu - *modely a přístup k datům - Ex1 úložiště Procházet*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
~~~
3. Aktualizace **Index** metody akce k načtení všech žánry.

    (Code fragment kódu - *modely a Data Access – Index úložiště Ex1*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
~~~
4. Aktualizace **Index** metody akce k načtení všech žánry a transformace kolekce do seznamu.

    (Code fragment kódu - *modely a přístup k datům - Ex1 úložiště GenreMenu*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]
~~~

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Úloha 5: spuštění aplikace

V této úloze zkontroluje se, že na stránce Index úložiště se nyní zobrazí žánry uloženy v databázi místo pevně zakódované ty, které jsou. Není třeba měnit zobrazit šablonu, protože **StoreController** vrací stejné entity jako předtím, ale tentokrát data budou pocházet z databáze.

1. Znovu sestavte řešení a stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Ověřte, že v nabídce **žánry** již není seznamu pevně zakódované a přímo načtena data z databáze.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Procházení žánry z databáze*
3. Nyní přejděte do jakékoli genre a ověřte, zda že alb budou naplněny z databáze.

    ![Procházení alb z databáze](aspnet-mvc-4-models-and-data-access/_static/image17.png "procházení alb z databáze")

    *Procházení alb z databáze*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Cvičení 2: Vytvoření databáze nejdřív pomocí kódu

V tomto cvičení se dozvíte, jak použít Code First přístup k vytvoření databáze s tabulkami aplikace MusicStore a jak pro přístup k jeho data.

Po vygenerování modelu upravíte StoreController poskytnout dat získaných z databáze, místo použití hodnot pevně zakódované zobrazit šablonu.

> [!NOTE]
> Pokud jste dokončili cvičení 1 a už pracovali s databáze prvním přístupem, se teď Další informace o získání stejné výsledky s jiným procesem. Úlohy, které jsou společné s cvičení 1 bylo označeno k usnadnění vaší čtení. Pokud jste nedokončili prověření 1, ale chtěli dozvědět Code First přístup, můžete spustit z tohoto cvičení a získat úplné vysvětlení tohoto tématu.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Úloha 1 - naplnění ukázková Data

V této úloze bude naplnit databázi s ukázkovými daty při výchozímu vytvoření pomocí Code First.

1. Otevřete **začít** řešení nacházející se v **zdroj/Ex2-CreatingADatabaseCodeFirst/počáteční/** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Přidat **SampleData.cs** do souboru **modely** složky. To lze provést, klikněte pravým tlačítkem na **modely** složku, přejděte na příkaz **přidat** a pak klikněte na **existující položka**. Přejděte do **\Source\Assets** a vyberte **SampleData.cs** souboru.

    ![Ukázková data naplnit kódu](aspnet-mvc-4-models-and-data-access/_static/image18.png "ukázkových dat naplnit kódu")

    *Ukázková data naplnit kódu*
3. Otevřete **Global.asax.cs** souboru a přidejte následující *pomocí* příkazy.

    (Code fragment kódu - *modely a přístup k datům - Ex2 globální direktiv Using Asax*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
~~~
4. V **aplikace\_Start()** metoda přidejte následující řádek k nastavení inicializátoru databáze.

    (Code fragment kódu - *modely a přístup k datům - Ex2 globální Asax SetInitializer*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]
~~~

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Úloha 2 – konfigurace připojení k databázi

Teď, když databáze jste už přidali do našich projektu, budete psát **Web.config** souboru připojovací řetězec.

1. Přidat připojovací řetězec v **Web.config**. Uděláte to tak, že otevřete **Web.config** v kořenu projektu a nahraďte připojovací řetězec s názvem objekt DefaultConnection se tento řádek ve **&lt;connectionStrings&gt;** části:

    ![Umístění souboru Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "umístění souboru Web.config")

    *umístění souboru Web.config*


~~~
[!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Úloha 3 – práci s modelem

Teď, když už jste nakonfigurovali připojení k databázi, propojíte model s databázových tabulek. V této úloze vytvoříte třídu, která propojí do databáze s Code First. Mějte na paměti, že je existující třídy modelu objektů POCO, které by měl být upraven.

   > [!NOTE]
> Pokud jste dokončili cvičení 1, Všimněte si, že se tento krok provést pomocí průvodce. Pomocí tohoto postupu Code First, ručně vytvoříte třídy, které budou propojené s dat entity.


1. Otevřete třídu modelu objektů POCO **Genre** z **modely** projektu složky a obsahovat identifikátor. Použít interní vlastnost s názvem **GenreId**.

    (Code fragment kódu - *modely a přístup k datům - Ex2 kód první Genre*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

> [!NOTE]
> To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.
> 
> You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
~~~
2. Nyní otevřete třídu modelu objektů POCO **Album** z **modely** projektu složky a zahrnují cizí klíče, vytvoření vlastností s názvy **GenreId** a  **ArtistId**. Tato třída už máte **GenreId** pro primární klíč.

    (Code fragment kódu - *modely a přístup k datům - Ex2 kód prvního alba*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
~~~
3. Otevřete třídu modelu objektů POCO **umělcem** a zahrnout **ArtistId** vlastnost.

    (Code fragment kódu - *modely a přístup k datům - Ex2 kód první umělcem*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
~~~
4. Klikněte pravým tlačítkem myši **modely** složky projektu a vyberte **přidat | Třída**. Název souboru **MusicStoreEntities.cs**. Potom klikněte na **přidat.**

    ![Přidání třídy](aspnet-mvc-4-models-and-data-access/_static/image20.png "přidání třídy")

    *Přidání nové položky*

    ![Přidání class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "přidávání class2")

    *Přidání třídy*
5. Třída jste právě vytvořili, otevřete **MusicStoreEntities.cs**a přidají obory názvů **System.Data.Entity** a **System.Data.Entity.Infrastructure**.


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
~~~
6. Nahraďte deklaraci třídy rozšířit **DbContext** – třída: deklarovat veřejné **DBSet** a přepsání **OnModelCreating** metoda. Po provedení tohoto kroku budete mít domény třídu, která odkaz modelu pomocí rozhraní Entity Framework. Aby bylo možné provést, nahraďte kód třídy následující:

    (Code fragment kódu - *modely a přístup k datům - Ex2 kód první MusicStoreEntities*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre. By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table. You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)
~~~

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Úloha 4 – dotazování databáze

V této úloze bude aktualizace třídy pro StoreController tak, aby místo použití pevně zakódované dat, načte se z databáze.

> [!NOTE]
> Tato úloha je společné s cvičení 1.
> 
> Pokud jste dokončili cvičení 1 Všimněte si tyto kroky jsou stejné v obou přístupů (první databáze nebo nejprve Code). Liší se v tom, jak data jsou spojena s modelem, ale přístup k data entity je ještě transparentní z řadiče.


1. Otevřete **Controllers\StoreController.cs** a přidejte následující pole do třídy pro uložení instance **MusicStoreEntities** třídy s názvem **storeDB**:

    (Code fragment kódu - *modely a přístup k datům - Ex1 storeDB*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
~~~
2. **MusicStoreEntities** třída zpřístupňuje vlastnost kolekce pro každou tabulku v databázi. Aktualizace **Procházet** metody akce k načtení Genre se všemi **alb**.

    (Code fragment kódu - *modely a přístup k datům - procházet úložiště Ex2*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
~~~
3. Aktualizace **Index** metody akce k načtení všech žánry.

    (Code fragment kódu - *modely a Data Access – Index úložiště Ex2*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
~~~
4. Aktualizace **Index** metody akce k načtení všech žánry a transformace kolekce do seznamu.

    (Code fragment kódu - *modely a přístup k datům - Ex2 úložiště GenreMenu*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]
~~~

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Úloha 5: spuštění aplikace

V této úloze zkontroluje se, že na stránce Index úložiště se nyní zobrazí žánry uloženy v databázi místo pevně zakódované ty, které jsou. Není třeba měnit zobrazit šablonu, protože **StoreController** vrací stejné **StoreIndexViewModel** jako předtím, ale tentokrát budou pocházet data z databáze.

1. Znovu sestavte řešení a stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Ověřte, že v nabídce **žánry** již není seznamu pevně zakódované a přímo načtena data z databáze.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Procházení žánry z databáze*
3. Nyní přejděte do jakékoli genre a ověřte, zda že alb budou naplněny z databáze.

    ![Procházení alb z databáze](aspnet-mvc-4-models-and-data-access/_static/image23.png "procházení alb z databáze")

    *Procházení alb z databáze*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Cvičení 3: Dotaz na databázi s parametry

V tomto cvičení se dozvíte, jak k dotazování databáze pomocí parametrů a jak používat službu Shaping výsledků dotazu, funkce, která snižuje počet databáze přistupuje k načítání dat v efektivnější.

> [!NOTE]
> Další informace o Shaping výsledek dotazu naleznete na následujícím [článku na webu msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Úloha 1 – změny StoreController alb načíst z databáze

V této úloze se změní **StoreController** třídy pro přístup k databázi načíst alb z konkrétní genre.

1. Otevřete **začít** řešení nacházející se v **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** složky, pokud chcete použít první kód nebo **Source\ EX3. QueryingTheDatabaseWithParametersDBFirst\Begin** složky, pokud chcete použít první databáze. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřete **StoreController** třída změnit **Procházet** metody akce. Chcete-li to provést, v **Průzkumníku řešení**, rozbalte **řadiče** složku a dvojím kliknutím **StoreController.cs**.
3. Změna **Procházet** metody akce k načtení alb pro konkrétní genre. Chcete-li to provést, nahraďte následujícím kódem:

    (Code fragment kódu - *modely a přístup k datům - EX3. StoreController BrowseMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too. You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album. The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.
> 
> You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved. This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information. In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.
> 
> The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well. This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Úloha 2 – spuštění aplikace

V této úloze se spustit aplikaci a načtení alb konkrétní genre z databáze.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/úložiště/procházet? genre = Pop** k ověření, že výsledky jsou načítány z databáze.

    ![Procházení podle genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "procházení podle genre")

    *Browsing /Store/Browse?genre=Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Úloha 3 – přístup k alb podle Id

V této úloze bude opakujte předchozí postup k získání alb podle jejich Id.

1. Zavřete prohlížeč v případě potřeby se vraťte do sady Visual Studio. Otevřete **StoreController** třída změnit **podrobnosti** metody akce. Chcete-li to provést, v **Průzkumníku řešení**, rozbalte **řadiče** složku a dvojím kliknutím **StoreController.cs**.
2. Změna **podrobnosti** metoda akce se načíst podrobnosti o alb na základě jejich **Id**. Chcete-li to provést, nahraďte následujícím kódem:

    (Code fragment kódu - *modely a přístup k datům - EX3. StoreController DetailsMethod*)


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Úloha 4 – spuštění aplikace

V této úloze se spustit aplikaci ve webovém prohlížeči a získat podrobnosti o album podle jejich Id.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/Store/Details/51** nebo žánry Procházet a vyberte album k ověření, že výsledky jsou načítány z databáze.

    ![Procházení podrobností](aspnet-mvc-4-models-and-data-access/_static/image25.png "procházení podrobností")

    *Procházení /Store/Details/51*

> [!NOTE]
> Kromě toho můžete nasadit tuto aplikaci do následující weby systému Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Pomocí dokončení tohoto testovacího prostředí Hands-on jste se naučili základy modely ASP.NET MVC a přístup k datům, pomocí **Database First** přístup a taky **Code First** přístup:

- Jak přidat databáze do řešení, aby bylo možné využívat svá data
- Postup aktualizace řadičů poskytnout šablon zobrazení dat získaných z databáze místo pevně jeden
- Postup dotazování databáze pomocí parametrů
- Jak používat dotazu výsledek tvarování, funkce, která snižuje počet přístupům do databáze, načítání dat v efektivnější
- Jak používat první databáze a Code First přístupy v Microsoft Entity Framework propojení databáze s modelem

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nyní nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.

    ![Nainstalovat Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "nainstalovat Visual Studio Express")

    *Nainstalovat Visual Studio Express*
4. Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Vyjádření souhlasu s podmínkami licence](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Vyjádření souhlasu s podmínkami licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Průběh instalace*
6. Po dokončení instalace, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.

    ![VS Express pro Web dlaždice](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    *VS Express pro Web dlaždice*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu

Tento dodatek vám ukáže, jak vytvořit nový web z portálu Windows Azure Management Portal a publikovat aplikace, kterou jste získali podle testovacím prostředí, využívat výhod nasazení webu publikování funkce poskytované služby Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Úloha 1 – Vytvoření nového webu ze systému Windows Azure Portal

1. Přejděte na [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů společnosti Microsoft, které jsou spojené s vaším předplatným.

    > [!NOTE]
    > S Windows Azure můžete bezplatné hostování 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu. Můžete si zaregistrovat [zde](http://aka.ms/aspnet-hol-azure).

    ![Přihlaste se k portálu Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k Windows Azure Management Portal*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](aspnet-mvc-4-models-and-data-access/_static/image32.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **výpočetní** | **webu**. Potom vyberte **rychle vytvořit** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvoření webu**.

    > [!NOTE]
    > Web systému Windows Azure je hostitel pro spouštění v cloudu, ve kterém můžete řídit a spravovat webovou aplikaci. Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál. Postup pro nastavení databáze neobsahuje.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](aspnet-mvc-4-models-and-data-access/_static/image33.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** je vytvořena.
5. Po vytvoření webu klikněte na odkaz v části **URL** sloupce. Zkontrolujte, zda je funkční nový web.

    ![Procházení na nový web](aspnet-mvc-4-models-and-data-access/_static/image34.png "procházení na nový web")

    *Procházení na nový web*

    ![Webový server spuštěn](aspnet-mvc-4-models-and-data-access/_static/image35.png "webu systémem")

    *Spuštění webu*
6. Přejděte zpět na portál a klikněte na název webu v části **název** sloupec pro zobrazení stránky pro správu.

    ![Otevření stránky Správa webu](aspnet-mvc-4-models-and-data-access/_static/image36.png "otevření stránek správu webového serveru")

    *Otevření stránek správu webového serveru*
7. V **řídicí panel** v části **rychlého přehledu** klikněte na položku **stažení profilu publikování** odkaz.

    > [!NOTE]
    > *Profilu publikování* obsahuje všechny informace požadované pro publikování webové aplikace na web služby Windows Azure pro každou metodu povoleno publikace. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a řetězců databází, které jsou potřebné k připojení k a ověřování na základě těchto koncových bodů, pro které je metoda publikace povolena. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pro Web** a **sadu Microsoft Visual Studio 2012** podporu čtení publikační profily k automatické konfiguraci těchto programů pro publikování webové aplikace na weby služby Windows Azure.

    ![Na webu stažení profilu publikování](aspnet-mvc-4-models-and-data-access/_static/image37.png "stahování webové stránky profilu publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profil publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak používat tento soubor k publikování webové aplikace na weby systému Windows Azure ze sady Visual Studio.

    ![Ukládání souboru profilu publikování](aspnet-mvc-4-models-and-data-access/_static/image38.png "ukládání profilu publikování")

    *Ukládání souboru profilu publikování*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá systému SQL Server, databáze, budete muset vytvořit databázi SQL server. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá systém SQL Server může tuto úlohu přeskočit.

1. Budete potřebovat databázi SQL serveru pro ukládání databázi aplikace. Databáze SQL servery můžete zobrazit ze svého předplatného na portál Windows Azure Management **databází Sql** | **servery** | **serveru Řídicí panel**. Pokud nemáte server vytvořeno, můžete vytvořit jeden pomocí **přidat** tlačítka na panelu příkazů. Poznamenejte si **název serveru a adresa URL, správce přihlašovací jméno a heslo**, jako je použijete v další úkoly. Nevytvářejte databáze ještě, jak bude vytvořen v pozdější fázi.

    ![Řídicí panel serveru databáze SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "řídicího panelu serveru databáze SQL")

    *Řídicí panel serveru databáze SQL*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio, proto je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. To lze provést, klikněte na tlačítko **konfigurace**, vyberte IP adresu z **aktuální IP adresa klienta** a vkládání na **počáteční IP adresa** a **Koncová IP adresa** textová pole a kliknutím ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) tlačítko.

    ![Přidávání IP adresy klienta](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Přidávání IP adresy klienta*
3. Jednou **IP adresa klienta** je povolené IP adresy do seznamu, klikněte na **Uložit** potvrďte změny.

    ![Potvrzení změn](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Potvrzení změn*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu

1. Přejděte zpět na ASP.NET MVC 4 řešení. V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](aspnet-mvc-4-models-and-data-access/_static/image43.png "publikování aplikace")

    *Publikování webu*
2. Umožňuje naimportujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](aspnet-mvc-4-models-and-data-access/_static/image44.png "import profilu publikování")

    *Import profilu publikování*
3. Klikněte na tlačítko **ověření připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření je hotová, jakmile se zobrazí zelené zaškrtnutí zobrazí vedle tlačítko ověřit připojení.

    ![Ověření připojení](aspnet-mvc-4-models-and-data-access/_static/image45.png "ověřování připojení")

    *Ověření připojení*
4. V **nastavení** v části **databáze** části, klikněte na tlačítko vedle připojení databáze textové pole (tj. **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](aspnet-mvc-4-models-and-data-access/_static/image46.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Připojení k databázi nakonfigurujte následujícím způsobem:

   - V **název serveru** zadejte vaše databáze SQL serveru adresu URL pomocí *tcp:* předponu.
   - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
   - V **heslo** zadejte přihlašovací heslo správce serveru.
   - Zadejte nový název databáze.

     ![Konfigurace cílový připojovací řetězec](aspnet-mvc-4-models-and-data-access/_static/image47.png "konfigurace cílový připojovací řetězec")

     *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze, klikněte na tlačítko **Ano**.

    ![Vytvoření databáze](aspnet-mvc-4-models-and-data-access/_static/image48.png "vytváření řetězec databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL v systému Windows Azure je uvedené v rámci textbox výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na databázi SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "připojovací řetězec odkazující na databázi SQL")

    *Připojovací řetězec odkazující na databázi SQL*
8. V **Preview** klikněte na tlačítko **publikovat**.

    ![Publikování webové aplikace](aspnet-mvc-4-models-and-data-access/_static/image50.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Jakmile proces publikování dokončí, otevře se výchozí prohlížeč publikované webové stránky.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Příloha C: používání fragmentů kódu

S fragmenty kódu máte všechny kód, který je nutné na dosah ruky. Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-models-and-data-access/_static/image51.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")

*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.
4. Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).
5. Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.

![Začněte psát název fragmentu](aspnet-mvc-4-models-and-data-access/_static/image52.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](aspnet-mvc-4-models-and-data-access/_static/image53.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")

*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*

![Stisknutím klávesy Tab znovu a fragmentu rozšíří](aspnet-mvc-4-models-and-data-access/_static/image54.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")

*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.
2. Vyberte relevantní fragment kódu ze seznamu, kliknutím na.

![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-models-and-data-access/_static/image55.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-models-and-data-access/_static/image56.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")

*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*
