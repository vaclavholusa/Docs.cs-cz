---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 – základy | Dokumentace Microsoftu
author: rick-anderson
description: Tohoto praktického testovacího prostředí je založena na MVC (Model View Controller) Music Store kurz aplikace, která představuje a najdete podrobné pokyny ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 3a282d02ba929eb86571e92f190550614962524d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386572"
---
# <a name="aspnet-mvc-4-fundamentals"></a>ASP.NET MVC 4 – základy

podle [Campy Web týmu](https://twitter.com/webcamps)

[Stáhněte si Web Campy školení Kit](https://aka.ms/webcamps-training-kit)

Tohoto praktického testovacího prostředí je založena na MVC (Model View Controller) Music Store aplikace, která představuje a vysvětluje krok za krokem, jak pomocí technologie ASP.NET MVC a sady Visual Studio. V rámci testovacího prostředí se dozvíte, jednoduchost, ještě výkonu společně použití těchto technologií. Začne fungovat s jednoduchou aplikaci a budou sestavte ho, dokud máte plně funkční technologie ASP.NET MVC 4 webovou aplikaci.

Toto testovací prostředí funguje s ASP.NET MVC 4.

Pokud chcete prozkoumat verze technologie ASP.NET MVC 3 kurz aplikace, najdete ho v [MVC. Music Store](https://github.com/evilDave/MVC-Music-Store).

Tohoto praktického testovacího prostředí se předpokládá, že vývojář má prostředí v oblasti technologií vývoje webových, jako je HTML a JavaScript.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [základy ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>Aplikace Music Store

Webová aplikace Music Store, které budou vytvořeny v rámci tohoto testovacího prostředí zahrnuje tři hlavní části: nákupní, registrace a správa. Uživatelé budou moci procházet podle žánru alb, přidat alb jejich košíku, zkontrolujte jejich výběr a nakonec jich přešlo k platbě přihlásit se a dokončete pořadí. Kromě toho úložiště správci budou moct spravovat dostupné alb, jakož i jejich hlavní vlastnosti.

![Obrazovky Music Store](aspnet-mvc-4-fundamentals/_static/image1.png "obrazovky Music Store")

*Obrazovky Music Store*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>Základy ASP.NET MVC 4

Aplikace Music Store bude vytvořen pomocí **řadiče MVC (Model View)**, schéma architektury, který rozděluje aplikace do tří hlavních součástí:

- **Modely**: objekty modelů jsou části aplikace, které implementují logiku domény. Objekty modelů často, také načíst a ukládají stav modelu v databázi.
- **Zobrazení:** zobrazení jsou komponenty, které zobrazují aplikace uživatelského rozhraní (UI). Standardně toto uživatelské rozhraní je vytvořena z dat modelu. Příkladem může být zobrazení pro úpravy alb, které zobrazuje textová pole a rozevírací seznam na základě aktuálního stavu objektu alba.
- **Řadiče:** Kontrolery jsou komponenty, které zpracovávají interakci s uživatelem, pracovat s modelem a konečně vybírají vykreslené uživatelské rozhraní zobrazení. V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce.

Vzor MVC pomáhá vytvářet aplikace, které separují jednotlivé aspekty aplikace (logika vstupu, obchodní logika a logika uživatelského rozhraní) současně poskytují volné párování mezi těmito elementy. Tato separace pomáhá zvládnout složitost při sestavování aplikace, protože umožňuje vám umožní zaměřit se na jeden aspekt jejich implementace v čase. Kromě toho vzor MVC usnadňuje testování aplikací také podporují použití vývoj řízený testováním (TDD) pro vytváření aplikací.

**ASP.NET MVC** framework představuje alternativu ke vzoru webových formulářů ASP.NET pro vytvoření webové aplikace založené na ASP.NET MVC. **ASP.NET MVC** framework je jednoduchý, s možností intenzivního testování prezentační platforma, která (stejně jako u aplikací na základě webových formulářů) je integrovaná s stávajících funkcí technologie ASP.NET, jako je například stránky předlohy a na základě členství ověřování, takže získáte všechny funkce rozhraní .NET core. To je užitečné, pokud jste už obeznámení s webovými formuláři ASP.NET protože všech knihoven, které už používáte jsou také k dispozici v architektuře ASP.NET MVC 4.

Kromě toho volné párování mezi třemi hlavními komponentami architektury MVC rovněž podporuje paralelní vývoj. Například jeden vývojář může pracovat na zobrazení, druhý může pracovat na logice kontroleru a třetí se můžete soustředit na obchodní logiku v modelu.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto praktického testovacího prostředí se dozvíte, jak:

- Vytvoření aplikace ASP.NET MVC zcela podle kurzu aplikace Music Store
- Přidat Kontrolery pro zpracování adresy URL na domovskou stránku webu a jeho hlavní funkce procházení
- Přidání zobrazení upravit obsah zobrazený spolu s jeho styl
- Přidání třídy modelu obsahovat a spravovat data a domény logiku
- Použití vzoru modelu zobrazení k předávání informací z akce kontroleru zobrazení šablon
- Prozkoumejte nové šablony ASP.NET MVC 4 pro internetové aplikace

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Musíte mít následující položky k dokončení tohoto testovacího prostředí:

- [Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Instalace

**Instalace fragmenty kódu**

Pro usnadnění práce velkou část kódu, které budete spravovat podél tohoto testovacího prostředí je k dispozici jako fragmenty kódu sady Visual Studio. K instalaci spustit fragmenty kódu **.\Source\Setup\CodeSnippets.vsi** souboru.

Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha C:](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Podle následující praktická cvičení se skládá tohoto praktického testovacího prostředí:

1. [Cvičení 1: Vytvoření projektu webové aplikace MusicStore ASP.NET MVC](#Exercise1)
2. [Cvičení 2: Vytvoření Kontroleru](#Exercise2)
3. [Cvičení 3: Předání parametrů do Kontroleru](#Exercise3)
4. [Cvičení 4: Vytvoření zobrazení](#Exercise4)
5. [Cvičení 5: Vytvoření modelu zobrazení](#Exercise5)
6. [Cvičení 6: Použití parametrů v zobrazení](#Exercise6)
7. [Cvičení 7: Kolečko okolo nové šablony ASP.NET MVC 4](#Exercise7)

> [!NOTE]
> Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.


Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Cvičení 1: Vytvoření projektu webové aplikace MusicStore ASP.NET MVC

V tomto cvičení se dozvíte, jak vytvořit aplikaci ASP.NET MVC v sadě Visual Studio 2012 Express pro Web, stejně jako jeho hlavní složky organizace. Kromě toho se dozvíte, jak přidat nový kontroler a nastavte ji zobrazit na domovské stránce aplikace jednoduchým řetězcem.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Úloha 1 – Vytvoření projektu webové aplikace ASP.NET MVC

1. V této úloze vytvoříte prázdný projekt aplikace ASP.NET MVC pomocí šablony MVC Visual Studio. Spustit **VS Express for Web**.
2. Na **souboru** nabídky, klikněte na tlačítko **nový projekt**.
3. V **nový projekt** dialogové okno Vyberte **webové aplikace ASP.NET MVC 4** typ nachází v rámci projektu **Visual C#,** **webové** šablony seznam.
4. Změnit **název** k *MvcMusicStore*.
5. Nastavit umístění řešení uvnitř nové **začít** složky v tomto cvičení zdrojové složky, například **[YOUR-Mezidobí-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**. Klikněte na tlačítko **OK**.

    ![Vytvoření dialogového okna Nový projekt](aspnet-mvc-4-fundamentals/_static/image2.png "vytvořit dialogové okno Nový projekt")

    *Vytvoření dialogového okna Nový projekt*
6. V **nového projektu ASP.NET MVC 4** dialogové okno Vyberte **základní** šablony a ujistěte se, že **zobrazovací modul** vybraného je **Razor**. Klikněte na tlačítko **OK**.

    ![ASP.NET MVC 4 projektu dialogové okno Nový](aspnet-mvc-4-fundamentals/_static/image3.png "nové pole dialogové okno projektu ASP.NET MVC 4")

    *Nové pole dialogové okno projektu ASP.NET MVC 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Úloha 2 – zkoumání struktury řešení

Architektura ASP.NET MVC zahrnuje šablony projektu sady Visual Studio, který vám pomůže vytvořit webové aplikace, které podporují vzor MVC. Tato šablona vytvoří novou aplikaci ASP.NET MVC Web s požadované složky, šablony položek a položek konfigurace souboru.

V této úloze se zaměřuje na rozumět elementům sady, které se podílejí struktury řešení a jejich vztahy. Následující složky jsou součástí všechny aplikace ASP.NET MVC, protože používá rozhraní ASP.NET MVC ve výchozím nastavení &quot;konvence nad konfigurací&quot; přístup a provede některé předpoklady výchozí podle pojmenování složky konvence.

1. Jakmile se vytvoří projekt, zkontrolujte strukturu složek, který byl vytvořen v Průzkumníku řešení na pravé straně:

    ![Struktura složek ASP.NET MVC v Průzkumníku řešení](aspnet-mvc-4-fundamentals/_static/image4.png "strukturu složek ASP.NET MVC v Průzkumníku řešení")

    *Struktura složek ASP.NET MVC v Průzkumníku řešení*

   1. **Kontrolery**. Tato složka bude obsahovat třídy kontroleru. V aplikaci MVC na základě řadiče jsou zodpovědná za zpracování interakce s koncovým uživatelem, manipulace s modelem a nakonec výběrem zobrazení k vykreslení uživatelské rozhraní.

       > [!NOTE]
       > Rozhraní MVC požaduje názvy všech řadičů bude končit &quot;řadič&quot;– například HomeController LoginController či ProductController.
   2. **Modely**. Tato složka se poskytuje pro třídy, které představují aplikačního modelu pro MVC webovou aplikaci. To obvykle zahrnuje kód, který definuje objekty a logiku pro interakci s úložišti. Obvykle budou pro objekty skutečné model v samostatné třídy knihovny. Ale když vytvoříte novou aplikaci, můžete zahrnout třídy a přesuňte je do samostatné třídy knihovny později v cyklu vývoje.
   3. **Zobrazení**. Tato složka je doporučené umístění pro zobrazení, součásti za zobrazení uživatelského rozhraní aplikace. Zobrazení pomocí souborů .aspx, .ascx, .master a cshtml, kromě jiných souborů, které se vztahují k vykreslení zobrazení. Zobrazení složky obsahuje složku pro každý kontroler; složka je název s předponou názvu kontroleru. Například, pokud máte řadič s názvem **HomeController**, složka zobrazení bude obsahovat složku s názvem Domů. Ve výchozím nastavení, pokud rozhraní ASP.NET MVC načte zobrazení, hledá soubor .aspx s názvem požadované zobrazení ve složce Views\controllerName (**zobrazení [parametr ControllerName] [Action] .aspx**) nebo (**zobrazení [parametr ControllerName] [Akce] .cshtml**) pro zobrazení syntaxe Razor.

      > [!NOTE]
      > Kromě složky uvedených výše, aplikace MVC Web používá **Global.asax** výchozí soubor nastavení globální směrování adres URL a používá **Web.config** souboru konfigurace aplikace.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Úloha 3 – Přidání HomeController

V aplikacích ASP.NET, které nepoužívají rozhraní MVC je interakci s uživatelem uspořádaná kolem stránky a vyvolávání a zpracování událostí z těchto stránek. Interakce uživatele s aplikací ASP.NET MVC se naopak věnuje kontrolerů a jejich metod akcí.

Architektura ASP.NET MVC na druhé straně mapuje adresy URL na třídy, které se označují jako řadiče. Řadiče zpracovat příchozí požadavky, zpracování vstupu uživatele a interakce, provést příslušné aplikace logiky a určit odpověď k odeslání zpět do klienta (zobrazení HTML, stáhněte si soubor, přesměrovat na jinou adresu URL, atd.). V případě zobrazení HTML, volá třídu kontroleru obvykle součástí samostatné zobrazení generovat kód HTML pro daný požadavek. V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce.

V této úloze přidejte třídu Kontroleru, který bude zpracovávat adresy URL na domovské stránce webu Music Store.

1. Klikněte pravým tlačítkem na **řadiče** složku v Průzkumníku řešení vyberte **přidat** a potom **řadič** příkaz:

    ![Přidání Kontroleru příkazu](aspnet-mvc-4-fundamentals/_static/image5.png "přidat příkaz Kontroleru")

    *Přidat kontroler – příkaz*
2. **Přidat kontroler** se zobrazí dialogové okno. Název kontroleru *HomeController* a stiskněte klávesu **přidat**.

    ![Přidat kontroler Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "přidat kontroler Dialog")

    *Přidat Dialog Kontroleru*
3. Soubor **HomeController.cs** se vytvoří v **řadiče** složky. Abyste měli **HomeController** vrátit řetězec na jeho akce indexu, nahraďte **Index** metodu s následujícím kódem:

    (Fragment - kódu *základy ASP.NET MVC 4 – Ex1 HomeController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Úloha 4 – spouštění aplikace

V této úloze bude vyzkoušet aplikace ve webovém prohlížeči.

1. Stisknutím klávesy **F5** ke spuštění aplikace. Projekt je zkompilován a spustí místní webový Server služby IIS. Místní webový Server služby IIS se automaticky otevře webový prohlížeč a přejděte na adresu URL webového serveru.

    ![Aplikace běžící ve webovém prohlížeči](aspnet-mvc-4-fundamentals/_static/image7.png "aplikaci běžící ve webovém prohlížeči")

    *Aplikace běžící ve webovém prohlížeči*

    > [!NOTE]
    > Na místním webovém serveru IIS se spustí na webu na několika náhodný volný port. Na obrázku výše je web spuštěný v `http://localhost:50103/`, takže se používá port 50103. Vaše číslo portu se může lišit.
2. Zavřete prohlížeč.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Cvičení 2: Vytvoření Kontroleru

V tomto cvičení se dozvíte, jak aktualizovat kontroler k implementaci funkcí jednoduchého aplikace Music Store. Tento kontroler budou definovat metody akce pro zpracování každé z následujících konkrétní požadavky:

- Stránce žánrů Hudba v Music Store
- Procházet stránku, která obsahuje seznam všech hudebních alb pro konkrétní žánr
- Stránka s podrobnostmi s informacemi o alba konkrétní Hudba

Pro obor v tomto cvičení tyto akce jednoduše vrátí řetězec nyní.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Úloha 1 – přidání nové třídy StoreController

V této úloze přidáte nový kontroler.

1. Pokud ještě není otevřený, začněte **VS Express for Web 2012**.
2. V **souboru** nabídce zvolte **otevřít projekt**. V dialogovém okně Otevřít projekt, přejděte do **Source\Ex02 CreatingAController\Begin**vyberte **Begin.sln** a klikněte na tlačítko **otevřít**. Alternativně můžete pokračovat s řešením, který jste získali po dokončení předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Přidáte nový kontroler. Chcete-li to provést, klikněte pravým tlačítkem **řadiče** složku v Průzkumníku řešení vyberte **přidat** a pak **řadič** příkazu. Změnit **názvu Kontroleru** k *StoreController*a klikněte na tlačítko **přidat**.

    ![Přidat kontroler Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "přidat kontroler Dialog")

    *Přidat Dialog Kontroleru*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Úloha 2 - Úprava StoreController akce

V této úloze budete upravovat metodách Kontroleru, které jsou volány **akce**. Akce jsou zodpovědná za zpracování žádostí adresy URL a určení obsahu, který by měl být odesílaných zpět do prohlížeče nebo uživatel, který vyvolal adresu URL.

1. **StoreController** třída již má **Index** metody. Použijete ji později v tomto testovacím prostředí k implementaci stránky, která obsahuje seznam všech žánry music store. Teď stačí nahradit **Index** metodu s následujícím kódem, který vrací řetězec &quot;Dobrý den ze Store.Index()&quot;:

    (Fragment - kódu *základy ASP.NET MVC 4 – Ex2 StoreController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Přidat **Procházet** a **podrobnosti** metody. Chcete-li to provést, přidejte následující kód k **StoreController**:

    (Fragment - kódu *základy ASP.NET MVC 4 – Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Úloha 3 – spuštění aplikace

V této úloze bude vyzkoušet aplikace ve webovém prohlížeči.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí v **Domů** stránky. Změňte adresu URL na ověřit implementaci každou akci.

    1. **/ Store**. Zobrazí se  **&quot;Dobrý den ze Store.Index()&quot;**.
    2. **/ Store/procházení**. Zobrazí se  **&quot;Dobrý den ze Store.Browse()&quot;**.
    3. **/ Store/podrobnosti**. Zobrazí se  **&quot;Dobrý den ze Store.Details()&quot;**.

        ![Procházení StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "procházení StoreBrowse")

        *Procházení /Store/Browse*
3. Zavřete prohlížeč.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Cvičení 3: Předání parametrů do Kontroleru

Až doteď mají se vrací konstantní řetězce z řadičů. V tomto cvičení se dozvíte, jak předat parametry Kontroleru pomocí adresy URL a řetězec dotazu a potom provádění metody akce, které reakce do prohlížeče s textem.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Úloha 1 – Přidání parametru žánr StoreController

V této úloze budete používat **querystring** parametry se mají odeslat **Procházet** metodu akce v **StoreController**.

1. Pokud ještě není otevřený, začněte **VS Express for Web**.
2. V **souboru** nabídce zvolte **otevřít projekt**. V dialogovém okně Otevřít projekt, přejděte do **Source\Ex03 PassingParametersToAController\Begin**vyberte **Begin.sln** a klikněte na tlačítko **otevřít**. Alternativně můžete pokračovat s řešením, který jste získali po dokončení předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Otevřít **StoreController** třídy. Chcete-li to provést, v **Průzkumníka řešení**, rozbalte **řadiče** složky a dvojím kliknutím **StoreController.cs**.
4. Změnit **Procházet** metoda přidáním parametru řetězce vyžádat pro konkrétní žánr. ASP.NET MVC automaticky předávat všechny řetězce dotazu nebo pojmenované parametry formuláře **žánr** k této metodě akce při vyvolání. Chcete-li to provést, nahraďte **Procházet** metodu s následujícím kódem:

    (Fragment - kódu *základy ASP.NET MVC 4 – EX3. StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Používáte **HttpUtility.HtmlEncode** nástroj metody zabrání uživatelům v vkládá jazyka Javascript do zobrazení s odkazem jako   **/Store/Procházet? Rozšířením podle tematických =&lt;skript&gt;window.location= "[http://hackersite.com](http://hackersite.com)"&lt;/script&gt;**.
> 
> Další vysvětlení, navštivte prosím [článku na webu msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Úloha 2 – spuštění aplikace

V této úloze, vyzkoušejte si aplikace ve webovém prohlížeči a použít **žánr** parametru.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí v **Domů** stránky. Změňte adresu URL na   */Store/Procházet? Rozšířením podle tematických = Roz* k ověření, že akce přijímá parametr žánr.

    ![Procházení StoreBrowseGenre = Roz](aspnet-mvc-4-fundamentals/_static/image10.png "procházení StoreBrowseGenre = Roz")

    *Procházení /Store/Browse? Rozšířením podle tematických = Roz*
3. Zavřete prohlížeč.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Úloha 3 – Přidání parametr Id vložené v adrese URL

V této úloze budete používat **URL** předat **Id** parametr **podrobnosti** metody akce **StoreController**. ASP.NET MVC výchozí konvenci směrování se zachází segment adresy URL po názvu metody akce, jako parametr s názvem **Id**. Pokud vaše metoda akce má parametr s názvem Id, ASP.NET MVC se automaticky předat segment adresy URL pro vás jako parametr. V adrese URL **Store/podrobnosti/5**, **Id** bude vyhodnocen jako **5**.

1. Změnit **podrobnosti** metodu **StoreController**, přidání **int** parametr s názvem **id**. Chcete-li to provést, nahraďte **podrobnosti** metodu s následujícím kódem:

    (Fragment - kódu *základy ASP.NET MVC 4 – EX3. StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Úloha 4 – spouštění aplikace

V této úloze, vyzkoušejte si aplikace ve webovém prohlížeči a použít **Id** parametru.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí v **Domů** stránky. Změňte adresu URL na */Store/Details/5* k ověření, že akce přijímá parametr id.

    ![Procházení StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "procházení StoreDetails5")

    *Procházení /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Cvičení 4: Vytvoření zobrazení

Mají se zatím vracení řetězců z akce kontroleru. I když, který je užitečný způsob, jak Princip fungování řadiče, je postup nejsou sestavení webové aplikace skutečný. Zobrazení jsou komponenty, které poskytují lepší přístup ke generování HTML zpět do prohlížeče s použitím souborů šablon.

V tomto cvičení se dozvíte, jak přidat rozložení stránky předlohy k nastavení šablony pro běžné obsah HTML, šablony stylů k vylepšení vzhledu a chování webu a nakonec zobrazení šablony pro povolení HomeController vrátit ve formátu HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Úloha 1 - úprava souboru \_layout.cshtml

Soubor **~/Views/Shared/\_layout.cshtml** umožní vám nastavit šablonu pro společný kód HTML pro použití na různých celého webu. V této úloze přidáte rozložení stránky předlohy s společné hlavičky s odkazy na oblast domovské stránky a Store.

1. Pokud ještě není otevřený, začněte **VS Express for Web**.
2. V **souboru** nabídce zvolte **otevřít projekt**. V dialogovém okně Otevřít projekt, přejděte do **Source\Ex04 CreatingAView\Begin**vyberte **Begin.sln** a klikněte na tlačítko **otevřít**. Alternativně můžete pokračovat s řešením, který jste získali po dokončení předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Soubor  <strong>\_layout.cshtml</strong> obsahuje rozložení HTML kontejner pro všechny stránky na webu. Zahrnuje <strong>&lt;html&gt;</strong> – element pro odpovědi HTML, stejně jako <strong>&lt;head&gt;</strong> a <strong>&lt;tělo&gt;</strong> elementy. <strong>@RenderBody()</strong> v kódu HTML tělo identifikovat oblasti zobrazení šablony budete moct vyplní dynamický obsah.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Přidáte společné hlavičky s odkazy na domovskou stránku a Store oblast na všech stránkách v lokalitě. Aby bylo možné provést, přidejte následující kód níže &lt;tělo&gt; příkazu.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Zahrnout div k vykreslení textu části každé stránky. Nahraďte  <strong>@RenderBody()</strong> následujícím kódem higlighted: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Věděli jste? Visual Studio 2012 obsahuje fragmenty kódu, které usnadňují přidejte kód pro běžně používané v HTML, soubory kódu a další. Vyzkoušejte si zadáním **&lt;div&gt;** a stisknutím klávesy **kartu** dvakrát pro vložení kompletní **div** značky.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Úloha 2 – Přidání šablony stylů CSS

Šablonu prázdného projektu obsahuje velmi zjednodušený soubor šablony stylů CSS, který obsahuje pouze styly slouží k zobrazení založeného na základních formulářích a ověřovacích zpráv. Aby bylo možné vylepšit vzhled a chování webu bude používat další šablony stylů CSS a Image (potenciálně poskytované "designer").

V této úloze přidá šablona stylů CSS pro definování styly lokality.

1. Soubor šablony stylů CSS a image jsou součástí **Source\Assets\Content** složka tohoto testovacího prostředí. Aby bylo možné je přidat do aplikace, přetáhněte jejich obsah z **Windows Explorer** okno do **Průzkumníka řešení** v sadě Visual Studio Express for Web, jak je znázorněno níže:

    ![Přetažení obsah stylu](aspnet-mvc-4-fundamentals/_static/image12.png "přetažením obsah stylu")

    *Přetažení obsah stylu*
2. Dialogové okno upozornění se zobrazí, žádostí o potvrzení k nahrazení **Site.css** souboru a některé existující Image. Zkontrolujte **použít u všech položek** a klikněte na tlačítko **Ano**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Úloha 3 – Přidání zobrazit šablonu

V této úloze přidá šablonu zobrazení k vygenerování odpovědi HTML, který bude používat rozložení stránky předlohy a šablon stylů CSS přidaný v tomto cvičení.

1. Použití zobrazení šablony při procházení na domovskou stránku, musíte nejdřív místo vrácení řetězce, která označuje, že **HomeController Index** metoda vrátí **zobrazení**. Otevřít **HomeController** třídy a změňte jeho **Index** metodu pro návrat **ActionResult**, a vrátí **View()**.

    (Fragment - kódu *základy ASP.NET MVC 4 – Ex4 HomeController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Teď budete muset přidat odpovídající šablonu zobrazení. K tomu **klikněte pravým tlačítkem na** uvnitř **Index** metody akce a vyberte **přidat zobrazení**. Tím se otevře **přidat zobrazení** dialogového okna.

    ![Přidání zobrazení z v rámci metody Index](aspnet-mvc-4-fundamentals/_static/image13.png "přidání zobrazení z v rámci Index – metoda")

    *Přidání zobrazení z v rámci Index – metoda*
3. **Přidat zobrazení** zobrazí se dialogové okno pro vytvoření souboru šablony zobrazení. Ve výchozím nastavení toto dialogové okno zadá název zobrazení šablony tak, aby odpovídalo metodě akce, která bude používat. Protože jste použili **přidat zobrazení** místní nabídky v rámci **Index** metody akce v rámci HomeController, **přidat zobrazení** dialogové okno obsahuje Index jako výchozí název zobrazení. Klikněte na tlačítko **přidat**.

    ![Přidat Dialog zobrazení](aspnet-mvc-4-fundamentals/_static/image14.png "Přidat Dialog zobrazení")

    *Přidat Dialog zobrazení*
4. Visual Studio generuje **Index.cshtml** zobrazit šablonu uvnitř **Views\Home** složku a pak ho otevře.

    ![Domácí Index zobrazení vytvořené](aspnet-mvc-4-fundamentals/_static/image15.png "Domů Index zobrazení vytvořené")

    *Vytvoření domovské zobrazení indexu*

    > [!NOTE]
    > název a umístění **Index.cshtml** soubor je relevantní a dodržuje zásady vytváření názvů výchozí rozhraní ASP.NET MVC.
    > 
    > Složka \Views\**Domů** odpovídá názvu kontroleru (**Domů** Kontroleru). Název zobrazení šablony (**Index**), odpovídá metodu akce kontroleru, který se zobrazuje zobrazení.
    > 
    > Tímto způsobem, ASP.NET MVC díky tomu není nutné explicitně zadat název nebo umístění šablony zobrazení při použití tyto zásady vytváření názvů pro vrácení zobrazení.
5. Generované zobrazení šablona je založena na  **\_layout.cshtml** dříve definované šablony. Umožňuje aktualizovat vlastnost ViewBag.Title k **Domů**a změnit hlavní obsah na **Toto je domovská stránka**, jak je znázorněno v následujícím kódu:

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Vyberte **MvcMusicStore** projekt v Průzkumníku řešení a stiskněte klávesu **F5** ke spuštění aplikace.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Úloha 4: ověření

Pokud chcete ověřit, že jste správně provedli všechny kroky v předchozím cvičení, postupujte následovně:

V aplikaci otevřít v prohlížeči by měl Všimněte si, že:

1. Metoda akce indexu HomeController nalezen a zobrazí **\Views\Home\Index.cshtml** zobrazit šablonu, i když kód volá **vrátit View()**, protože zobrazit šablonu a potom standardní zásady vytváření názvů.
2. Na domovské stránce se zobrazí zobrazení uvítací zprávy definovaných v rámci **\Views\Home\Index.cshtml** zobrazit šablonu.
3. Domovská stránka používá  **\_layout.cshtml** šablony, a proto se uvítací zpráva je obsažena v rozložení standardní webu ve formátu HTML.

    ![Domácí zobrazení indexu pomocí definovaných LayoutPage a styl](aspnet-mvc-4-fundamentals/_static/image16.png "Domů zobrazení indexu pomocí definovaných LayoutPage a stylu")

    *Domovské zobrazení indexu pomocí definovaných LayoutPage a stylu*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Cvičení 5: Vytvoření modelu zobrazení

Zatím jste provedli v zobrazeních Zobrazit pevně zakódované HTML, ale chcete-li vytvořit dynamické webové aplikace, zobrazit šablonu získat informace z Kontroleru. Jednou z běžných metod má být použit pro tento účel je **ViewModel** vzor, který umožňuje Kontroleru Zabalit všechny informace potřebné ke generování odpovídající odpověď ve formátu HTML.

V tomto cvičení se dozvíte, jak vytvořit třídu ViewModel a přidejte požadované vlastnosti: počet žánry v úložišti a seznam těchto žánrů. Budete také aktualizovat StoreController používat vytvořený ViewModel a nakonec vytvoříte novou šablonu zobrazení, které se zobrazí vlastnosti uvedených na stránce.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Úloha 1 – vytváření tříd ViewModel

V této úloze vytvoříte třídu ViewModel, který implementuje scénář výpis žánr Store.

1. Pokud ještě není otevřený, začněte **VS Express for Web**.
2. V **souboru** nabídce zvolte **otevřít projekt**. V dialogovém okně Otevřít projekt, přejděte do **Source\Ex05 CreatingAViewModel\Begin**vyberte **Begin.sln** a klikněte na tlačítko **otevřít**. Alternativně můžete pokračovat s řešením, který jste získali po dokončení předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Vytvoření **modely ViewModels** složku pro uložení ViewModel. Chcete-li to provést, klikněte pravým tlačítkem myši na nejvyšší úrovni **MvcMusicStore** projekt, vyberte **přidat** a potom **novou složku**.

    ![Přidat novou složku](aspnet-mvc-4-fundamentals/_static/image17.png "přidat novou složku")

    *Přidání nové složky*
4. Název složky *modely ViewModels*.

    ![Modely ViewModels složku v Průzkumníku řešení](aspnet-mvc-4-fundamentals/_static/image18.png "modely ViewModels složku v Průzkumníku řešení")

    *Modely ViewModels složku v Průzkumníku řešení*
5. Vytvoření **ViewModel** třídy. Chcete-li to provést, klikněte pravým tlačítkem na **modely ViewModels** složky, naposledy vytvořené, vyberte **přidat** a potom **nová položka**. V části **kód**, zvolte **třídy** položku a zadejte název souboru *StoreIndexViewModel.cs*, pak klikněte na tlačítko **přidat**.

    ![Přidání nové třídy](aspnet-mvc-4-fundamentals/_static/image19.png "přidání nové třídy")

    *Přidání nové třídy*

    ![Vytvoření třídy StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel vytváření tříd")

    *Vytvoření třídy StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Úloha 2 – Přidání vlastností do třídy ViewModel

Existují dva parametry se mají předat z StoreController zobrazit šablonu k vytvoření odpovědi HTML očekávané: počet žánry v úložišti a seznam těchto žánrů.

V této úloze budete přidávat tyto 2 vlastností **StoreIndexViewModel** třídy: **NumberOfGenres** (celé číslo) a **žánry** (seznam řetězců).

1. Přidat **NumberOfGenres** a **žánry** vlastnosti, které chcete **StoreIndexViewModel** třídy. Chcete-li to provést, přidejte následující řádky 2 do definice třídy:

    (Fragment - kódu *ASP.NET MVC 4 základy - Ex5 StoreIndexViewModel vlastnosti*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> **{Get; nastavit;}**  notation využívá jazyka C# pro funkce automaticky implementované vlastnosti. Poskytuje výhody vlastnost bez nutnosti nám chcete-li deklarovat pole zálohování.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Úloha 3 - aktualizace StoreController používat StoreIndexViewModel

**StoreIndexViewModel** třída zapouzdří informace potřebné pro předání z **StoreController**společnosti **Index** metodu zobrazit šablonu, aby bylo možné generovat odpověď .

V této úloze budete aktualizovat **StoreController** používat **StoreIndexViewModel**.

1. Otevřít **StoreController** třídy.

    ![Otevírání StoreController třídy](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController počáteční třída")

    *Otevírání StoreController třídy*
2. Chcete-li použít **StoreIndexViewModel** třídy z **StoreController**, přidat následující obor názvů v horní části **StoreController** kódu:

    (Fragment - kódu *ASP.NET MVC 4 základy - Ex5 StoreIndexViewModel pomocí modely ViewModels*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Změnit **StoreController**společnosti **Index** metody akce, takže se vytvoří a naplní **StoreIndexViewModel** objektu a pak předá zobrazení šablony Generovat odpověď jazyka HTML s ním.

    > [!NOTE]
    > V testovacím prostředí &quot;ASP.NET MVC modely a přístup k datům&quot; budete psát kód, který načte seznam objektů úložiště žánry z databáze. V následujícím kódu, kterou vytvoříte **seznamu** žánrů fiktivní data, které se vyplní **StoreIndexViewModel**.
    > 
    > Po vytvoření a nastavení **StoreIndexViewModel** objektu, bude předáno jako argument **zobrazení** metody. To znamená, že zobrazení šablony použije k vygenerování odpověď jazyka HTML s ním tohoto objektu.
4. Nahradit **Index** metodu s následujícím kódem:

    (Fragment - kódu *ASP.NET MVC 4 základy - metoda Ex5 StoreController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Pokud nejste obeznámeni s jazykem C#, mohou předpokládat, který používá **var** znamená, že **viewModel** proměnná je s pozdní vazbou. Který není správný – kompilátor jazyka C# používá podle přiřadit k proměnné odvození typu k určení, která **viewModel** je typu **StoreIndexViewModel**. Také, kompilací místní **viewModel** proměnnou **StoreIndexViewModel** typ kontroly kompilace get a podpora editoru kódu sady Visual Studio.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Úloha 4 – vytvoření zobrazit šablonu, která používá StoreIndexViewModel

V této úloze vytvoříte zobrazení šablony, která se použije k zobrazení seznamu žánrů StoreIndexViewModel objekt předaný z Kontroleru.

1. Před vytvořením nové šablony zobrazení, můžeme sestavit projekt tak, aby **Přidat Dialog zobrazení** ví o **StoreIndexViewModel** třídy. Sestavte projekt výběrem **sestavení** položky nabídky a pak **sestavení MvcMusicStore**.

    ![Sestavení projektu](aspnet-mvc-4-fundamentals/_static/image22.png "sestavení projektu")

    *Sestavení projektu*
2. Vytvořte novou šablonu zobrazení. K tomu, klepněte pravým tlačítkem myši **Index** metody a vyberte **přidat zobrazení**.

    ![Přidání zobrazení](aspnet-mvc-4-fundamentals/_static/image23.png "přidání zobrazení")

    *Přidání zobrazení*
3. Protože **Přidat Dialog zobrazení** byla vyvolána z **StoreController**, přidá ve výchozím nastavení v šabloně zobrazení **\Views\Store\Index.cshtml** souboru. Zkontrolujte **vytvoření silně zadali – zobrazení** zaškrtávací políčko a potom vyberte **StoreIndexViewModel** jako **třída modelu**. Také se ujistěte, že je modul zobrazení vybrané **Razor**. Klikněte na tlačítko **přidat**.

    ![Přidat Dialog zobrazení](aspnet-mvc-4-fundamentals/_static/image24.png "Přidat Dialog zobrazení")

    *Přidat Dialog zobrazení*

    **\Views\Store\Index.cshtml** je vytvořen a otevřít soubor šablony zobrazení. Podle informací uvedených na **přidat zobrazení** dialogového okna v posledním kroku, zobrazení bude očekávat šablony **StoreIndexViewModel** instance jako data pro generování odpověď jazyka HTML. Všimnete si, že šablona dědí `ViewPage<musicstore.viewmodels.storeindexviewmodel>` v jazyce C#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Úloha 5: aktualizuje se šablona zobrazení

V této úloze budete aktualizovat zobrazení šablony vytvořili v minulé úloze se načíst počet žánry a jejich názvy v rámci stránky.

> [!NOTE]
> Budete používat @ syntaxe (označovaný také jako &quot;kódu útržky&quot;) ke spouštění kódu v rámci zobrazení šablony.

1. V **Index.cshtml** souborů v rámci **Store** složky, nahraďte jeho kód následujícím kódem:

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Smyčka v seznamu žánr v **StoreIndexViewModel** a vytvoření HTML **&lt;ul&gt;** seznamu pomocí **foreach** smyčky.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Stisknutím klávesy **F5** ke spuštění aplikace a Procházet **/Store**. Zobrazí se seznam žánry předaný **StoreIndexViewModel** objektu z **StoreController** k šabloně zobrazení.

    ![Zobrazení seznamu žánry](aspnet-mvc-4-fundamentals/_static/image26.png "zobrazení seznamu žánry")

    *Zobrazení seznamu žánry*
4. Zavřete prohlížeč.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Cvičení 6: Použití parametrů v zobrazení

Cvičení 3 jste zjistili, jak pro předání parametrů do Kontroleru. V tomto cvičení se dozvíte, jak používat tyto parametry v šabloně zobrazení. Pro tento účel vám představíme nejprve do tříd modelu, které vám pomohou spravovat data a domény logiku. Kromě toho se dozvíte, jak vytvořit odkazy na stránky v aplikaci ASP.NET MVC bez obav z věci, jako je kódování cesty adresy URL.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Úloha 1 – přidání třídy modelu

Na rozdíl od modely ViewModel, který se vytvoří pouze k předávání informací z Kontroleru zobrazení, jsou integrované tříd modelu obsahovat a spravovat data a domény logiku. V této úloze budete přidávat dvou tříd modelu pro reprezentaci těchto konceptů: **žánr** a **alba**.

1. Pokud ještě není otevřený, začněte **VS Express for Web**
2. V **souboru** nabídce zvolte **otevřít projekt**. V dialogovém okně Otevřít projekt, přejděte do **Source\Ex06 UsingParametersInView\Begin**vyberte **Begin.sln** a klikněte na tlačítko **otevřít**. Alternativně můžete pokračovat s řešením, který jste získali po dokončení předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Přidat **žánr** třída modelu. Chcete-li to provést, klikněte pravým tlačítkem **modely** složky v **Průzkumníku řešení**vyberte **přidat** a pak **nová položka** možnost. V části **kód**, zvolte **třídy** položku a zadejte název souboru *Genre.cs*, pak klikněte na tlačítko **přidat**.

    ![Přidání třídy](aspnet-mvc-4-fundamentals/_static/image27.png "přidání třídy")

    *Přidání nové položky*

    ![Přidejte třídu modelu s rozšířením podle tematických](aspnet-mvc-4-fundamentals/_static/image28.png "přidejte třídu modelu s rozšířením podle tematických")

    *Přidejte třídu modelu s rozšířením podle tematických*
4. Přidat **název** vlastností do třídy rozšířením podle tematických. Chcete-li to provést, přidejte následující kód:

    (Fragment - kódu *základy ASP.NET MVC 4 – Ex6 žánr*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Stejný postup jako předtím, přidejte **alba** třídy. Chcete-li to provést, klikněte pravým tlačítkem **modely** složky v **Průzkumníku řešení**vyberte **přidat** a pak **nová položka** možnost. V části **kód**, zvolte **třídy** položku a zadejte název souboru *Album.cs*, pak klikněte na tlačítko **přidat**.
6. Přidejte dvě vlastnosti do třídy alba: **žánr** a **Title**. Chcete-li to provést, přidejte následující kód:

    (Fragment - kódu *základy ASP.NET MVC 4 – Ex6 alba*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Úloha 2 – přidání StoreBrowseViewModel

A **StoreBrowseViewModel** se použije k zobrazení alb, která odpovídá vybrané žánr při plnění tohoto úkolu. V této úloze vytvoříte této třídy a pak přidejte dvě vlastnosti pro zpracování **žánr** a jeho **alba**v seznamu.

1. Přidat **StoreBrowseViewModel** třídy. Chcete-li to provést, klikněte pravým tlačítkem **modely ViewModels** složky v **Průzkumníku řešení**vyberte **přidat** a pak **nová položka** možnost. V části **kód**, zvolte **třídy** položku a zadejte název souboru *StoreBrowseViewModel.cs*, pak klikněte na tlačítko **přidat**.
2. Přidejte odkaz na modely v **StoreBrowseViewModel** třídy. Chcete-li to provést, přidejte následující s použitím oboru názvů:

    (Fragment - kódu *základy ASP.NET MVC 4 – Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Přidejte dvě vlastnosti do **StoreBrowseViewModel** třídy: **žánr** a **alb**. Chcete-li to provést, přidejte následující kód:

    (Fragment - kódu *základy ASP.NET MVC 4 – Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> Co je **seznamu&lt;alba&gt;**  ?: Tato definice používá **seznamu&lt;T&gt;**  typ, ve kterém **T** omezuje typ, na čem to **seznamu** patří, v tomto případě **alba** (nebo libovolného z jeho potomků).
> 
> Volá se tato schopnost navrhování tříd a metod, které specifikace jeden nebo více typů odložit, dokud třídy nebo metody je deklarovaný a vytvořena kódem na straně klienta je funkce jazyka C# **obecných typů**.
> 
> **Seznam&lt;T&gt;**  je obecný ekvivalent **ArrayList** typ a je k dispozici **System.Collections.Generic** oboru názvů. Jednou z výhod použití **obecných typů** je, že vzhledem k tomu, že typ je určen, není potřeba starat o kontrolu operace, jako jsou prvky převedete do každodenního přetypování typu **alba** jako by tomu u **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Úloha 3 – používání nového ViewModel v StoreController

V této úloze budete upravovat **StoreController**společnosti **Procházet** a **podrobnosti** metody akce k používání nového **StoreBrowseViewModel** .

1. Přidejte odkaz na složku modely v **StoreController** třídy. Chcete-li to provést, rozbalte **řadiče** složky **Průzkumníku řešení** a otevřete **StoreController** třídy. Přidejte následující kód:

    (Fragment - kódu *základy ASP.NET MVC 4 – Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Nahradit **Procházet** metoda akce se má použít **StoreViewBrowseController** třídy. S fiktivními daty se vytvoří rozšířením podle tematických a dva nové objekty alb (v dalším praktického testovacího prostředí budete využívat reálná data z databáze). Chcete-li to provést, nahraďte **Procházet** metodu s následujícím kódem:

    (Fragment - kódu *základy ASP.NET MVC 4 – Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Nahradit **podrobnosti** metoda akce se má použít **StoreViewBrowseController** třídy. Vytvoří nový **alba** objekt, který se má vrátit **zobrazení**. Chcete-li to provést, nahraďte **podrobnosti** metodu s následujícím kódem:

    (Fragment - kódu *základy ASP.NET MVC 4 – Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Úloha 4 – přidání procházet zobrazit šablonu

V této úloze, které přidáte **Procházet** zobrazení alb pro konkrétní žánr se nenašly.

1. Před vytvořením nové šablony zobrazení se má sestavit projekt tak, aby **přidat zobrazení** dialogové okno ví o **ViewModel** třídu použít. Sestavte projekt výběrem **sestavení** položky nabídky a pak **sestavení MvcMusicStore**.
2. Přidat **Procházet** zobrazení. Chcete-li to provést, klikněte pravým tlačítkem **Procházet** metody akce **StoreController** a klikněte na tlačítko **přidat zobrazení**.
3. V **přidat zobrazení** dialogového okna zkontrolujte, zda je název zobrazení, **Procházet**. Zkontrolujte **vytvoření zobrazení se silnými typy** zaškrtávací políčko a vyberte **StoreBrowseViewModel** z **třída modelu** rozevíracího seznamu. V ostatních polích ponechte jejich výchozí hodnotu. Pak klikněte na tlačítko **přidat**.

    ![Přidání zobrazení Procházet](aspnet-mvc-4-fundamentals/_static/image29.png "přidání Procházet zobrazení")

    *Přidání zobrazení procházení*
4. Upravit **Browse.cshtml** k zobrazení informací Genre, přístup k **StoreBrowseViewModel** objektu, který je předán do zobrazení šablony. Chcete-li to provést, nahraďte obsah následujícím kódem: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Úloha 5: spuštění aplikace

V této úloze budete testovat, který **Procházet** metoda načte alb z **Procházet** metody akce.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na   **/Store/Procházet? Rozšířením podle tematických = Roz** k ověření, že akce vrací dva alb.

    ![Procházení Disco alb Store](aspnet-mvc-4-fundamentals/_static/image30.png "procházení Disco alb Store")

    *Procházení Disco alb Store*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Krok 6 – zobrazení informací o určité Album

V této úloze budete implementovat **Store/podrobnosti** zobrazení pro zobrazení informací o určité album. V tomto testovacím prostředí praktického všechno, co se zobrazí informace o alba je již součástí **zobrazení** šablony. Ano, místo vytvoření **StoreDetailsViewModel** třídy, budete používat aktuální **StoreBrowseViewModel** předáním alba šablony.

1. Zavřete prohlížeč v případě potřeby se vraťte do okna nástroje Visual Studio. Přidat nový **podrobnosti** zobrazit **StoreController**společnosti **podrobnosti** metody akce. Chcete-li to provést, klikněte pravým tlačítkem **podrobnosti** metoda ve **StoreController** třídy a klikněte na tlačítko **přidat zobrazení**.
2. V **přidat zobrazení** dialogového okna, ověřte, že **název zobrazení** je **podrobnosti**. Zkontrolujte **vytvoření zobrazení se silnými typy** zaškrtávací políčko a vyberte **alba** z **třída modelu** rozevíracího seznamu. V ostatních polích ponechte jejich výchozí hodnotu. Pak klikněte na tlačítko **přidat**. To vytvoří a otevře **\Views\Store\Details.cshtml** souboru.

    ![Přidání zobrazení podrobností o](aspnet-mvc-4-fundamentals/_static/image31.png "přidávání zobrazení podrobností")

    *Přidání zobrazení podrobností*
3. Upravit **Details.cshtml** soubor k zobrazení informací na Album přístup k **alba** objektu, který je předán do zobrazení šablony. Chcete-li to provést, nahraďte obsah následujícím kódem: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Úloha 7: spuštění aplikace

V této úloze budete testovat, který **podrobnosti** zobrazení načte informace o alba z **podrobnosti akce** metody.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí v **Domů** stránky. Změňte adresu URL na **/Store/Details/5** potvrzení alba informací.

    ![Procházení podrobností alb](aspnet-mvc-4-fundamentals/_static/image32.png "procházení podrobností alb")

    *Procházení podrobností alba*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Úloha 8 – přidání propojení mezi stránkami

V této úloze budete přidávat prostřednictvím odkazu ve Store zobrazení má odkaz v názvu každý žánr na příslušné **/Store/Procházet** adresy URL. Tímto způsobem, po kliknutí na Genre, například **Roz**, přejdete na **/Store/procházet? žánr = Roz** adresy URL.

1. Zavřete prohlížeč v případě potřeby se vraťte do okna nástroje Visual Studio. Aktualizace **Index** stránky a přidat odkaz **Procházet** stránky. Chcete-li to provést, v **Průzkumníka řešení** rozbalte **zobrazení** složku, pak bude **Store** složky a dvojím kliknutím **Index.cshtml** stránky.
2. Přidáte odkaz na zobrazení procházet označující žánr vybrali. Chcete-li to provést, nahradit následující zvýrazněný kód v rámci **&lt;li&gt;** značky: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > Další možností by propojení přímo na stránku s kódem, jako je následující:
   > 
   > &lt;href =&quot;/Store/procházet? žánr =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Přestože tento přístup funguje, závisí na pevně zakódované řetězce. Pokud později přejmenovat Kontroleru, budete muset změnit tento pokyn ručně. Lepší alternativou je použití **pomocné rutiny HTML** metody. ASP.NET MVC zahrnuje metodu pomocné rutiny HTML, který je k dispozici u takových úloh. **Html.ActionLink()** Pomocná metoda usnadňuje vytváření HTML **&lt;&gt;** odkazy, ujistěte se cesty URL jsou správně kódování URL.
   > 
   > Htlm.ActionLink má několik přetížení. V tomto cvičení budete používat ten, který přijímá tři parametry:
   > 
   > 1. Text odkazu, který se zobrazí název žánru
   > 2. Název akce kontroleru (**Procházet**)
   > 3. Hodnoty parametrů, zadáte název trasy (**žánr**) a hodnota (**název žánru**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Úloha 9 - spuštění aplikace

V této úloze budete testovat, že se zobrazí každý žánr s odkazem na příslušné **/Store/Procházet** adresy URL.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/Store** k ověření, že každý žánr odkazuje na příslušnou **/Store/Procházet** adresy URL.

    ![Procházení žánry s odkazy na stránky Procházet](aspnet-mvc-4-fundamentals/_static/image33.png "procházení žánry s odkazy na stránky procházení")

    *Procházení žánry s odkazy na stránky procházení*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Úloha 10 - pomocí dynamické ViewModel kolekce k předání hodnot

V této úloze se dozvíte jednoduchá ale účinná metoda k předání hodnot mezi Kontrolerem a zobrazení bez jakýchkoli změn v modelu. ASP.NET MVC 4 poskytuje kolekci &quot;ViewModel&quot;, což může být přiřazena na libovolnou hodnotu dynamické a získat přístup do kontrolerů a zobrazení také.

Teď použijete dynamické kolekce položek ViewBag předat seznam &quot; **označený hvězdičkou žánry** &quot; z kontroleru zobrazení. Zobrazení indexu Store bude přístup k **ViewModel** a zobrazení informací.

1. Zavřete prohlížeč v případě potřeby se vraťte do okna nástroje Visual Studio. Otevřít **StoreController.cs** a upravit **Index** metodu pro vytvoření seznamu hvězdičkou žánry do ViewModel kolekce:

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Můžete také použít syntaxi **objekt ViewBag [&quot;Starred&quot;]** pro přístup k vlastnosti.
2. Ikona hvězdičky **&quot;starred.png&quot;** je součástí **Source\Assets\Images** složka tohoto testovacího prostředí. Chcete-li přidat ji do aplikace, přetáhněte jejich obsah z **Windows Explorer** okno do **Průzkumníka řešení** v aplikaci Visual Web Developer Express, jak je znázorněno níže:

    ![Přidáním hvězdičky image do řešení](aspnet-mvc-4-fundamentals/_static/image34.png "přidáním hvězdičky image do řešení")

    *Přidání hvězdičky bitové kopie do řešení*
3. Otevřete zobrazení **Store/Index.cshtml** a upravovat obsah. Bude číst &quot;označený hvězdičkou&quot; vlastnost **objekt ViewBag** kolekce a požádejte ho, pokud aktuální žánr název je v seznamu. V takovém případě se zobrazí ikona hvězdičky zprava žánr odkaz.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Úloha 11 – spuštění aplikace

V této úloze budete testovat, Ohodnoťte žánry zobrazit označené ikonou s hvězdičkou.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Projekt se spustí v **Domů** stránky. Změňte adresu URL na **/Store** ověřte, že má každý žánr vybrané pečlivě popisku:

    ![Procházení žánry Ohodnoťte elementy](aspnet-mvc-4-fundamentals/_static/image35.png "procházení žánry Ohodnoťte elementy")

    *Procházení žánry Ohodnoťte elementy*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Cvičení 7: Kolečko okolo nové šablony ASP.NET MVC 4

V tomto cvičení bude prozkoumat rozšíření v šablonách projektů ASP.NET MVC 4, podívali se do nanejvýš relevantní funkce nové šablony.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Úloha 1: Zkoumání šablony ASP.NET MVC 4 Internetové aplikace

1. Pokud ještě není otevřený, začněte **VS Express for Web**
2. Vyberte **soubor | Nové | Projekt** příkazu nabídky. V **nový projekt** dialogového okna, vyberte **Visual C# | Web** šablony v levém podokně stromové struktury a zvolte **webové aplikace ASP.NET MVC 4**. **Název** projektu *MusicStore* a aktualizovat **název řešení** k *začít*, pak vyberte umístění (nebo ponechte výchozí nastavení) a klikněte na tlačítko **OK** .

    ![Vytvoření nového projektu ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "vytvoření nového projektu ASP.NET MVC 4")

    *Vytvoření nového projektu ASP.NET MVC 4*
3. V **nového projektu ASP.NET MVC 4** dialogového okna, vyberte **internetovou aplikaci** šablony projektu a klikněte na tlačítko **OK**. Všimněte si, že jste vybrali jako zobrazovací modul Razor nebo ASPX.

    ![Vytvoření nové aplikace ASP.NET MVC 4 Internet](aspnet-mvc-4-fundamentals/_static/image37.png "vytvoření nové aplikace ASP.NET MVC 4 Internet")

    *Vytvoření nové aplikace ASP.NET MVC 4 Internet*

    > [!NOTE]
    > Syntaxe Razor byla zavedena v architektuře ASP.NET MVC 3. Jeho cílem je minimalizovat počet znaků a stisknutí kláves vyžaduje v souboru, umožňuje rychlé a dynamika kódování pracovního postupu. Razor využívá existující C# /VB (nebo jiné) jazykové dovednosti a poskytuje šablonu syntaxe značek, umožňující pracovním Super konstrukce jazyka HTML.
4. Stisknutím klávesy **F5** ke spuštění řešení a zobrazit obnovené šablonu. Si můžete prohlédnout následující funkce:

    1. **Moderní styl šablony**

        Šablony byly obnoveny, poskytuje další styly moderního vzhledu.

        ![Šablony ASP.NET MVC 4 restyled](aspnet-mvc-4-fundamentals/_static/image38.png "restyled šablony ASP.NET MVC 4")

        *Šablony ASP.NET MVC 4 restyled*
    2. **Adaptivní vykreslování**

        Podívejte se na změně velikosti okna prohlížeče a Všimněte si, jak rozložení stránky dynamicky přizpůsobí novou velikost okna. Tyto šablony pomocí adaptivního vykreslování techniku správně vykreslit, desktopových a mobilních platforem bez jakéhokoli přizpůsobení.

        ![Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti](aspnet-mvc-4-fundamentals/_static/image39.png "šablonu projektu ASP.NET MVC 4 v jiném prohlížeči velikosti")

        *Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti*
5. Zavřete prohlížeč zastavení ladicího programu a vrátíte se do sady Visual Studio.
6. Nyní budete moci prozkoumat řešení a podívejte se na některé z nových funkcích zavedených v architektuře ASP.NET MVC 4 v šabloně projektu.

    ![Technologie ASP.NET MVC4 – internet--šablony projektu aplikace-](aspnet-mvc-4-fundamentals/_static/image40.png "šablonu projektu ASP.NET MVC 4 Internetové aplikace")

    *Šablona projektu ASP.NET MVC 4 Internetové aplikace*

   1. **Značek HTML5**

       Procházet šablony zobrazení a zjistěte, nové značky motiv, který je třeba otevřít **About.cshtml** zobrazit v rámci **Domů** složky.

       ![Nové šablony, pomocí značky Razor a HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "novou šablonu, pomocí značky Razor a HTML5")

       *Nové šablony, pomocí značky Razor a HTML5*
   2. **Zahrnuté knihovny jazyka JavaScript**

      1. **jQuery**: jQuery usnadňuje procházení dokumentu HTML, zpracování událostí, animace a interakce Ajax.
      2. **uživatelské rozhraní jQuery**: Tato knihovna poskytuje abstrakci pro nízké úrovni interakce a animace, pokročilé efekty a bylo widgetů, postavený na Javascriptovou knihovnu jQuery.

         > [!NOTE]
         > Informace o jQuery a uživatelské rozhraní jQuery v [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).
      3. **KnockoutJS**: nyní zahrnuje výchozí šablonu pro ASP.NET MVC 4 **KnockoutJS**, rozhraní MVVM jazyka JavaScript, které vám umožní vytvářet bohaté a s velmi rychlou odezvou webové aplikace pomocí jazyků JavaScript a HTML. Stejně jako v architektuře ASP.NET MVC 3, jQuery a knihovny uživatelského rozhraní jQuery jsou také zahrnuté v architektuře ASP.NET MVC 4.

          > [!NOTE]
          > Můžete získat další informace o knihovně KnockOutJS v tomto odkazu: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).
      4. **Modernizr**: Tato knihovna spouští automaticky, vytváření webu kompatibilní s starší prohlížeče, při použití technologií HTML5 a CSS3.

          > [!NOTE]
          > Můžete získat další informace o knihovně Modernizr v tomto odkazu: [ http://www.modernizr.com/ ](http://www.modernizr.com/).
   3. **SimpleMembership zahrnutý v řešení**

       SimpleMembership byly navržené jako náhrada za předchozí systému zprostředkovatele rolí ASP.NET a členství. Obsahuje mnoho nových funkcí, které usnadňují pro vývojáře na zabezpečené webové stránky tak flexibilnější.

       Šablona Internet již má nastavit pár věcí, které integrují SimpleMembership, například AccountController připravena k použití OAuthWebSecurity (pro registraci účtu OAuth, přihlášení, správu, atd.) a zabezpečení webového.

       ![SimpleMembership zahrnutý v řešení](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership zahrnutý v řešení")

       *SimpleMembership zahrnutý v řešení*

       > [!NOTE]
       > Najít další informace o [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) na webu MSDN.

> [!NOTE]
> Kromě toho můžete nasadit tuto aplikaci následující weby Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Po dokončení tohoto praktického testovacího prostředí jste se naučili základy ASP.NET MVC:

- Základní prvky aplikace MVC a jak pracují
- Jak vytvořit aplikaci ASP.NET MVC
- Postup přidání a konfigurace řadiče pro zpracování parametry předat prostřednictvím adresy URL a řetězec dotazu
- Postup přidání rozložení stránky předlohy k nastavení šablony pro běžné obsah HTML, šablony stylů k vylepšení vzhledu a chování a zobrazit šablonu pro zobrazení obsahu HTML
- Použití vzoru ViewModel pro předání vlastnosti chcete zobrazit šablonu k zobrazení dynamických informací
- Jak používat parametry předané do řadičů v zobrazení šablony
- Jak přidat odkazy na stránky v aplikaci ASP.NET MVC
- Jak přidat a používat dynamické vlastnosti v zobrazení
- Rozšíření v šablonách projektů ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.

    ![Instalace sady Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "instalace sady Visual Studio Express")

    *Instalace sady Visual Studio Express*
4. Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Přijetí podmínek licence](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Přijetí podmínek licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Průběh instalace*
6. Až instalace skončí, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.

    ![VS Express for Web dlaždice](aspnet-mvc-4-fundamentals/_static/image47.png)

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

    ![Přihlaste se k portálu Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k portálu pro správu Azure Windows*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](aspnet-mvc-4-fundamentals/_static/image49.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **Compute** | **webu**. Potom vyberte **rychlé vytvoření** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvořit web**.

    > [!NOTE]
    > Hostitel pro webovou aplikaci spuštěnou v cloudu, který může řídit a spravovat je web Windows Azure. Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál. Nezahrnuje kroky pro vytvoření databáze.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](aspnet-mvc-4-fundamentals/_static/image50.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** se vytvoří.
5. Po vytvoření webové stránky, klikněte na odkaz v části **URL** sloupce. Zkontrolujte, jestli funguje nový web.

    ![Na nový web](aspnet-mvc-4-fundamentals/_static/image51.png "přechodu na nový web")

    *Procházení na nový web*

    ![Spuštění webu](aspnet-mvc-4-fundamentals/_static/image52.png "spuštění webové stránky")

    *Spuštění webové stránky*
6. Přejděte zpět na portál a klikněte na název webové stránky v části **název** sloupec, který se zobrazí na stránkách správy.

    ![Otevřete správu webových stránek](aspnet-mvc-4-fundamentals/_static/image53.png "otevřete správu webových stránek")

    *Otevřete správu webových stránek*
7. V **řídicí panel** stránce v části **rychlý přehled** klikněte na tlačítko **stáhnout profil publikování** odkaz.

    > [!NOTE]
    > *Profil publikování* obsahuje všechny informace požadované pro publikování webových aplikací na webu Windows Azure pro každou metodu povoleno publikování. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a databázové řetězce požadované k připojení a ověřování na základě jednotlivých koncových bodů, u kterých je povolena metoda publikace. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** a **Microsoft Visual Studio 2012** podporují čtení publikační profily k automatizaci konfigurace z těchto programů pro publikování webových aplikací na Windows Azure websites.

    ![Stahování webové stránky publikovat profil](aspnet-mvc-4-fundamentals/_static/image54.png "stahování webové stránky profil publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profilu publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak publikovat webovou aplikaci na weby Windows Azure ze sady Visual Studio pomocí tohoto souboru.

    ![Ukládání souboru profilu publikování](aspnet-mvc-4-fundamentals/_static/image55.png "ukládá se profil publikování")

    *Ukládá se profil publikování*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá SQL Server databáze, budete muset vytvořit server služby SQL Database. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá SQL Server může tuto úlohu přeskočit.

1. Pro uložení databáze aplikace budete potřebovat databázi SQL serveru. Servery SQL Database můžete zobrazit ze svého předplatného na portálu správy Windows Azure na **databází Sql** | **servery** | **serveru Řídicí panel**. Pokud nemáte server vytvořili, můžete vytvořit jednu **přidat** tlačítko na panelu příkazů. Poznamenejte si **název serveru a adresu URL, správce přihlašovací jméno a heslo**, jak je použijete v další úkoly. Nevytvářet databáze, protože vytvoří se v pozdější fázi.

    ![Řídicí panel serveru SQL Database](aspnet-mvc-4-fundamentals/_static/image56.png "řídicího panelu serveru SQL Database")

    *Řídicí panel serveru SQL Database*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio z tohoto důvodu je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. Chcete-li to mohli udělat, klikněte na tlačítko **konfigurovat**, vyberte IP adresu z **aktuální IP adresa klienta** a vložte ho na **počáteční IP adresa** a **Koncová IP adresa** textová pole a klikněte na tlačítko ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) tlačítko.

    ![Přidat IP adresu klienta](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Přidat IP adresu klienta*
3. Jednou **IP adresa klienta** je přidat do povolených IP adres klikněte na tlačítko na **Uložit** potvrďte provedené změny.

    ![Potvrzení změn](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Potvrzení změn*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nasazení webu

1. Vraťte se do řešení ASP.NET MVC 4. V **Průzkumníka řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](aspnet-mvc-4-fundamentals/_static/image60.png "publikování aplikace")

    *Publikování na webu*
2. Importujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](aspnet-mvc-4-fundamentals/_static/image61.png "import profilu publikování")

    *Import publikačního profilu*
3. Klikněte na tlačítko **ověřit připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření bylo dokončeno, jakmile se zobrazí zelené zaškrtnutí vedle tlačítka ověřit připojení.

    ![Ověřuje se připojení](aspnet-mvc-4-fundamentals/_static/image62.png "ověřuje se připojení")

    *Ověřuje se připojení*
4. V **nastavení** stránce v části **databází** klikněte na tlačítko vedle textového pole připojení k databázi (to znamená **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](aspnet-mvc-4-fundamentals/_static/image63.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Konfigurace připojení k databázi následujícím způsobem:

   - V **název serveru** zadejte vaše pomocí adresy URL databáze SQL serveru *tcp:* předponu.
   - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
   - V **heslo** zadejte heslo pro přihlašovací jméno správce serveru.
   - Zadejte nový název databáze, například: *MVC4SampleDB*.

     ![Konfigurace cílový připojovací řetězec](aspnet-mvc-4-fundamentals/_static/image64.png "konfigurace cílový připojovací řetězec")

     *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze klikněte na **Ano**.

    ![Vytvoření databáze](aspnet-mvc-4-fundamentals/_static/image65.png "vytvoření řetězce databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL ve službě Windows Azure se zobrazí v textovém poli výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "připojovací řetězec odkazující na SQL Database")

    *Připojovací řetězec odkazující na SQL Database*
8. V **ve verzi Preview** klikněte na **publikovat**.

    ![Publikování webové aplikace](aspnet-mvc-4-fundamentals/_static/image67.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Až se proces publikování dokončí, otevře se váš výchozí prohlížeč publikovaného webu.

    ![Publikování aplikace do Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "publikování aplikace do Windows Azure")

    *Aplikace publikovaná do Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Příloha C: používání fragmentů kódu

Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky. Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-fundamentals/_static/image69.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")

*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.
4. Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).
5. Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.

![Začněte psát název fragmentu kódu](aspnet-mvc-4-fundamentals/_static/image70.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](aspnet-mvc-4-fundamentals/_static/image71.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")

*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*

![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](aspnet-mvc-4-fundamentals/_static/image72.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")

*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.
2. Kliknutím na vyberte relevantní fragment kódu ze seznamu.

![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-fundamentals/_static/image73.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-fundamentals/_static/image74.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")

*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*
