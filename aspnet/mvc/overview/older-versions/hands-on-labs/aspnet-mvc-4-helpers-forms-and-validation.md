---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 – Pomocníci, formuláře a ověřování | Dokumentace Microsoftu
author: rick-anderson
description: V ASP.NET MVC 4 modely a Data Access praktického testovacího prostředí které byly načítání a zobrazení dat z databáze. Ve tohoto praktického testovacího prostředí, které přidáte...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 8671ae8e9408e6f05135fa27d56480477521c4ba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756478"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4 – Pomocníci, formuláře a ověřování

podle [Campy Web týmu](https://twitter.com/webcamps)

[Stáhněte si Web Campy školení Kit](https://aka.ms/webcamps-training-kit)

V **ASP.NET MVC 4 modely a přístup k datům** praktického testovacího prostředí, aby byla načtení a zobrazení dat z databáze. Ve tohoto praktického testovacího prostředí, které přidáte **Music Store** aplikace možnost upravit tato data.

Pomocí tohoto cíle v úvahu nejprve vytvoříte kontroler, který bude podporovat vytvoření, čtení, aktualizace a odstranění (CRUD) akce alb. Vygeneruje šablonu zobrazení Index s využitím ASP.NET MVC generování uživatelského rozhraní funkce k zobrazení vlastností alb v tabulku HTML. K vylepšení tohoto zobrazení, které přidáte vlastní pomocné rutiny HTML, který se zkrátí dlouhé popisy.

Později přidáte, upravit a vytvořit zobrazení, které vám umožní změnit alb v databázi, pomocí elementů formuláře jako rozevírací seznamy.

A konečně vám umožní uživatelům odstranit alba a také vám zabrání jejich zadávání chybná data ověřením svůj vstup.

Tohoto praktického testovacího prostředí předpokládá, že máte základní znalosti o **ASP.NET MVC**. Pokud jste ještě nepoužívali **ASP.NET MVC** před, doporučujeme si projít **základy ASP.NET MVC** praktického testovacího prostředí.

Tato laboratoř vás provede vylepšení a nových funkcí popsaných dříve použitím menší změny na ukázkovou webovou aplikaci ve zdrojové složce k dispozici.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 Pomocníci, formuláře a ověřování](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto praktického testovacího prostředí se dozvíte, jak:

- Vytvoření kontroleru podporovat operace CRUD
- Generovat zobrazení o Index pro zobrazení vlastností entity do tabulky HTML
- Přidat vlastní pomocné rutiny HTML
- Vytvoření a přizpůsobení zobrazení pro úpravy
- Rozlišovat mezi metody akce, které reagují na HTTP GET nebo POST protokolu HTTP volání
- Přidání a přizpůsobení vytvořit zobrazení
- Zpracování odstranění entity
- Ověření vstupu uživatele

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

Následující praktická cvičení tvoří tohoto praktického testovacího prostředí:

1. [Vytvoření kontroleru Store Manager a jeho zobrazení indexu](#Exercise1)
2. [Přidání pomocné rutiny HTML](#Exercise2)
3. [Vytvoření zobrazení pro úpravy](#Exercise3)
4. [Přidání zobrazení pro vytváření](#Exercise4)
5. [Zpracování odstranění](#Exercise5)
6. [Přidání ověření](#Exercise6)
7. [Pomocí Nerušivého jQuery na straně klienta](#Exercise7)

> [!NOTE]
> Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.


Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Cvičení 1: Vytvoření kontroleru Store Manager a jeho zobrazení indexu

V tomto cvičení se dozvíte, jak vytvořit nový kontroler podporuje operace CRUD, přizpůsobte její metody akce indexu se seznam alb z databáze a nakonec generování šablony služby Index zobrazení využití výhod generování uživatelského rozhraní ASP.NET MVC funkce k zobrazení vlastností alb v tabulku HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Úloha 1 – vytváření StoreManagerController

V této úloze vytvoříte nový kontroler volá **StoreManagerController** pro podporu operací CRUD.

1. Otevřít **začít** řešení nachází v **zdroj/Ex1-CreatingTheStoreManagerController/počáteční/** složky.

   1. Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Přidáte nový kontroler. Chcete-li to provést, klikněte pravým tlačítkem **řadiče** složku v Průzkumníku řešení vyberte **přidat** a pak **řadič** příkazu. Změnit **řadič** **název** k **StoreManagerController** a ujistěte se, že možnost **kontroler MVC s akcemi čtení/zápisu-prázdný**zaškrtnuto. Klikněte na tlačítko **přidat**.

    ![Dialogové okno Přidat kontroler](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "dialogového okna Přidat kontroler")

    *Přidat Dialog Kontroleru*

    Nová třída Kontroleru je generována. Vzhledem k tomu, který jste určili pro přidání akce pro čtení a zápis, metody zástupných procedur pro ty, běžné akce CRUD se vytvoří s komentáře TODO vyplněna, s výzvou k zahrnout konkrétní logiku aplikace.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Úloha 2 – přizpůsobení StoreManager indexu

V této úloze budete přizpůsobovat StoreManager Index metody akce k vrácení se seznamem alb zobrazení z databáze.

1. Ve třídě StoreManagerController přidejte následující *pomocí* direktivy.

    (Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - Ex1 pomocí MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Přidáním pole do **StoreManagerController** pro uložení instance **MusicStoreEntities.**

    (Fragment - kódu *ASP.NET MVC 4 – Pomocníci, formuláře a ověřování – Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implementace StoreManagerController indexu akce, která vrátí zobrazení se seznamem alb.

    Akce logice Kontroleru bude velmi podobný indexu akce StoreController napsané dříve. Zprostředkovatel LINQ slouží pro načtení všech alb, včetně informací o žánr a interpret pro zobrazení.

    (Fragment - kódu *ASP.NET MVC 4 – Pomocníci, formuláře a ověřování – Ex1 StoreManagerController Index*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Úloha 3 – vytvoření indexu zobrazení

V této úloze vytvoříte šablonu zobrazení Index k zobrazení seznamu vrácených alb **StoreManager** Kontroleru.

1. Před vytvořením nové šablony zobrazení se má sestavit projekt tak, aby **Přidat Dialog zobrazení** ví o **alba** třídu použít. Vyberte **sestavení | Sestavení MvcMusicStore** k sestavení projektu.
2. Klepněte pravým tlačítkem myši **Index** metody akce a vyberte **přidat zobrazení**. Tím se otevře **přidat zobrazení** dialogového okna.

    ![Přidání zobrazení](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "přidat zobrazení")

    *Přidání zobrazení z v rámci Index – metoda*
3. V dialogovém okně Přidat zobrazení ověřte, zda je název zobrazení **Index**. Vyberte **vytvoření zobrazení se silnými typy** a vyberte možnost **alba (MvcMusicStore.Models)** z **třída modelu** rozevíracího seznamu. Vyberte **seznamu** z **šablona Scaffold** rozevíracího seznamu. Nechte **zobrazovací modul** k **Razor** a v ostatních polích pomocí jejich výchozí hodnoty a pak klikněte na tlačítko **přidat**.

    ![Přidání zobrazení index se o](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "přidáte zobrazení indexu")

    *Přidání zobrazení Index se o*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Úloha 4 – přizpůsobení vygenerované uživatelské rozhraní zobrazení indexu

V tomto úkolu budou upraveny jednoduché zobrazení šablony vytvořené s funkcí generování uživatelského rozhraní ASP.NET MVC na něm zobrazit pole, která chcete.

> [!NOTE]
> **Generování uživatelského rozhraní** podpory v rámci technologie ASP.NET MVC vygeneruje šablonu jednoduché zobrazení, která se zobrazí seznam všech polí v modelu alba. **Generování uživatelského rozhraní** poskytuje rychlý způsob, jak začít pracovat v zobrazení se silnými typy: namísto nutnosti psát zobrazit šablonu ručně, generování uživatelského rozhraní rychle generuje výchozí šablonu a pak můžete vygenerovaný kód upravit.


1. Revize kódu vytvoří. Vygenerovaný seznam polí bude součástí následující tabulky HTML, který **generování uživatelského rozhraní** používá pro zobrazení tabulková data.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Nahradit **&lt;tabulky&gt;** kód následujícím kódem, chcete-li zobrazit pouze **žánr**, **interpreta**, **alba**, a **cena** pole. Tím se odstraní **AlbumId** a **alba obrázky URL** sloupce. Navíc změní GenreId a ArtistId sloupce pro zobrazení jejich vlastností propojené třídy **Artist.Name** a **Genre.Name**a odebere **podrobnosti** odkaz.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Změna v následujících popisech.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Úloha 5: spuštění aplikace

V této úloze budete testovat, který **StoreManager** **Index** zobrazit šablonu zobrazí seznam alb podle návrhu v předchozích krocích.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager** k ověření, že se zobrazí seznam alb, zobrazení jejich **Title**, **interpreta** a **žánr**.

    ![Procházení seznamu alb](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "procházení seznamu alb")

    *Procházení seznamu alb*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Cvičení 2: Přidání pomocné rutiny HTML

StoreManager indexovou stránku má jedním potenciálním problémem: název a interpret vlastnosti mohou být dostatečně dlouhá, aby throw vypnout formátování tabulky. V tomto cvičení se dozvíte, jak přidat vlastní pomocné rutiny HTML došlo ke zkrácení tento text.

Na následujícím obrázku vidíte, jak formát se mění z důvodu délka textu při použití malé prohlížeče.

![Procházení seznamu alb s nikoli zkráceno text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "procházení seznamu alb s nikoli zkráceno text")

*Procházení seznamu alb s nikoli zkráceno text*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Úloha 1 – rozšíření pomocné rutiny HTML

V této úloze přidá novou metodu **Truncate** k **HTML** objekt zveřejní v zobrazeních ASP.NET MVC. K tomuto účelu můžete implementovat **– metoda rozšíření** do vestavěné **System.Web.Mvc.HtmlHelper** třídy poskytované rozhraní ASP.NET MVC.

> [!NOTE]
> Další informace o tom **rozšiřující metody**, navštivte prosím článku na webu msdn. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Otevřít **začít** řešení nachází v **zdroj/Ex2-AddingAnHTMLHelper/počáteční/** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřete zobrazení indexu StoreManager společnosti. Chcete-li to provést, v Průzkumníku řešení rozbalte **zobrazení** složku, pak bude **StoreManager** a otevřete **Index.cshtml** souboru.
3. Přidejte následující kód níže <strong>@model</strong> směrnice pro definování <strong>Truncate</strong> Pomocná metoda.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Úloha 2 – zkracování textu na stránce

V této úloze budete používat **Truncate** metoda se text zkrátí podle v šabloně zobrazení.

1. Otevřete zobrazení indexu StoreManager společnosti. Chcete-li to provést, v Průzkumníku řešení rozbalte **zobrazení** složku, pak bude **StoreManager** a otevřete **Index.cshtml** souboru.
2. Nahraďte na čárách znázorňujících **interpret** a alba **Title**. Provedete to nahrazením následující řádky.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Úloha 3 – spuštění aplikace

V této úloze budete testovat, který **StoreManager** **Index** zobrazit šablonu zkrátí nadpis alba a interpret.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager** k ověření tohoto dlouho texty v **Title** a **interpreta** sloupce se zkrátí.

    ![Oříznutá názvy a umělci názvy](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "zkrácen názvy a umělci názvy")

    *Zkrácené názvy a názvy interpret*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Cvičení 3: Vytvoření zobrazení pro úpravy

V tomto cvičení se dozvíte, jak vytvořit formulář umožňuje správcům úložiště upravit alba. Bude procházet **/StoreManager/Edit/id** adresy URL (**id** je jedinečné id alba pro úpravy), díky čemuž volání rozhraní HTTP GET na server.

Metoda akce Kontroleru upravit bude načítat příslušné alba z databáze, vytvořit **StoreManagerViewModel** objektu k zapouzdření (spolu s seznam vašim Animátorům a žánry) a pak ji předal zobrazit šablonu pro vykreslení stránky HTML zpět uživateli. Na této stránce **&lt;formuláře&gt;** element s textových polí a rozevírací seznamy pro úpravy vlastnosti.

Jakmile uživatel aktualizuje hodnoty formuláře alba a klikne na tlačítko **Uložit** tlačítko, se změny odesílají prostřednictvím HTTP POST zpětné volání **/StoreManager/Edit/id**. I když adresa URL zůstane stejná jako v posledním volání, ASP.NET MVC identifikuje, který této doby je POST protokolu HTTP a proto spustí jinou metodu akce úpravy (jeden dekorován **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Úloha 1 – implementace metody GET protokolu HTTP úpravy akce

V této úloze budete implementovat verze HTTP GET metody akce úpravy k načtení příslušné alba z databáze, a také seznam všech žánry a umělci. To se zabalit tato data do **StoreManagerViewModel** definované v posledním kroku, který se potom předá do šablony zobrazení k vykreslení odpovědi s objekty.

1. Otevřít **začít** řešení nachází v **zdroj/EX3.-CreatingTheEditView/počáteční/** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřít **StoreManagerController** třídy. Chcete-li to provést, rozbalte **řadiče** složky a dvojím kliknutím **StoreManagerController.cs**.
3. Nahraďte **upravit HTTP GET** metodu akce pomocí následující kód k načtení příslušné **alba** také **žánry** a **umělci**seznamy.

    (Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - EX3. StoreManagerController HTTP GET upravit akci*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Používáte **System.Web.Mvc** **SelectList** vašim Animátorům a žánry místo **System.Collections.Generic** seznamu.
    > 
    > **SelectList** je čistší způsob naplnění HTML a spravovat věci, jako je aktuální výběr. Vytváření instancí a novější k nastavení těchto objektů ViewModel v akce kontroleru způsobí, že scénář formuláře úprav čisticího modulu.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Úloha 2 – Vytvoření zobrazení pro úpravy

V této úloze vytvoříte šablonu zobrazení pro úpravy, které se později zobrazí vlastnosti.

1. Vytvoření zobrazení pro úpravy. Chcete-li to provést, klepněte pravým tlačítkem myši **upravit** metody akce a vyberte **přidat zobrazení**.
2. V dialogovém okně Přidat zobrazení ověřte, zda je název zobrazení **upravit**. Zkontrolujte **vytvoření zobrazení se silnými typy** zaškrtávací políčko a vyberte **alba (MvcMusicStore.Models)** z **zobrazení dat třídy** rozevíracího seznamu. Vyberte **upravit** z **šablona Scaffold** rozevíracího seznamu. Ostatní pole nechte výchozí hodnoty a pak klikněte na tlačítko **přidat**.

    ![Přidání zobrazení pro úpravy](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "přidání zobrazení pro úpravy")

    *Přidání zobrazení pro úpravy*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Úloha 3 – spuštění aplikace

V této úloze budete testovat, který **StoreManager** **upravit** zobrazení stránky zobrazí hodnoty vlastností pro album předán jako parametr.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager/Edit/1** k ověření, že se zobrazují hodnoty vlastností pro alba předán.

    ![Procházení zobrazení pro úpravy alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "procházení alba zobrazení pro úpravy")

    *Procházení alba zobrazení pro úpravy*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Úloha 4 – implementace rozevírací seznamy pro šablony alba editoru

V této úloze přidáte rozevírací seznamy k šabloně zobrazení vytvořili v minulé úloze tak, aby si uživatel může vybrat ze seznamu umělců nebo žánrů.

1. Nahradit vše **alba** fieldset kód následujícím kódem:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > **Html.DropDownList** pomocné rutiny byla přidána k vykreslení rozevírací seznamy pro výběr umělců nebo žánrů. Parametry předané do **Html.DropDownList** jsou:
    > 
    > 1. Název pole formuláře (**&quot;ArtistId&quot;**).
    > 2. **SelectList** hodnot pro rozevírací seznam.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Úloha 5: spuštění aplikace

V této úloze budete testovat, který **StoreManager** **upravit** zobrazení stránky zobrazí rozevírací seznamy místo interpreta a rozšířením podle tematických ID textová pole.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager/Edit/1** k ověření, zobrazuje rozevírací seznamy místo interpreta a rozšířením podle tematických ID textová pole.

    ![Procházení alba zobrazení pro úpravy s rozevírací nabídky](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "procházení alba zobrazení pro úpravy s rozevírací nabídky")

    *Zobrazení pro úpravy procházení alb, tentokrát s rozevírací seznamy*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Krok 6 – implementace upravit HTTP POST metody akce

Teď, když zobrazení pro úpravy pro zobrazí podle očekávání, budete muset implementovat metodu HTTP POST upravit akci Uložit změny provedené v alba.

1. Zavřete prohlížeč v případě potřeby se vraťte do okna nástroje Visual Studio. Otevřít **StoreManagerController** z **řadiče** složky.
2. Nahraďte **upravit HTTP POST** kódu metody akce s tímto (Všimněte si, že je metoda, která se musí nahradit přetížené verze, která přijímá dva parametry):

    (Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - EX3. StoreManagerController HTTP POST upravit akci*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Tato metoda se provede, když uživatel klikne **Uložit** tlačítko zobrazení a provádí HTTP-POST hodnot formulář zpět na server pro uchování v databázi. Dekoratér **[HttpPost]** označuje, že metoda by měla sloužit pro tyto scénáře HTTP POST. Tato metoda přebírá **alba** objektu. ASP.NET MVC automaticky vytvoří objekt alba z zaúčtované &lt;formuláře&gt; hodnoty.
    > 
    > Metoda provede tyto kroky:
    > 
    > 1. Pokud je model platný:
    > 
    >     1. Aktualizujte položku alba v kontextu označte ji jako změněný objekt.
    >     2. Uložte změny a přesměrování na zobrazení indexu.
    > 2. Pokud model není platný, naplní objekt ViewBag s **GenreId** a **ArtistId**, vrátí zobrazení s přijatého objektu alba umožňující uživateli provést všechny požadované aktualizace.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Úloha 7: spuštění aplikace

V této úloze budete testovat, který **StoreManager upravit** aktualizovaná data alba zobrazit stránku ve skutečnosti uloží do databáze.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager/Edit/1**. Změnit název alba **zatížení** a klikněte na **Uložit**. Ověřte, že ve skutečnosti změnit název společnosti v seznamu alb.

    ![Aktualizuje se alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "aktualizace alba")

    *Aktualizuje se alba*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Cvičení 4: Přidání vytvořit zobrazení

Teď, když **StoreManagerController** podporuje **upravit** možnost, v tomto cvičení se dozvíte, jak přidat příkaz Create View šablonu, která umožní ukládat správci přidat nové alb do aplikace.

Stejně jako s funkcí upravit implementujete scénář vytvořit pomocí dvou samostatných metod v rámci **StoreManagerController** třídy:

1. Jedna metoda akce se zobrazí prázdný formulář po Správci úložiště nejprve přejděte **/StoreManager/vytvořit** adresy URL.
2. Druhá metoda akce zpracuje scénář, kde klikne na tlačítko Správce úložiště **Uložit** tlačítko ve formuláři a odešle hodnoty zpět **/StoreManager/vytvořit** adresy URL jako POST protokolu HTTP.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Úloha 1 – implementace metody GET protokolu HTTP vytvořit akci

V této úloze budete implementovat verze HTTP GET metody akce vytvořit seznam všech žánry a umělci získat, zabalit tato data do **StoreManagerViewModel** objekt, který se potom předá do zobrazení šablony.

1. Otevřít **začít** řešení nachází v **zdroj/Ex4-AddingACreateView/počáteční/** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřít **StoreManagerController** třídy. Chcete-li to provést, rozbalte **řadiče** složky a dvojím kliknutím **StoreManagerController.cs**.
3. Nahradit **vytvořit** akce metoda kód následujícím kódem:

    (Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování – akce vytvoření Ex4 StoreManagerController HTTP GET*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Úloha 2 – Přidání zobrazení pro vytváření

V této úloze přidá šablony vytvořit zobrazení, které se zobrazí nový formulář alba (prázdné).

1. Klepněte pravým tlačítkem myši **vytvořit** metody akce a vyberte **přidat zobrazení**. Tím se otevře dialogové okno Přidat zobrazení.
2. V dialogovém okně Přidat zobrazení ověřte, zda je název zobrazení **vytvořit**. Vyberte **vytvoření zobrazení se silnými typy** možnost a vyberte **alba (MvcMusicStore.Models)** z **třída modelu** rozevíracího seznamu a **vytvořit** z **šablona Scaffold** rozevíracího seznamu. Ostatní pole nechte výchozí hodnoty a pak klikněte na tlačítko **přidat**.

    ![Přidání zobrazení pro vytváření](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "přidání-create-view.png")

    *Přidání zobrazení pro vytváření*
3. Aktualizace **GenreId** a **ArtistId** pole pomocí rozevíracího seznamu, jak je znázorněno níže:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Úloha 3 – spuštění aplikace

V této úloze budete testovat, který **StoreManager** **vytvořit** prázdný formulář alba zobrazí stránku zobrazení.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager/vytvoření**. Zkontrolujte, jestli prázdný formulář zobrazuje pro vyplnění nové vlastnosti.

    ![Vytvořit zobrazení s prázdný formulář](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "vytvořit zobrazení s prázdný formulář")

    *Vytvořit zobrazení s prázdný formulář*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Úloha 4 – implementace HTTP POST vytvořit metody akce

V této úloze budete implementovat verze HTTP POST metody akce Create, která se vyvolá, když uživatel klikne **Uložit** tlačítko. Metoda by měla uložit nové album v databázi.

1. Zavřete prohlížeč v případě potřeby se vraťte do okna nástroje Visual Studio. Otevřít **StoreManagerController** třídy. Chcete-li to provést, rozbalte **řadiče** složky a dvojím kliknutím **StoreManagerController.cs**.
2. Nahraďte **HTTP POST vytvořit** akce metoda kód následujícím kódem:

    (Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - Ex4 StoreManagerController HTTP - POST akce vytvoření*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > Akce vytvoření je hodně podobné předchozí metoda akce upravit, ale namísto objektu nastavení, jako jsou změny, se přidá se do kontextu.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Úloha 5: spuštění aplikace

V této úloze budete testovat, který **StoreManager vytvořit** stránka zobrazení vám umožní vytvářet nové alba a pak přesměruje na zobrazení StoreManager Index.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager/vytvoření**. Vyplnit všechna pole formuláře s daty pro nové Album jako na následujícím obrázku:

    ![Vytváření alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "vytváření alba")

    *Vytváření alba*
3. Ověřte, že získání přesměrován na StoreManager Index zobrazení, které obsahuje nové Album právě vytvořili.

    ![Vytvoří nové Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "nové Album vytvořili")

    *Vytvoření nové Album*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Cvičení 5: Zpracování odstranění

Možnost odstranit alb ještě není naimplementovaný. To je, co toto cvičení bude o. Podobně jako předtím implementujete scénář odstranění pomocí dvou samostatných metod v rámci **StoreManagerController** třídy:

1. Jedna metoda akce se zobrazí potvrzení formuláře
2. Druhá metoda akce bude zpracovávat odeslání formuláře

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Úloha 1 – implementace metody GET protokolu HTTP Delete akce

V této úloze budete implementovat verze HTTP GET metody akce Delete se načíst informace o alba.

1. Otevřít **začít** řešení nachází v **zdroj/Ex5-HandlingDeletion/počáteční/** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřít **StoreManagerController** třídy. Chcete-li to provést, rozbalte **řadiče** složky a dvojím kliknutím **StoreManagerController.cs**.
3. Odstranění akce kontroleru je přesně stejný jako předchozí akce kontroleru Store podrobnosti: se dotázal **alba** objekt z databáze pomocí **id** součástí adresy URL a vrátí odpovídající **zobrazení**. Chcete-li to provést, nahraďte HTTP GET **odstranit** akce metoda kód následujícím kódem:

    (Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - Ex5 zpracování odstranění – odstranit HTTP GET – akce*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Klepněte pravým tlačítkem myši **odstranit** metody akce a vyberte **přidat zobrazení**. Tím se otevře dialogové okno Přidat zobrazení.
5. V dialogovém okně Přidat zobrazení ověřte, zda je název zobrazení **odstranit**. Vyberte **vytvoření zobrazení se silnými typy** možnost a vyberte **alba (MvcMusicStore.Models)** z **třída modelu** rozevíracího seznamu. Vyberte **odstranit** z **šablona Scaffold** rozevíracího seznamu. Ostatní pole nechte výchozí hodnoty a pak klikněte na tlačítko **přidat**.

    ![Přidání zobrazení odstranit](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "přidání odstranit zobrazení")

    *Přidání zobrazení Delete*
6. Odstranit šablonu zobrazí všechna pole z modelu. Se zobrazí pouze alba název. Provedete to tak, nahradí obsah zobrazení s následujícím kódem:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Úloha 2 – spuštění aplikace

V této úloze budete testovat, který **StoreManager** **odstranit** zobrazení stránky zobrazí formulář potvrzení odstranění.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager**. Vyberte jeden alba odstranit tak, že kliknete na **odstranit** a ověřte, že se nahraje nové zobrazení.

    ![Odstraňuje se alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "odstranění alba")

    *Odstraňuje se alba*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Úloha 3 – implementace metody akce Delete HTTP POST

V této úloze budete implementovat verze HTTP POST metody akce odstranění, který bude vyvolán při kliknutí **odstranit** tlačítko. Metoda by měla odstranit album v databázi.

1. Zavřete prohlížeč v případě potřeby se vraťte do okna nástroje Visual Studio. Otevřít **StoreManagerController** třídy. Chcete-li to provést, rozbalte **řadiče** složky a dvojím kliknutím **StoreManagerController.cs**.
2. Nahraďte **HTTP POST odstranit** akce metoda kód následujícím kódem:

    (Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - Ex5 zpracování odstranění – odstranit HTTP POST – akce*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Úloha 4 – spouštění aplikace

V této úloze budete testovat, který **StoreManager odstranit** stránka zobrazení můžete odstranit alba a pak přesměruje na zobrazení indexu StoreManager.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager**. Vyberte jeden alba odstranit tak, že kliknete na **odstranit.** Potvrďte odstranění kliknutím **odstranit** tlačítka:

    ![Odstraňuje se alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "odstranění alba")

    *Odstraňuje se alba*
3. Ověřte, že byl alba odstranit, protože se nezobrazují v **Index** stránky.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Cvičení 6: Přidání ověření

Vytvoření a úprava formuláře, které máte v současné době neprovádějte jakéhokoli druhu ověřování. Pokud uživatel ponechá povinné pole je prázdné nebo písmena typ v poli pro cenu, první chyby, které dostanete se z databáze.

Můžete přidat ověřování do aplikace tak, že přidáte anotacemi dat do vaší třídy modelu. Anotací dat při povolení popisující pravidla, která chcete použít pro vlastnosti modelu a ASP.NET MVC se postará o vynucení a zobrazení odpovídající zprávu pro uživatele.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Úloha 1 – přidání anotací dat

V této úloze přidáte anotacemi dat alba model, který bude na stránce vytvořit a upravit zobrazení ověřovacích zpráv v případě potřeby.

Pro jednoduchou třídu modelu, přidávání poznámek Data právě probíhá tak, že přidáte **pomocí** příkaz pro **System.ComponentModel.DataAnnotation**, pak umístit **[povinné]** atribut na odpovídající vlastnosti. V následujícím příkladu s žádným **název** vlastnost povinné pole. v zobrazení.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Toto je o něco složitější v případech, jako je tato aplikace, ve kterém se vygeneruje modelu Entity Data Model. Pokud jste přidali anotacemi dat přímo do třídy modelu, byste při aktualizaci modelu z databáze nástroje přepsány. Místo toho můžete provést pomocí metadat částečné třídy, které budou existovat pro uložení poznámky a jsou spojeny s modelem třídy pomocí **[MetadataType]** atribut.

1. Otevřít **začít** řešení nachází v **zdroj/Ex6-AddingValidation/počáteční/** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřít **Album.cs** z **modely** složky.
3. Nahraďte **Album.cs** obsahu se zvýrazněný kód tak, aby vypadal jako následující:

    > [!NOTE]
    > Na řádku **[DisplayFormat(ConvertEmptyStringToNull=false)]** označuje, že prázdné řetězce z modelu nebudou převedeny na hodnotu null při aktualizaci ve zdroji dat datové pole. Toto nastavení zabrání výjimku, pokud předtím, než Data anotace ověřuje pole Entity Framework přiřadí hodnoty null do modelu.

    (Fragment - kódu *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování - částečné třídy metadat Ex6 alba*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > To **alba** má částečné třídy **MetadataType** atribut, který odkazuje na **AlbumMetaData** třídy pro datové poznámky. Zde je několik příkladů dat anotace atributy, které používáte opatřit poznámkami alba modelu:
    > 
    > - Nepožadováno – označuje, že vlastnost je povinné pole.
    > - DisplayName – definuje text bude použit na pole formuláře a ověřovacích zpráv
    > - DisplayFormat – určuje způsob zobrazení a ve formátu datová pole.
    > - StringLength – definuje maximální délku pole řetězce
    > - V rozsahu - poskytuje maximální a minimální hodnoty pro číselné pole
    > - ScaffoldColumn – umožňuje skrýt pole z editoru formuláře

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Úloha 2 – spuštění aplikace

V této úloze budete testovat, vytvoření a úprava stránky ověření pole, pomocí zobrazované názvy zvolili v minulé úloze.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager/vytvoření**. Ověřte, že zobrazované názvy odpovídají v částečné třídy (jako je **alba obrázky URL** místo **AlbumArtUrl**)
3. Klikněte na tlačítko **vytvořit**, bez vyplňování formuláře. Ověřte, že dostanete odpovídajících ověřovacích zpráv.

    ![Ověření pole na stránce vytvořit](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "ověření pole na stránce vytvořit")

    *Ověřená polí na stránce vytvořit*
4. Můžete ověřit, že stejné dochází u **upravit** stránky. Změňte adresu URL na **/StoreManager/Edit/1** a ověřte, že zobrazované názvy odpovídají v částečné třídy (jako je **alba obrázky URL** místo **AlbumArtUrl**). Prázdný **Title** a **cena** pole a klikněte na tlačítko **Uložit**. Ověřte, že dostanete odpovídajících ověřovacích zpráv.

    ![Ověřená polí na stránce Upravit](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Ověřená polí na stránce Upravit*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Cvičení 7: Pomocí Nerušivého jQuery na straně klienta

V tomto cvičení se dozvíte, jak povolit MVC 4 Nerušivý jQuery ověření na straně klienta.

> [!NOTE]
> Nerušivý jQuery používá data-ajax předponu JavaScript volat v serveru místo intrusively generování skriptů klienta vložené metody akce.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Úloha 1 – spuštění aplikace před povolením Nerušivý jQuery

V této úloze budete spouštět aplikace před zahrnutím jQuery k porovnání obou modelů ověřování.

1. Otevřít **začít** řešení nachází v **zdroj/Ex7-UnobtrusivejQueryValidation/počáteční/** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Stisknutím klávesy **F5** ke spuštění aplikace.
3. Projekt se spustí na domovské stránce. Procházet **/StoreManager/vytvořit** a klikněte na tlačítko **vytvořit** bez vyplňování formuláře a přesvědčte se, že dostanete zprávy ověření na:

    ![Zakázáno ověřování na straně klienta](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "zakázáno ověřování na straně klienta")

    *Zakázáno ověřování na straně klienta*
4. V prohlížeči otevřete zdrojový kód HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Úloha 2 – povolení ověřování Nerušivý klienta

V této úloze povolíte jQuery **ověření nerušivého klienta** z **Web.config** soubor, který je ve výchozím nastavení nastavena na hodnotu false ve všech nových projektech ASP.NET MVC 4. Kromě toho můžete přidá že nezbytné skripty odkazy, aby jQuery pracovní Nerušivý ověřování na straně klienta.

1. Otevřít **Web.Config** soubor v kořenu projektu a ujistěte se, že **ClientValidationEnabled** a **UnobtrusiveJavaScriptEnabled** hodnoty klíče jsou nastaveny na **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Můžete také povolit ověřování na straně klienta pomocí kódu v Global.asax.cs stejné výsledky:
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > Kromě toho můžete přiřadit ClientValidationEnabled atribut do libovolného řadiče mít vlastní chování.
2. Otevřít **Create.cshtml** na **Views\StoreManager**.
3. Ujistěte se, že následující soubory skriptu, **jquery.validate** a **jquery.validate.unobtrusive**, přes zobrazení odkazuje &quot; **~/bundles/jqueryval** &quot; sady.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Tyto knihovny jQuery jsou součástí nové projekty MVC 4. Můžete najít další knihovny v **/skripty** složce projektu je.
    > 
    > Aby bylo toto ověření pracovní knihovny, budete muset přidat odkaz na framework knihovnu jQuery. Vzhledem k tomu, že tento odkaz je již přidána do  **\_Layout.cshtml** souboru, není potřeba ji přidat do tohoto zobrazení.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Úloha 3 – spuštění aplikace pomocí Nerušivého jQuery ověření

V této úloze budete testovat, který **StoreManager** vytvořte zobrazení šablony provádí ověřování na straně klienta pomocí knihovny jQuery, když uživatel vytvoří nové album.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Procházet **/StoreManager/vytvořit** a klikněte na tlačítko **vytvořit** bez vyplňování formuláře a přesvědčte se, že dostanete zprávy ověření na:

    ![Ověřování na straně klienta s jQuery povolené](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "ověřování na straně klienta s jQuery povoleno")

    *Ověřování na straně klienta s jQuery povoleno*
3. V prohlížeči otevřete zdrojový kód pro zobrazení pro vytváření:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Pro každý ověřovací pravidlo klienta Nerušivý jQuery přidá atribut s daty – val –*rulename*=&quot;*zpráva*&quot;. Níže je seznam značek tohoto Unobtrusive jQuery vloží do vstupního pole html k provedení ověřování na straně klienta:
   > 
   > - Data val
   > - Číslo datového val
   > - Oblast dat val
   > - Data val rozsah min / Data val range max
   > - Vyžadována val data
   > - Délka dat val
   > - Data-val délka max / dat val délka min
   > 
   > Všechny datové hodnoty jsou vyplněny hodnotou modelu **dat. Poznámka**. Potom veškerou logiku, která funguje na straně serveru může být spuštěna na straně klienta. Například cena atribut má následující poznámka data v modelu:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Po použití Nerušivý jQuery, je generovaný kód:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Vyplněním tohoto praktického testovacího prostředí jste se naučili, jak povolit uživatelům změnit data uložená v databázi s použitím následující:

- Akce kontroleru, jako je Index, vytvořit, upravit, odstranit
- Funkce generování uživatelského rozhraní ASP.NET MVC pro zobrazení vlastností v tabulku HTML
- Vlastní pomocné rutiny HTML pro zlepšení uživatelského prostředí
- Metody akce, které reagují na HTTP GET nebo POST protokolu HTTP volání
- Šablona sdíleného editor pro podobné zobrazení šablony, jako je vytvoření a úprava
- Rozevírací seznamy, jako jsou elementy Form
- Anotací dat při ověření modelu
- Ověřování na straně klienta pomocí Nerušivého knihovny jQuery

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.

    ![Instalace sady Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "instalace sady Visual Studio Express")

    *Instalace sady Visual Studio Express*
4. Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Přijetí podmínek licence](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Přijetí podmínek licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Průběh instalace*
6. Až instalace skončí, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.

    ![VS Express for Web dlaždice](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express for Web dlaždice*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Příloha B: používání fragmentů kódu

Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky. Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")

*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.
4. Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).
5. Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.

![Začněte psát název fragmentu kódu](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")

*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*

![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")

*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.
2. Kliknutím na vyberte relevantní fragment kódu ze seznamu.

![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")

*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*
