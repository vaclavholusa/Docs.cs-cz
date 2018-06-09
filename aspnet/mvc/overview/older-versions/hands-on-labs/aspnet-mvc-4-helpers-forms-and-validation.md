---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Pomocné rutiny ASP.NET MVC 4, formulářů a ověřování | Microsoft Docs
author: rick-anderson
description: V technologii ASP.NET MVC 4 modely a Data laboratoř Hands-on přístupu máte byla načítání a zobrazení dat z databáze. V tomto testovacím prostředí Hands-on přidáte...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 4cfa98144919c3f1bdb3608970af1a7952fe6ea7
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "30878175"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>Pomocné rutiny ASP.NET MVC 4, formulářů a ověřování

Podle [webové táborech Team](https://twitter.com/webcamps)

[Stažení webové táborech cvičení Kit](https://aka.ms/webcamps-training-kit)

V **ASP.NET MVC 4 modely a přístup k datům** Hands-on testovací prostředí, jste načítání a zobrazení dat z databáze. V tomto testovacím prostředí Hands-on přidáte **Hudba úložiště** aplikace možnost upravovat data.

S Pamatujte, že cílem nejprve vytvoříte řadič, která bude podporovat vytvoření, čtení, aktualizace a odstranění (CRUD) akce alb. Vygeneruje šablonu zobrazení indexu využívat výhod funkce generování uživatelského rozhraní ASP.NET MVC zobrazíte vlastnosti alb do tabulky HTML. K vylepšení tohoto zobrazení, přidáte vlastní pomocné rutiny HTML, který bude zkrátit dlouhý popis.

Bude později, přidat, upravit a vytvořit zobrazení, které vám umožní změnit alb v databázi, pomocí elementů formuláře jako rozevíracích seznamů.

Nakonec vám umožní uživatelům odstranění alba a také můžete zabránit tomu, zadávání chybná data tím, že ověří jejich vstup.

Toto testovací prostředí Hands-on předpokládá, že máte základní znalosti o **ASP.NET MVC**. Pokud jste nepoužili **ASP.NET MVC** před, doporučujeme si projít **ASP.NET MVC Základy** Hands-on testovacího prostředí.

Tato laboratoř vás provede procesem vylepšení a nových funkcí popsaných výše použitím malých změn na ukázkové webové aplikaci ve zdrojové složce zadané.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [verze Microsoft-webové/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 pomocné rutiny, formulářů a ověřování](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí Hands-On se dozvíte, jak:

- Vytvoření kontroleru podporovat operace CRUD
- Generovat zobrazení Index pro zobrazení vlastností entity do tabulky HTML
- Přidat vlastní pomocné rutiny HTML
- Vytvářet a přizpůsobovat zobrazení o upravit
- Rozlišit mezi metody akce, které reagují na HTTP GET nebo POST protokolu HTTP volání
- Přidání a přizpůsobení vytvořit zobrazení
- Zpracování odstranění entity
- Ověření vstupu uživatele

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

Následující cvičení tvoří toto Hands-On testovací prostředí:

1. [Vytváření řadič úložiště Manager a jeho zobrazení indexu](#Exercise1)
2. [Přidání pomocné rutiny HTML](#Exercise2)
3. [Vytvoření zobrazení pro úpravy](#Exercise3)
4. [Přidání zobrazení pro vytváření](#Exercise4)
5. [Zpracování odstranění](#Exercise5)
6. [Přidání ověření](#Exercise6)
7. [Pomocí Nerušivý jQuery na straně klienta](#Exercise7)

> [!NOTE]
> Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.


Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Cvičení 1: Vytvoření řadič úložiště Manager a jeho zobrazení indexu

V tomto cvičení se dozvíte, jak vytvořit nový řadič pro podporu operací CRUD, přizpůsobit její metody akce Index k zobrazení seznamu alb z databáze a nakonec generování šablonu zobrazení indexu využívat výhod generování uživatelského rozhraní ASP.NET MVC Funkce zobrazíte vlastnosti alb do tabulky HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Úloha 1 – Vytvoření StoreManagerController

V této úloze se vytvoří nový řadič názvem **StoreManagerController** pro podporu operací CRUD.

1. Otevřete **začít** řešení nacházející se v **zdroj/Ex1-CreatingTheStoreManagerController/počáteční/** složky.

   1. Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Přidání nového řadiče. Chcete-li to provést, klikněte pravým tlačítkem **řadiče** složky v Průzkumníku řešení, vyberte **přidat** a potom **řadič** příkaz. Změna **řadič** **název** k **StoreManagerController** a zajistěte, aby možnost **kontroler MVC s akcemi čtení/zápisu-prázdný**je vybrána. Klikněte na tlačítko **přidat**.

    ![Dialogové okno Přidat řadič](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "dialogové okno Přidat kontroler")

    *Dialogové okno řadiče přidání*

    Generuje se nové třídy Kontroleru. Vzhledem k tomu, že uvedené přidat akce pro čtení a zápis, se zakázaným inzerováním metody pro ty, jsou vytvořeny běžné akce CRUD s komentáře TODO vyplněno, výzvy zahrnout určitou logiku aplikace.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Úloha 2 – přizpůsobení StoreManager indexu

V této úloze budete přizpůsobovat StoreManager Index metody akce lze vrátit zobrazení se seznamem alb z databáze.

1. Ve třídě StoreManagerController přidejte následující *pomocí* direktivy.

    (Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování – Ex1 pomocí MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Přidat pole do **StoreManagerController** pro uložení instanci **MusicStoreEntities.**

    (Code fragment kódu - *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování – Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implementujte StoreManagerController indexu akce lze vrátit zobrazení se seznamem alb.

    Logice Kontroleru akce bude velmi podobná akce indexu StoreController zapsána dříve. Umožňuje načíst všechny alb, včetně informací o Genre a umělcem pro zobrazení LINQ.

    (Code fragment kódu - *pomocné rutiny ASP.NET MVC 4 a formulářů a ověřování – Ex1 StoreManagerController Index*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Úloha 3 – vytvoření zobrazení pro Index

V této úloze, vytvoříte šablony zobrazení Index pro zobrazení seznamu alb vrácený **StoreManager** řadiče.

1. Před vytvořením nové šablony zobrazení, měli byste vytvořit projekt tak, aby **Přidat Dialog zobrazení** zná **Album** třídu se má použít. Vyberte **sestavení | Sestavení MvcMusicStore** a tím projekt sestavit.
2. Klepněte pravým tlačítkem myši **Index** metody akce a vyberte **přidat zobrazení**. Tím se otevře **přidat zobrazení** dialogové okno.

    ![Přidání zobrazení](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "přidat zobrazení")

    *Přidání zobrazení v aplikaci Index – metoda*
3. V dialogovém okně Přidat zobrazení, ověřte, zda je název zobrazení **Index**. Vyberte **vytvořit zobrazení silného typu** a vyberte možnost **Album (MvcMusicStore.Models)** z **třída modelu** rozevíracího seznamu. Vyberte **seznamu** z **vygenerované uživatelské rozhraní šablony** rozevíracího seznamu. Ponechte **zobrazovací modul** k **Razor** a v ostatních polích s jejich výchozí hodnoty a pak klikněte na tlačítko **přidat**.

    ![Přidání zobrazení index se o](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "přidání zobrazení indexu")

    *Přidání zobrazení indexu*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Úloha 4 – přizpůsobení vygenerovaného uživatelského rozhraní zobrazení indexu

V této úloze se upraví jednoduché zobrazení šablony vytvořené pomocí funkce generování uživatelského rozhraní ASP.NET MVC jej pole, které chcete zobrazit.

> [!NOTE]
> **Generování uživatelského rozhraní** podpory v rámci rozhraní ASP.NET MVC vygeneruje šablonu jednoduché zobrazení, která uvádí všechna pole v modelu alb. **Generování uživatelského rozhraní** poskytuje rychlý způsob, jak začít pracovat na zobrazení se silnými typy: místo nutnosti psaní zobrazit šablonu ručně, generování uživatelského rozhraní rychle generuje výchozí šablonu a upravte generovaného kódu.


1. Zkontrolujte kód vytvořený. Vygenerovaný seznam polí bude součástí následující tabulky HTML, který **generování uživatelského rozhraní** používá pro zobrazení tabulková data.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Nahraďte **&lt;tabulky&gt;** kódu pomocí následující kód k zobrazení pouze **Genre**, **umělcem**, **název alba**, a **cena** pole. To odstraní **AlbumId** a **alb obrázky URL** sloupce. Také změní GenreId a ArtistId sloupce k zobrazení jejich vlastnosti propojené třída **Artist.Name** a **Genre.Name**a odebere **podrobnosti** odkaz.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Změňte v následujících popisech.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Úloha 5: spuštění aplikace

V této úloze budete testovat, **StoreManager** **Index** zobrazit šablonu zobrazí seznam alb podle návrhu předchozí kroky.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager** k ověření, že se zobrazí seznam alb, zobrazení jejich **název**, **umělcem** a **Genre**.

    ![Procházení seznamu alb](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "procházení seznam alb")

    *Procházení seznamu alb*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Cvičení 2: Přidání pomocné rutiny HTML

StoreManager indexovou stránku má jeden potenciální potíže: umělcem název a název vlastnosti může být dostatečně dlouhé, aby throw vypnout formátování tabulky. V tomto cvičení se dozvíte, jak přidat vlastní pomocné rutiny HTML zkrátit tento text.

Na následujícím obrázku vidíte, jak formát se mění z důvodu délka textu při použití na velikost malých prohlížeče.

![Procházení seznamu alb s nezkrátila text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "procházení seznam alb s nezkrátila textu")

*Procházení seznamu alb s nezkrátila textu*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Úloha 1 – rozšíření pomocné rutiny HTML

V této úloze budete přidávat nové metody **Truncate** k **HTML** objekt vystavený v zobrazení ASP.NET MVC. K tomu budete implementovat **metoda rozšíření** do vestavěné **System.Web.Mvc.HtmlHelper** třída poskytuje rozhraní ASP.NET MVC.

> [!NOTE]
> Další informace o **rozšiřující metody**, navštivte tohoto článku na webu msdn. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Otevřete **začít** řešení nacházející se v **zdroj/Ex2-AddingAnHTMLHelper/počáteční/** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřete zobrazení StoreManager na Index. Chcete-li to provést, v Průzkumníku řešení rozbalte **zobrazení** složku, pak se **StoreManager** a otevřete **Index.cshtml** souboru.
3. Přidejte následující kód níže <strong>@model</strong> – direktiva k definování <strong>Truncate</strong> metodu helper.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Úloha 2 - zkracování textu na stránce

V této úloze budete používat **Truncate** metoda zkrátit text v šabloně zobrazení.

1. Otevřete zobrazení StoreManager na Index. Chcete-li to provést, v Průzkumníku řešení rozbalte **zobrazení** složku, pak se **StoreManager** a otevřete **Index.cshtml** souboru.
2. Nahradit řádky, které ukazují **umělcem název** a alba **název**. Chcete-li to provést, nahraďte následující řádky.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Úloha 3 – spuštění aplikace

V této úloze budete testovat, **StoreManager** **Index** zobrazit šablonu zkrátí titul alba a umělcem jméno.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager** k ověření tohoto dlouho texty v **název** a **umělcem** sloupec se zkrátí.

    ![Zkrátí názvy produktů a umělci](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "zkrácen názvy produktů a umělci")

    *Zkrácený názvy a umělcem názvy*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Cvičení 3: Vytvoření zobrazení pro úpravy

V tomto cvičení se dozvíte, jak vytvořit formulář umožňuje správcům úložiště upravit Album. Bude procházení **/StoreManager/Edit/id** adresa URL (**id** probíhá jedinečné id alba, chcete-li upravit), díky čemuž volání GET protokolu HTTP na server.

Bude metoda akce Kontroleru upravit načtení příslušné Album z databáze, vytvořte **StoreManagerViewModel** objekt, který chcete zapouzdření (spolu s seznam umělci a žánry) a předejte ji do šablonu zobrazení vykreslení stránky HTML zpět na uživatele. Tato stránka bude obsahovat **&lt;formuláře&gt;** element s textových polí a rozevírací seznamy pro úpravy vlastnosti alba.

Jakmile je uživatel aktualizace hodnot formuláře alba a klikne **Uložit** tlačítko, změny se odešlou přes HTTP POST zpětné volání pro **/StoreManager/Edit/id**. I když adresa URL zůstává stejná jako v posledním volání, ASP.NET MVC identifikuje, že tuto chvíli je HTTP POST a proto provede jinou metodu akce úpravy (jeden označených pomocí **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Úloha 1 – implementace metody akce HTTP GET úpravy

V této úloze budete implementovat verze HTTP GET metody akce úpravy k načtení příslušné Album z databáze, a také seznam všech žánry a umělci. Tato data se bude balíček do **StoreManagerViewModel** objekt definovaný v posledním kroku, který se potom předá šablonu zobrazení k vykreslení odpovědi s.

1. Otevřete **začít** řešení nacházející se v **zdroj/EX3.-CreatingTheEditView/počáteční/** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřete **StoreManagerController** třídy. Chcete-li to provést, rozbalte **řadiče** složku a dvojím kliknutím **StoreManagerController.cs**.
3. Nahraďte **HTTP GET upravit** metoda akce s následující kód k načtení příslušné **Album** společně s **žánry** a **umělci**uvádí.

    (Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování – EX3. StoreManagerController HTTP GET upravit akce*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Používáte **System.Web.Mvc** **SelectList** umělci a žánry místo **System.Collections.Generic** seznamu.
    > 
    > **SelectList** je čisticí způsob, jak naplnit rozevírací seznamy HTML a spravovat takové věci, jako aktuální výběr. Vytváření instancí a novější nastavením tyto objekty ViewModel v akce kontroleru bude scénář formuláře upravit čisticí.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Úloha 2 – Vytvoření zobrazení pro úpravy

V této úloze vytvoříte šablonu upravit zobrazení, které se zobrazí později vlastnosti alba.

1. Vytvoření zobrazení pro úpravy. Chcete-li to provést, klikněte pravým tlačítkem uvnitř **upravit** metody akce a vyberte **přidat zobrazení**.
2. V dialogovém okně Přidat zobrazení, ověřte, zda je název zobrazení **upravit**. Zkontrolujte **vytvořit zobrazení silného typu** zaškrtávací políčko a vyberte **Album (MvcMusicStore.Models)** z **zobrazit třída dat** rozevíracího seznamu. Vyberte **upravit** z **vygenerované uživatelské rozhraní šablony** rozevíracího seznamu. V ostatních polích nechte výchozí hodnoty a potom klikněte na **přidat**.

    ![Přidání zobrazení](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "přidávání zobrazení")

    *Přidání zobrazení*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Úloha 3 – spuštění aplikace

V této úloze budete testovat, **StoreManager** **upravit** stránka zobrazení zobrazí vlastnosti hodnoty alba předán jako parametr.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager/Edit/1** k ověření, že se zobrazují vlastnosti hodnoty alba předán.

    ![Procházení zobrazení alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "procházení alba upravit zobrazení")

    *Procházení alba upravit zobrazení*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Úloha 4 – implementace rozevírací seznamy v šabloně Album editoru

V této úloze přidáte rozevírací seznamy do zobrazení šablony vytvořené v posledním úkolem tak, aby si uživatel může vybrat ze seznamu umělci a žánry.

1. Nahraďte všechny **Album** fieldset kódu pomocí následující:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > **Html.DropDownList** pomocná byla přidána k vykreslení rozevírací seznamy pro výběr umělci a žánry. Parametry předaný **Html.DropDownList** jsou:
    > 
    > 1. Název pole formuláře (**&quot;ArtistId&quot;**).
    > 2. **SelectList** hodnot pro rozevíracího seznamu.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Úloha 5: spuštění aplikace

V této úloze budete testovat, **StoreManager** **upravit** stránka zobrazení zobrazí rozevírací seznamy místo umělcem a Genre ID textových polí.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager/Edit/1** a ověřte, že ho rozevírací seznamy místo umělcem a Genre ID textových polí.

    ![Procházení alba upravit zobrazení s rozevírací nabídky](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "procházení Album upravit zobrazení s rozevírací nabídky")

    *Procházení Album zobrazení, tentokrát pomocí rozevíracích seznamů*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Úloha 6 – implementace metody akce HTTP POST upravit

Teď, když upravit zobrazení zobrazí podle očekávání, potřebujete implementovat metodu akce HTTP POST upravit uložit změny provedené album.

1. Zavřete prohlížeč v případě potřeby se vraťte do okna Visual Studio. Otevřete **StoreManagerController** z **řadiče** složky.
2. Nahraďte **HTTP POST upravit** kódu metoda akce s následující (Všimněte si, že je metoda, která se musí nahradit přetížené verze, která přijímá dva parametry):

    (Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování – EX3. StoreManagerController HTTP POST upravit akce*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Tato metoda se provede, když uživatel klikne **Uložit** tlačítko zobrazení a provede POST protokolu HTTP-hodnot formuláře zpět na server aby je uchoval v databázi. Dekoratéra **[HttpPost]** označuje, že metoda by měla použít pro tyto scénáře HTTP POST. Tato metoda přebírá **Album** objektu. ASP.NET MVC automaticky vytvoří objekt Album z odeslaných &lt;formuláře&gt; hodnoty.
    > 
    > Metoda provede tyto kroky:
    > 
    > 1. Pokud je model platný:
    > 
    >     1. Aktualizujte položku album v kontextu označit jako upravený objekt.
    >     2. Uložte změny a přesměruje na zobrazení indexu.
    > 2. Pokud model není platný, naplní objekt ViewBag s **GenreId** a **ArtistId**, pak vrátí zobrazení s přijatého objektu Album uživatelům provádět všechny požadované aktualizace.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Úloha 7 – spuštění aplikace

V této úloze budete testovat, **StoreManager úpravy** stránka zobrazení aktualizovaných dat alb ve skutečnosti uloží v databázi.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager/Edit/1**. Změna názvu alba k **zatížení** a klikněte na **Uložit**. Ověřte, že název alba je ve skutečnosti změnila v seznamu alb.

    ![Aktualizace album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "aktualizace album")

    *Aktualizace Album*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Cvičení 4: Přidání vytvořit zobrazení

Teď, když **StoreManagerController** podporuje **upravit** možnost, v tomto cvičení se dozvíte, jak přidat šablonu vytvořit zobrazení umožníte ukládání správci přidat nové alb k aplikaci.

Stejně jako s funkcemi upravit implementujete scénář vytvořit pomocí dvou samostatných metod v rámci **StoreManagerController** třídy:

1. Jedna metoda akce se zobrazí formuláře prázdný, když správci úložiště nejprve navštíví **/StoreManager/vytvořit** adresy URL.
2. Druhé metody akce zpracuje scénář, kde správce obchodu klikne **Uložit** tlačítko ve formuláři a odešle hodnoty zpět do **/StoreManager/vytvořit** adresu URL jako HTTP POST.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Úloha 1 – implementace metody GET protokolu HTTP vytvořit akce

V této úloze budete implementovat HTTP GET verzi vytvořit metody akce k načtení seznamu všech žánry a umělci, tato data zabalit do **StoreManagerViewModel** objekt, který bude předána do zobrazení šablony.

1. Otevřete **začít** řešení nacházející se v **zdroj/Ex4-AddingACreateView/počáteční/** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřete **StoreManagerController** třídy. Chcete-li to provést, rozbalte **řadiče** složku a dvojím kliknutím **StoreManagerController.cs**.
3. Nahraďte **vytvořit** kódu metoda akce následujícím kódem:

    (Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování - Ex4 StoreManagerController HTTP GET vytvořit akce*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Úloha 2 – Přidání zobrazení pro vytváření

V této úloze přidá šablony vytvořit zobrazení, která se zobrazí nový formulář Album (prázdný).

1. Klepněte pravým tlačítkem myši **vytvořit** metody akce a vyberte **přidat zobrazení**. Tím se otevře dialogové okno Přidat zobrazení.
2. V dialogovém okně Přidat zobrazení, ověřte, zda je název zobrazení **vytvořit**. Vyberte **vytvořit zobrazení silného typu** možnost a vyberte **Album (MvcMusicStore.Models)** z **třída modelu** rozevíracího seznamu a **vytvořit** z **vygenerované uživatelské rozhraní šablony** rozevíracího seznamu. V ostatních polích nechte výchozí hodnoty a potom klikněte na **přidat**.

    ![Přidání zobrazení vytvořit](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "přidání-create-view.png")

    *Přidání zobrazení pro vytváření*
3. Aktualizace **GenreId** a **ArtistId** pole k použití rozevíracího seznamu, jak je uvedeno níže:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Úloha 3 – spuštění aplikace

V této úloze budete testovat, **StoreManager** **vytvořit** stránka zobrazení zobrazí prázdné formuláře alb.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager/vytvořit**. Ověřte, zda formuláře prázdný zobrazí pro naplnění nové vlastnosti alb.

    ![Vytvoření zobrazení s formuláře prázdný](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "vytvořit zobrazení s prázdnou formuláře")

    *Vytvoření zobrazení pomocí prázdného formuláře*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Úloha 4 – implementace HTTP POST vytvořit metody akce

V této úloze budete implementovat HTTP POST verzi vytvořit metody akce, která bude volána, když uživatel klikne **Uložit** tlačítko. Metoda měli uložit nové album v databázi.

1. Zavřete prohlížeč v případě potřeby se vraťte do okna Visual Studio. Otevřete **StoreManagerController** třídy. Chcete-li to provést, rozbalte **řadiče** složku a dvojím kliknutím **StoreManagerController.cs**.
2. Nahraďte **HTTP POST vytvořit** kódu metoda akce následujícím kódem:

    (Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování – Ex4 StoreManagerController HTTP POST vytvořením akce*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > Akce vytvoření je poměrně podobná předchozích úprav metody akce, ale místo objekt nastavení, jako jsou změny, se přidá do kontextu.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Úloha 5: spuštění aplikace

V této úloze budete testovat, **StoreManager vytvořit** stránka zobrazení umožňuje vytvářet nové Album a pak přesměruje na zobrazení StoreManager Index.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager/vytvořit**. Pro nové Album, jako na následujícím obrázku vyplníte všechna pole formuláře s daty:

    ![Vytváření alba](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "vytváření alba")

    *Vytváření alba*
3. Ověřte, že budete přesměrováni na StoreManager Index zobrazení, které obsahuje nové Album právě vytvořili.

    ![Vytvořit nové Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "nové Album vytvořen")

    *Vytvořit nové Album*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Cvičení 5: Zpracování odstranění

Umožňuje odstranit alb není dosud implementována. Je to, co toto cvičení bude o. Jako před, budete implementovat scénáře odstranit pomocí dvou samostatných metod v rámci **StoreManagerController** třídy:

1. Jedna metoda akce se zobrazí potvrzení formuláře
2. Druhé metody akce, bude zpracovávat odeslání formuláře

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Úloha 1 – implementace metody akce Delete HTTP GET

V této úloze budete implementovat verze HTTP GET metody akce odstranění, načtení informací o alba.

1. Otevřete **začít** řešení nacházející se v **zdroj/Ex5-HandlingDeletion/počáteční/** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřete **StoreManagerController** třídy. Chcete-li to provést, rozbalte **řadiče** složku a dvojím kliknutím **StoreManagerController.cs**.
3. Akce kontroleru odstranění je přesně stejný jako předchozí akce kontroleru podrobnosti úložiště: odešle dotaz **album** objekt z databáze pomocí **id** součástí adresy URL a vrátí odpovídající **zobrazení**. Chcete-li to provést, nahraďte HTTP-GET **odstranit** kódu metoda akce s následující:

    (Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování - Ex5 zpracování odstranění – odstranit HTTP GET – akce*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Klepněte pravým tlačítkem myši **odstranit** metody akce a vyberte **přidat zobrazení**. Tím se otevře dialogové okno Přidat zobrazení.
5. V dialogovém okně Přidat zobrazení, ověřte, zda je název zobrazení **odstranit**. Vyberte **vytvořit zobrazení silného typu** možnost a vyberte **Album (MvcMusicStore.Models)** z **třída modelu** rozevíracího seznamu. Vyberte **odstranit** z **vygenerované uživatelské rozhraní šablony** rozevíracího seznamu. V ostatních polích nechte výchozí hodnoty a potom klikněte na **přidat**.

    ![Přidání zobrazení odstranění](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "přidání odstranit zobrazení")

    *Přidání zobrazení odstranění*
6. Odstranit šablonu zobrazí všechna pole z modelu. Se zobrazí pouze alba název. Chcete-li to provést, nahradí obsah zobrazení s následujícím kódem:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Úloha 2 – spuštění aplikace

V této úloze budete testovat, **StoreManager** **odstranit** stránka zobrazení zobrazí potvrzení odstranění formuláře.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager**. Vyberte jeden album odstranit kliknutím **odstranit** a ověřte, zda je nahrán nové zobrazení.

    ![Odstraňování Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "odstraňování Album")

    *Odstraňování Album*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Úloha 3 – implementace metody akce Delete HTTP POST

V této úloze budete implementovat verze HTTP POST metodě akce odstranění, která bude volána, když uživatel klikne **odstranit** tlačítko. Metoda, měli byste odstranit album v databázi.

1. Zavřete prohlížeč v případě potřeby se vraťte do okna Visual Studio. Otevřete **StoreManagerController** třídy. Chcete-li to provést, rozbalte **řadiče** složku a dvojím kliknutím **StoreManagerController.cs**.
2. Nahraďte **HTTP POST odstranit** kódu metoda akce následujícím kódem:

    (Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování – Ex5 zpracování odstranění – odstranit HTTP POST – akce*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Úloha 4 – spuštění aplikace

V této úloze budete testovat, **StoreManager odstranění** stránka zobrazení umožňuje odstranění alba a pak přesměruje na zobrazení StoreManager Index.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager**. Vyberte jeden album odstranit kliknutím **odstranit.** Potvrďte odstranění kliknutím **odstranit** tlačítko:

    ![Odstraňování Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "odstraňování Album")

    *Odstraňování Album*
3. Ověřte, že alba byl odstranit, protože v nezobrazí **Index** stránky.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Cvičení 6: Přidání ověření

Vytvořit a upravit formuláře, které mají zavedené v současné době neprovádět jakýkoli druh ověřování. Pokud uživatel odejde povinné pole, které jsou prázdné nebo písmena typ pole cena, první chyba, kterou budete mít bude z databáze.

Přidáním datových poznámek na třídu modelu, můžete přidat ověřování do aplikace. Povolit datových poznámek popisující pravidla, která chcete použít pro vaše vlastnosti modelu a ASP.NET MVC se postará o vynucení a zobrazení odpovídající zprávu pro uživatele.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Úloha 1 – Přidání datových poznámek

V této úloze přidáte, že datových poznámek Album modelu, který bude stránka vytvořit a upravit zobrazení ověřovacích zpráv v případě nutnosti.

Pro třídu modelu jednoduché přidávání datové poznámky se právě zpracovává přidáním **pomocí** příkaz pro **System.ComponentModel.DataAnnotation**, pak umístění **[požadované]** atribut u příslušných vlastností. V následujícím příkladu by zkontrolujte **název** vlastnost povinné pole v zobrazení.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Toto je trochu složitější v případech, jako je tato aplikace, kde se vygeneruje datového modelu Entity. Pokud jste přidali datových poznámek přímo do třídy modelu, pokud aktualizovat model z databáze se budou přepsána. Místo toho můžete provést pomocí metadat částečné třídy, které budou pro uložení poznámky existují a jsou přidruženy k modelu třídy pomocí **[MetadataType]** atribut.

1. Otevřete **začít** řešení nacházející se v **zdroj/Ex6-AddingValidation/počáteční/** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřete **Album.cs** z **modely** složky.
3. Nahraďte **Album.cs** obsahu se zvýrazněný kód tak, aby vypadal jako následující:

    > [!NOTE]
    > Na řádku **[DisplayFormat(ConvertEmptyStringToNull=false)]** označuje, že prázdné řetězce z modelu nebudou převedeny na hodnotu null při aktualizaci datové pole v datovém zdroji. Toto nastavení zabrání výjimku, pokud rozhraní Entity Framework před datové poznámky ověří pole přiřadí hodnoty null do modelu.

    (Code fragment kódu - *pomocné rutiny pro aplikaci ASP.NET MVC 4 a formulářů a ověřování - třídu metadat Ex6 Album*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > To **Album** má třídu **MetadataType** atribut, který odkazuje na **AlbumMetaData** třídu pro datových poznámek. Toto jsou některé z datové poznámky atributy, které použijete k přidání poznámek ke Album modelu:
    > 
    > - Požadováno – označuje, že vlastnost je povinné pole.
    > - DisplayName – definuje text, který se použije na pole formuláře a ověřovacích zpráv
    > - DisplayFormat - Určuje, jak zobrazit a formátovány datová pole.
    > - StringLength - definuje maximální délku pole řetězce
    > - V rozsahu – poskytuje maximální a minimální hodnotu pro číselné pole
    > - ScaffoldColumn – umožňuje skrytí pole z editoru formulářů

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Úloha 2 – spuštění aplikace

V této úloze budete testovat, se stránky vytvořit a upravit ověření pole, použijte názvy zobrazení vybrali v posledním úkolem.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/StoreManager/vytvořit**. Ověřte, že zobrazované názvy odpovídají těm, které jsou v třídu (jako je **alb obrázky URL** místo **AlbumArtUrl**)
3. Klikněte na tlačítko **vytvořit**, bez vyplnění formuláře. Ověřte, že dostanete odpovídající ověřovacích zpráv.

    ![Ověření pole na stránce vytvořit](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "ověření pole na stránce vytvořit")

    *Ověřené polí na stránce vytvořit*
4. Můžete ověřit, že stejné vyskytuje v **upravit** stránky. Změňte adresu URL na **/StoreManager/Edit/1** a ověřte, že zobrazované názvy odpovídají těm, které jsou v třídu (jako je **alb obrázky URL** místo **AlbumArtUrl**). Prázdný **název** a **cena** pole a klikněte na tlačítko **Uložit**. Ověřte, že dostanete odpovídající ověřovacích zpráv.

    ![Ověřené polí na stránce Upravit](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Ověřené polí na stránce Upravit*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Cvičení 7: Pomocí Nerušivý jQuery na straně klienta

V tomto cvičení se dozvíte, jak povolit MVC 4 Nerušivý jQuery ověření na straně klienta.

> [!NOTE]
> Nerušivý jQuery používá data-ajax předponu JavaScript volat metody akce na serveru místo intrusively generování vložené skripty klienta.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Úloha 1 – spuštění aplikace před povolením Nerušivý jQuery

V této úloze budete spouštět aplikaci před včetně jQuery k porovnání obou modelů ověření.

1. Otevřete **začít** řešení nacházející se v **zdroj/Ex7-UnobtrusivejQueryValidation/počáteční/** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Stiskněte klávesu **F5** ke spuštění aplikace.
3. Projekt se spustí na domovské stránce. Procházet **/StoreManager/vytvořit** a klikněte na tlačítko **vytvořit** bez vyplnění formuláře a přesvědčte se, abyste měli ověřovacích zpráv:

    ![Ověření klienta zakázaná](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "zakázáno ověření klienta")

    *Ověření klienta zakázána*
4. V prohlížeči otevřete zdrojový kód HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Úloha 2 – povolení ověření Nerušivého klienta

V této úloze povolíte jQuery **ověření nerušivého klienta** z **Web.config** souboru, který je ve výchozím nastavení nastavena na hodnotu false na všechny nové projekty ASP.NET MVC 4. Kromě toho budete přidávat, že nezbytné skriptů odkazy aby jQuery pracovní Nerušivý ověření klienta.

1. Otevřete **Web.Config** souborů v kořenu projektu a ujistěte se, že **ClientValidationEnabled** a **UnobtrusiveJavaScriptEnabled** klíče hodnoty jsou nastaveny na **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Kód v Global.asax.cs na stejné výsledky můžete také povolit ověření klienta:
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > Kromě toho můžete přiřadit atribut ClientValidationEnabled do libovolného řadiče do mají vlastní chování.
2. Otevřete **Create.cshtml** v **Views\StoreManager**.
3. Zajistěte, aby následující soubory skriptu, **jquery.validate** a **jquery.validate.unobtrusive**, se odkazuje v zobrazení přes &quot; **~/bundles/jqueryval** &quot; sady.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Tyto knihovny jQuery jsou součástí nové projekty MVC 4. Můžete najít další knihovny v **/skriptů** složky můžete projektu.
    > 
    > Aby bylo možné toto ověření knihovny práci, budete muset přidat odkaz na knihovnu jQuery framework. Vzhledem k tomu, že tento odkaz je již přidán do  **\_Layout.cshtml** souboru, není nutné přidat do tohoto zobrazení.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Úloha 3 – spuštění aplikace pomocí Nerušivý jQuery ověření

V této úloze budete testovat, **StoreManager** vytvořit zobrazení šablony provádí ověřování na straně klienta pomocí knihovny jQuery, když uživatel vytváří nové album.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Procházet **/StoreManager/vytvořit** a klikněte na tlačítko **vytvořit** bez vyplnění formuláře a přesvědčte se, abyste měli ověřovacích zpráv:

    ![Ověření klienta s jQuery povoleno](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "ověření klienta s jQuery povoleno")

    *Ověření klienta s jQuery povoleno*
3. V prohlížeči otevřete zdrojový kód pro vytvoření zobrazení:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Pro každý ověřovací pravidlo klienta Nerušivý jQuery přidá atribut data-val -*rulename*=&quot;*zpráva*&quot;. Níže je seznam značek této Unobtrusive jQuery vloží do vstupní pole html k provedení ověření klienta:
   > 
   > - Val dat
   > - Číslo datového val
   > - Oblast dat val
   > - Data-val rozsah min / Data-val rozsah max
   > - Potřeba val dat
   > - Délka dat val
   > - Data-val délka max / Data-val délka min
   > 
   > Všechny hodnoty data jsou vyplněny modelu **datové poznámky**. Potom všechny logiky, která pracuje na straně serveru může být spuštěna na straně klienta. Například cena atribut má následující datové poznámky v modelu:
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

Provedením tohoto testovacího prostředí Hands-On jste se naučili, jak povolit uživatelům změnit data uložená v databázi s použitím následujících akcí:

- Akce kontroleru jako Index, vytvořit, upravit, odstranit
- Funkce generování uživatelského rozhraní ASP.NET MVC pro zobrazení vlastností do tabulky HTML
- Vlastní pomocné rutiny HTML ke zlepšení uživatelského prostředí
- Metody akce, které reagují na HTTP GET nebo POST protokolu HTTP volání
- Šablonu sdílené editor pro podobné zobrazení šablon, například vytvoření a úpravy
- Elementy Form jako rozevírací seznamy
- Anotací dat při ověřování modelu
- Ověřování na straně klienta pomocí Nerušivý knihovny jQuery

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nyní nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.

    ![Nainstalovat Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "nainstalovat Visual Studio Express")

    *Nainstalovat Visual Studio Express*
4. Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Vyjádření souhlasu s podmínkami licence](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Vyjádření souhlasu s podmínkami licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Průběh instalace*
6. Po dokončení instalace, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.

    ![VS Express pro Web dlaždice](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express pro Web dlaždice*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Příloha B: používání fragmentů kódu

S fragmenty kódu máte všechny kód, který je nutné na dosah ruky. Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")

*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.
4. Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).
5. Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.

![Začněte psát název fragmentu](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")

*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*

![Stisknutím klávesy Tab znovu a fragmentu rozšíří](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")

*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.
2. Vyberte relevantní fragment kódu ze seznamu, kliknutím na.

![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")

*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*
