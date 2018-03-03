---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: "Základy architektury ASP.NET MVC 4 | Microsoft Docs"
author: rick-anderson
description: "Toto testovací prostředí Hands-On vychází z úložiště Hudba MVC (Model View Controller), kurz aplikace, která představuje a vysvětluje podrobný postup používání ASP.NET MV..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: f93f51219403cd5aeca2dd3648444a84690c3d25
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-fundamentals"></a>Základy architektury ASP.NET MVC 4

Podle [webové táborech Team](https://twitter.com/webcamps)

[Stažení webové táborech cvičení Kit](https://aka.ms/webcamps-training-kit)

Toto testovací prostředí Hands-On je založena na rozhraní MVC (Model View Controller) Hudba úložiště, kurz aplikace, která uvádí a popisuje podrobný aplikace ASP.NET MVC a Visual Studio. V testovacím prostředí se dozvíte, jednoduchost, ještě power společně používání těchto technologií. Se spustí s jednoduchou aplikaci a bude sestavte jej, dokud nebudete mít plně funkční ASP.NET MVC 4 webovou aplikaci.

Toto testovací prostředí pracuje s ASP.NET MVC 4.

Pokud chcete prozkoumat verze ASP.NET MVC 3 kurz aplikace, najdete ji v [MVC. Hudba úložiště](https://github.com/evilDave/MVC-Music-Store).

Toto testovací prostředí Hands-On předpokládá, že vývojář má prostředí do webové vývoj technologií, jako je například HTML a JavaScript.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [verze Microsoft-webové/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 Základy](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>Aplikaci Store Hudba

Hudba úložiště webové aplikace, které budou vytvořeny v celém tomto testovacím prostředí zahrnuje tři hlavní části: nákupy, najdete v článku věnovaném a správu. Bude moct procházet alb podle genre, přidejte do své košíku alb, zkontrolujte jejich výběr a nakonec pokračujte najdete v článku věnovaném přihlášení a dokončení pořadí návštěvníky. Kromě toho Správci úložiště bude možné spravovat dostupné alb, jakož i jejich hlavní vlastnosti.

![Hudba úložiště obrazovky](aspnet-mvc-4-fundamentals/_static/image1.png "obrazovky Hudba úložiště")

*Hudba úložiště obrazovky*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

Aplikaci Store Hudba budou vytvořeny pomocí **řadiče MVC (Model View)**, architekturní vzor, který rozděluje aplikace do tří hlavních součástí:

- **Modely**: objekty modelů jsou části aplikace, které implementují logiku domény. Objekty modelů často také načíst a ukládají stav modelu v databázi.
- **Zobrazení:** zobrazení jsou komponenty, které zobrazují aplikace uživatelské rozhraní (UI). Toto uživatelské rozhraní se obvykle je vytvořena z dat modelu. Příkladem může být zobrazení upravit alb, které zobrazuje textová pole a rozevírací seznam na základě aktuálního stavu objektu alb.
- **Řadiče:** Kontrolery jsou komponenty, které zpracovávají interakci s uživatelem, pracovat s modelem a konečně vybírají zobrazení k vykreslení uživatelského rozhraní. V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce.

Vzor MVC pomáhá vytvářet aplikace, které separují jednotlivé aspekty aplikace (logika vstupu, obchodní logika a logika uživatelského rozhraní) současně poskytují volné párování mezi těmito elementy. Tato separace pomáhá spravovat složitost, když vytvoříte aplikaci, jako umožňuje zaměřit se na jeden aspekt jejich implementace najednou. Kromě toho vzor MVC usnadňuje testování aplikací, také podpora pro vytváření aplikací pro používání testy řízený vývoj (TDD).

**ASP.NET MVC** framework představuje alternativu ke vzoru webových formulářů ASP.NET pro vytváření založené na ASP.NET MVC webových aplikací. **ASP.NET MVC** framework je odlehčený, intenzivního prezentační architektura která (stejně jako u aplikace využívající webové formuláře) je integrována stávajících funkcí technologie ASP.NET, jako je například hlavní stránky a na základě členství ověřování tak, abyste dosáhli všech power základní rozhraní .NET Framework. To je užitečné, pokud jste již obeznámeni s webovými formuláři ASP.NET protože všech knihoven, které už používáte jsou také k dispozici v architektuře ASP.NET MVC 4.

Kromě toho volné párování mezi třemi hlavními komponentami architektury MVC rovněž podporuje paralelní vývoj. Například jeden vývojář může pracovat na zobrazení, druhý může pracovat na logice kontroleru a třetí můžete soustředit na obchodní logiku v modelu.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí Hands-On se dozvíte, jak:

- Vytvoření aplikace ASP.NET MVC od začátku podle kurzu aplikaci Store Hudba
- Přidání řadičů pro zpracování adresy URL na domovské stránce webu a jeho hlavní funkce procházení
- Přidání zobrazení upravit obsah zobrazený spolu s jeho styl
- Přidání třídy modelu obsahovat a spravovat data a domény logiku
- Použití vzoru zobrazení modelu k předávání informací z akce kontroleru zobrazení šablon
- Prozkoumat nové šablony ASP.NET MVC 4 pro internetové aplikace

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Musíte mít následující položky k dokončení tohoto testovacího prostředí:

- [Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Instalace

**Instalace fragmenty kódu**

Pro usnadnění práce většinu kódu, který bude spravovat podél toto testovací prostředí je k dispozici jako fragmenty kódu v sadě Visual Studio. K instalaci fragmenty kódu spustit **.\Source\Setup\CodeSnippets.vsi** souboru.

Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha C:](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto testovací prostředí Hands-On se skládá ve cvičeních následující:

1. [Cvičení 1: Vytvoření projektu webové aplikace pomocí MusicStore rozhraní ASP.NET MVC](#Exercise1)
2. [Cvičení 2: Vytvoření řadiče](#Exercise2)
3. [Cvičení 3: Předávání na řadič parametrů](#Exercise3)
4. [Cvičení 4: Vytvoření zobrazení](#Exercise4)
5. [Cvičení 5: Vytvoření modelu zobrazení](#Exercise5)
6. [Cvičení 6: Pomocí parametrů v zobrazení](#Exercise6)
7. [Cvičení 7: Okruhu kolem ASP.NET MVC 4 nové šablony](#Exercise7)

> [!NOTE]
> Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.


Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Cvičení 1: Vytvoření projektu webové aplikace pomocí MusicStore rozhraní ASP.NET MVC

V tomto cvičení se dozvíte, jak vytvořit aplikaci ASP.NET MVC v aplikaci Visual Studio 2012 Express pro Web, jakož i jeho hlavní složky organizaci. Kromě toho se dozvíte, jak přidat nový řadič a změňte ji zobrazit jednoduchým řetězcem aplikace domovské stránce.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Úloha 1 – Vytvoření projektu webové aplikace ASP.NET MVC

1. V této úloze vytvoříte pomocí šablony sady Visual Studio MVC prázdný projekt aplikace ASP.NET MVC. Spustit **VS Express pro Web**.
2. Na **soubor** nabídky, klikněte na tlačítko **nový projekt**.
3. V **nový projekt** dialogovém okně vyberte pole **webové aplikace ASP.NET MVC 4** projektu typu, najdete v **Visual C#,** **webové** šablony seznam.
4. Změna **název** k *MvcMusicStore*.
5. Nastavte umístění, řešení uvnitř novou **začít** složky v tomto cvičení zdrojové složky, například **[YOUR-HOL-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**. Click **OK**.

    ![Vytvořit dialogové okno Nový projekt](aspnet-mvc-4-fundamentals/_static/image2.png "vytvořit dialogové okno Nový projekt")

    *Vytvořit dialogové okno Nový projekt*
6. V **nový ASP.NET MVC 4 projekt** dialogovém okně vyberte pole **základní** šablony a ujistěte se, že **zobrazovací modul** vybraná **Razor**. Click **OK**.

    ![ASP.NET MVC 4 projektu dialogové okno Nový](aspnet-mvc-4-fundamentals/_static/image3.png "architektury ASP.NET MVC 4 projektu dialogové okno Nový")

    *ASP.NET MVC 4 projektu dialogové okno Nový*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Úloha 2 – prohlížení struktury řešení

Rozhraní ASP.NET MVC zahrnuje šablona projektu sady Visual Studio, která vám pomůže vytvořit webové aplikace podporující vzor MVC. Tato šablona vytvoří novou aplikaci ASP.NET MVC Web s potřebné složky, šablony položek a položek konfigurace souboru.

V této úloze prozkoumáte strukturu řešení pochopit prvky, které se podílejí a jejich vztahů. Následující složky jsou součástí všechny aplikace ASP.NET MVC, protože rozhraní ASP.NET MVC ve výchozím nastavení používá &quot;konvence přes konfigurace&quot; přístup a díky některé předpoklady výchozí podle složky pojmenování konvence.

1. Po vytvoření projektu, projděte si strukturu složek, který byl vytvořen v Průzkumníku řešení na pravé straně:

    ![Struktura složek ASP.NET MVC v Průzkumníku řešení](aspnet-mvc-4-fundamentals/_static/image4.png "struktura složek ASP.NET MVC v Průzkumníku řešení")

    *Struktura složek ASP.NET MVC v Průzkumníku řešení*

    1. **Řadiče**. Tato složka bude obsahovat třídy kontroleru. V aplikaci MVC na základě řadiče jsou zodpovědná za zpracování interakce s koncovým uživatelem, manipulace s modelem a nakonec výběr zobrazení k vykreslení uživatelského rozhraní.

        > [!NOTE]
        > Rozhraní MVC požaduje názvy všech řadičů tak, aby končit &quot;řadič&quot;– například HomeController, LoginController nebo ProductController.
    2. **Modely**. Tato složka se poskytuje třídy, které představují aplikačního modelu pro MVC webové aplikace. To obvykle zahrnuje kód, který definuje objekty a logiku pro interakci s úložištěm dat. Obvykle objekty skutečné modelu bude v samostatné třídy knihovny. Ale když vytvoříte novou aplikaci, můžete zahrnout třídy a přesuňte je do knihovny tříd samostatné později v cyklu vývoje.
    3. **Zobrazení**. Tato složka je doporučené umístění pro zobrazení, komponenty zodpovědná za zobrazení uživatelského rozhraní aplikace. Zobrazení souborů .aspx, .ascx, .cshtml a .master pomocí všech ostatních souborů, které se vztahují k vykreslení zobrazení. Složka zobrazení obsahuje složku pro každý řadič; Složka služby se nazývá s předponou názvu kontroleru. Například, pokud se jedná o zařízení s názvem **HomeController**, složce zobrazení bude obsahovat složku s názvem Domů. Ve výchozím nastavení, pokud rozhraní ASP.NET MVC načte zobrazení, hledá soubor .aspx s názvem požadované zobrazení ve složce Views\controllerName (**zobrazení [ControllerName] [akce] .aspx**) nebo (**zobrazení [ControllerName] [Akce] .cshtml**) pro zobrazení syntaxe Razor.

    > [!NOTE]
    > Kromě složky uvedených výše, používá MVC webovou aplikaci **Global.asax** výchozí soubor, aby globální směrování adres URL a používá **Web.config** soubor konfigurace aplikace.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Úloha 3 – Přidání HomeController

V aplikacích ASP.NET, které nepoužívají rozhraní MVC je organizovaná interakci s uživatelem okolo stránek a kolem vyvolání a zpracování události z tyto stránek. Interakce uživatele s aplikací ASP.NET MVC je naopak věnuje řadiče a jejich metody akce.

Na druhé straně rozhraní ASP.NET MVC mapuje adresy URL do tříd, které se označují jako řadiče. Řadiče zpracovat příchozí žádosti, zpracování uživatelského vstupu a interakce, spouštět logiku příslušné aplikace a určení odpověď k odeslání zpět do klienta (zobrazení HTML, stažení souboru, přesměrovat na jinou adresu URL, atd.). V případě zobrazení HTML, třídy kontroleru obvykle volá komponentu oddělená zobrazení pro generování kód HTML pro daný požadavek. V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce.

V této úloze přidáte řadič třídu, která bude zpracovávat adresy URL na domovské stránce webu Hudba úložiště.

1. Klikněte pravým tlačítkem na **řadiče** složky v Průzkumníku řešení, vyberte **přidat** a potom **řadič** příkaz:

    ![Přidat řadič příkaz](aspnet-mvc-4-fundamentals/_static/image5.png "přidat příkaz řadiče")

    *Přidat řadič – příkaz*
2. **Přidat kontroler** otevře se dialogové okno. Název kontroleru *HomeController* a stiskněte klávesu **přidat**.

    ![Dialogové okno řadiče přidání](aspnet-mvc-4-fundamentals/_static/image6.png "řadiče dialogové okno Přidání")

    *Dialogové okno řadiče přidání*
3. Soubor **HomeController.cs** je vytvořen v **řadiče** složky. Aby bylo možné používat **HomeController** vrátí řetězec na jeho akce indexu, nahraďte **Index** metoda následujícím kódem:

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex1 HomeController Index*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Úloha 4 – spuštění aplikace

V této úloze můžete vyzkoušet na aplikaci ve webovém prohlížeči.

1. Stiskněte klávesu **F5** ke spuštění aplikace. Kompilace projektu a spustí místní webový Server IIS. Místní webový Server IIS se automaticky otevře webový prohlížeč, přejdete na adresu URL webového serveru.

    ![Aplikace běžící ve webovém prohlížeči](aspnet-mvc-4-fundamentals/_static/image7.png "aplikaci spuštěnou ve webovém prohlížeči")

    *Aplikace běžící ve webovém prohlížeči*

    > [!NOTE]
    > Místního webového serveru IIS se spustí na webu na náhodných volné portu s číslem. Ve výše uvedeném obrázku webu běží v `http://localhost:50103/`, takže používá port 50103. Vaše číslo portu se může lišit.
2. Zavřete prohlížeč.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Cvičení 2: Vytvoření řadiče

V tomto cvičení se dozvíte, jak k aktualizaci řadiče implementovat jednoduché funkce aplikace Hudba úložiště. Tomuto řadiči definují akce metody pro zpracování každé z následujících konkrétní požadavky:

- Stránka výpis žánry Hudba v úložišti Hudba
- Procházet stránky, který se zobrazí seznam všech hudebních alb pro konkrétní genre
- Stránka Podrobnosti, která se zobrazují informace o konkrétní Hudba album

Pro obor tohoto cvičení tyto akce jednoduše vrátí řetězec nyní.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Úloha 1 – přidání nové třídy StoreController

V této úloze přidáte nový řadič.

1. Pokud už otevřený, spusťte **VS Express pro Web 2012**.
2. V **soubor** nabídce zvolte **otevřít projekt**. V dialogovém okně Otevřít projekt přejděte do **Source\Ex02 CreatingAController\Begin**, vyberte **Begin.sln** a klikněte na tlačítko **otevřete**. Alternativně můžete pokračovat s řešením jste získali po dokončení předchozím cvičení.

    1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
    2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
    3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

    > [!NOTE]
    > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Přidáte nový řadič. Chcete-li to provést, klikněte pravým tlačítkem **řadiče** složky v Průzkumníku řešení, vyberte **přidat** a potom **řadič** příkaz. Změna **názvu Kontroleru** k *StoreController*a klikněte na tlačítko **přidat**.

    ![Dialogové okno řadiče přidání](aspnet-mvc-4-fundamentals/_static/image8.png "řadiče dialogové okno Přidání")

    *Dialogové okno řadiče přidání*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Úloha 2 – úpravy StoreController akce

Tato úloha slouží k úpravě řadiče metody, které se nazývají **akce**. Akce jsou zodpovědná za zpracování žádostí adresy URL a určení, obsah, který by měly být odeslány zpět do prohlížeče nebo uživatele, který volá adresu URL.

1. **StoreController** třídy již **Index** metoda. Použijete ho později v tomto testovacím prostředí implementovat stránka, která obsahuje seznam všech žánry Hudba úložiště. Prozatím se právě nahradit **Index** metoda s následující kód, který vrací řetězec &quot;Hello z Store.Index()&quot;:

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex2 StoreController Index*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Přidat **Procházet** a **podrobnosti** metody. K tomu, přidejte následující kód, který **StoreController**:

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Úloha 3 – spuštění aplikace

V této úloze můžete vyzkoušet na aplikaci ve webovém prohlížeči.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí v **Domů** stránky. Změňte adresu URL k ověření implementace každá akce.

    1. **/ Ukládání**. Zobrazí se  **&quot;Hello z Store.Index()&quot;**.
    2. **/ Úložiště nebo procházení**. Zobrazí se  **&quot;Hello z Store.Browse()&quot;**.
    3. **/ / Podrobnosti úložiště**. Zobrazí se  **&quot;Hello z Store.Details()&quot;**.

        ![Procházení StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "procházení StoreBrowse")

        *Procházení /Store/Browse*
3. Zavřete prohlížeč.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Cvičení 3: Předávání na řadič parametrů

Dosud jste byla vrací konstantní řetězce z řadičů. V tomto cvičení se dozvíte, jak předat parametry do řadiče pomocí adresy URL a řetězce dotazu a pak provedením metoda akce odpovídat text do prohlížeče.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Úloha 1 – Přidání parametru Genre StoreController

V této úloze budete používat **řetězce dotazu** odeslat parametry, které **Procházet** metodu akce v **StoreController**.

1. Pokud už otevřený, spusťte **VS Express pro Web**.
2. V **soubor** nabídce zvolte **otevřít projekt**. V dialogovém okně Otevřít projekt přejděte do **Source\Ex03 PassingParametersToAController\Begin**, vyberte **Begin.sln** a klikněte na tlačítko **otevřete**. Alternativně můžete pokračovat s řešením jste získali po dokončení předchozím cvičení.

    1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
    2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
    3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

    > [!NOTE]
    > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Otevřete **StoreController** třídy. Chcete-li to provést, v **Průzkumníku řešení**, rozbalte **řadiče** složku a dvojím kliknutím **StoreController.cs**.
4. Změna **Procházet** metoda, přidání k vyžádání pro konkrétní genre parametr řetězce. ASP.NET MVC automaticky předat žádné řetězce dotazu nebo zpracování odeslaného formuláře Parametry s názvem **genre** k této metodě akce při vyvolání. Chcete-li to provést, nahraďte **Procházet** metoda následujícím kódem:

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - EX3. StoreController BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

    > [!NOTE]
    > Používáte **HttpUtility.HtmlEncode** nástroj metodu zabraňuje uživatelům vložení Javascript do zobrazení s odkazem jako   **/úložiště/Procházet? Genre =&lt;skriptu&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.
    > 
    > Další vysvětlení, navštivte [tohoto článku na webu msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Úloha 2 – spuštění aplikace

V této úloze, můžete vyzkoušet na aplikaci ve webovém prohlížeči a používat **genre** parametr.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí v **Domů** stránky. Změňte adresu URL na   */ukládání/Procházet? Genre = Disco* k ověření, že akce obdrží parametr genre.

    ![Procházení StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "procházení StoreBrowseGenre = Disco")

    *Browsing /Store/Browse?Genre=Disco*
3. Zavřete prohlížeč.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Úloha 3 – Přidání parametru Id adresy URL

V této úloze budete používat **URL** předat **Id** parametru **podrobnosti** metody akce **StoreController**. ASP.NET MVC výchozích konvencí směrování se zachází segment adresy URL po název metody akce jako parametr s názvem **Id**. Pokud vaše metoda akce má parametr s názvem Id, potom ASP.NET MVC automaticky předá segment adresy URL je jako parametr. V adrese URL **úložiště/podrobnosti/5**, **Id** , bude vyhodnocen jako **5**.

1. Změna **podrobnosti** metodu **StoreController**, přidání **int** parametr s názvem **id**. Chcete-li to provést, nahraďte **podrobnosti** metoda následujícím kódem:

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - EX3. StoreController DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Úloha 4 – spuštění aplikace

V této úloze, můžete vyzkoušet na aplikaci ve webovém prohlížeči a používat **Id** parametr.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí v **Domů** stránky. Změňte adresu URL na */Store/Details/5* k ověření, že akce obdrží parametr id.

    ![Procházení StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "procházení StoreDetails5")

    *Procházení /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Cvičení 4: Vytvoření zobrazení

Mít byla dosavadní vrácení řetězce z akce kontroleru. I když snadno pochopení fungování řadiče, je to, jak nejsou vytvořené skutečné webových aplikací. Zobrazení jsou součástí, které poskytují lepší přístup ke generování HTML zpět do prohlížeče s použitím soubory šablon.

V tomto cvičení se dozvíte, jak přidat rozložení stránky předlohy nastavit šablonu pro běžné obsah HTML, šablony stylů k vylepšení vzhledu a chování webu a v neposlední řadě zobrazit šablonu umožňující HomeController vrátit HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Úloha 1 - upravovat soubor \_layout.cshtml

Soubor **~/Views/Shared/\_layout.cshtml** umožňuje nastavit šablonu pro běžné HTML a použít v rámci celého webu. V této úloze přidáte rozložení stránky předlohy s běžné hlavička s odkazy na oblasti domovské stránky a úložiště.

1. Pokud už otevřený, spusťte **VS Express pro Web**.
2. V **soubor** nabídce zvolte **otevřít projekt**. V dialogovém okně Otevřít projekt přejděte do **Source\Ex04 CreatingAView\Begin**, vyberte **Begin.sln** a klikněte na tlačítko **otevřete**. Alternativně můžete pokračovat s řešením jste získali po dokončení předchozím cvičení.

    1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
    2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
    3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

    > [!NOTE]
    > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Soubor  **\_layout.cshtml** obsahuje rozložení HTML kontejner pro všechny stránky webu. Obsahuje  **&lt;html&gt;**  element pro odpovědi HTML a taky  **&lt;head&gt;**  a  **&lt;textu&gt;**  elementy. **@RenderBody()** v kódu HTML textu identifikovat oblasti tohoto zobrazení šablony budou moci uživatelé zadat dynamický obsah.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Přidáte hlavičku běžné s odkazy do oblasti domovské stránky a úložiště na všech stránkách v této lokalitě. Aby bylo možné provést, přidejte následující kód níže &lt;textu&gt; příkaz.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Zahrnout div k vykreslení části textu každé stránce. Nahraďte  **@RenderBody()** následujícím kódem higlighted: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Věděli jste? Visual Studio 2012 má fragmenty kódu, které usnadňují přidejte běžně používané kód HTML, soubory kódu a další! Vyzkoušet odhlašování zadáním  **&lt;div&gt;**  a stisknutím klávesy **KARTĚ** dvakrát k vložení úplná **div** značky.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Úloha 2 – Přidání šablony stylů CSS

Šablona prázdný projekt obsahuje soubor velmi efektivní šablon stylů CSS, který obsahuje pouze styly slouží k zobrazení základní formulářů a ověřovacích zpráv. Chcete-li vylepšení vzhledu a chování webu budete používat další šablon stylů CSS a bitové kopie (potenciálně poskytované návrháře).

V této úloze budete přidávat šablonu stylů CSS k definování stylů webu.

1. Soubor CSS a image jsou součástí **Source\Assets\Content** složky tohoto testovacího prostředí. Chcete-li přidat je do aplikace, přetáhněte obsah z **Průzkumníka Windows** okno na **Průzkumníku řešení** ve Visual Studio Express pro Web, jak je uvedeno níže:

    ![Přetahování styl obsah](aspnet-mvc-4-fundamentals/_static/image12.png "přetahování styl obsah")

    *Přetahování styl obsah*
2. Dialogové okno upozornění se zobrazí, žádostí o potvrzení nahradit **Site.css** soubor a některé existujících bitových kopií. Zkontrolujte **použít pro všechny položky** a klikněte na tlačítko **Ano**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Úloha 3 – Přidání zobrazit šablonu

V této úloze budete přidávat šablony zobrazení pro generování odpovědi HTML, který bude používat rozložení stránky předlohy a šablon stylů CSS přidat v tomto cvičení.

1. Pro šablonu zobrazení procházení na domovskou stránku, budete nejprve muset označuje, že místo vrací řetězec, **HomeController Index** metoda vrátí **zobrazení**. Otevřete **HomeController** třídy a změňte jeho **Index** metodu pro návrat **ActionResult**, a mějte ho vrátit **View()**.

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex4 HomeController Index*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Teď je potřeba přidat šablonu odpovídající zobrazení. K tomu, **klikněte pravým tlačítkem na** uvnitř **Index** metody akce a vyberte **přidat zobrazení**. Tím se otevře **přidat zobrazení** dialogové okno.

    ![Přidání zobrazení z v rámci metody Index](aspnet-mvc-4-fundamentals/_static/image13.png "přidávání zobrazení v aplikaci Index – metoda")

    *Přidání zobrazení v aplikaci Index – metoda*
3. **Přidat zobrazení** ke generování souboru šablony zobrazení zobrazí se dialogové okno. Ve výchozím nastavení toto dialogové okno předem vyplní název šablony zobrazení tak, aby odpovídala metody akce, která bude používat. Protože jste použili **přidat zobrazení** místní nabídky v rámci **Index** metody akce v rámci HomeController, **přidat zobrazení** dialogové okno má Index jako výchozí název zobrazení. Klikněte na tlačítko **přidat**.

    ![Přidat Dialog zobrazení](aspnet-mvc-4-fundamentals/_static/image14.png "Přidat Dialog zobrazení")

    *Přidat Dialog zobrazení*
4. Visual Studio generuje **Index.cshtml** zobrazit šablonu uvnitř **Views\Home** složku a pak ho otevře.

    ![Domácí Index zobrazení vytvořené](aspnet-mvc-4-fundamentals/_static/image15.png "Domů Index zobrazení vytvořené")

    *Domácí Index zobrazení vytvořené*

    > [!NOTE]
    > název a umístění **Index.cshtml** soubor je relevantní a dodržuje konvence pojmenování výchozí rozhraní ASP.NET MVC.
    > 
    > Složka \Views\**Domů** odpovídá názvu kontroleru (**Domů** řadič). Název šablony zobrazení (**Index**), odpovídá metoda akce kontroleru, který bude zobrazit zobrazení.
    > 
    > Tímto způsobem, ASP.NET MVC zabraňuje nutnosti explicitně určovat název nebo umístění zobrazení šablony při použití této zásady vytváření názvů lze vrátit zobrazení.
5. Je na základě vygenerované šablony zobrazení  **\_layout.cshtml** dříve definované šablony. Aktualizovat vlastnosti ViewBag.Title **Domů**a změnit hlavní obsahu **Toto je domovská stránka**, jak je znázorněno v následujícím kódu:


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Vyberte **MvcMusicStore** projekt v Průzkumníku řešení a stiskněte klávesu **F5** ke spuštění aplikace.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Úloha 4: ověření

Chcete-li ověřit, že jste správně provedli všechny kroky v předchozím cvičení, postupujte takto:

S aplikací v prohlížeči otevřít by měl Pamatujte, že:

1. Metoda akce indexu HomeController najít a zobrazit **\Views\Home\Index.cshtml** zobrazit šablony, i když kód volá **vrátit View()**, protože zobrazit šablonu a potom standardní zásady vytváření názvů.
2. Domovská stránka zobrazí zobrazení uvítací zprávy, které jsou definované v rámci **\Views\Home\Index.cshtml** zobrazit šablonu.
3. Domovská stránka používá  **\_layout.cshtml** šablony, a proto zobrazení uvítací zprávy je obsažena v rozložení standardní webu HTML.

    ![Domácí zobrazení Index pomocí definované LayoutPage a stylu](aspnet-mvc-4-fundamentals/_static/image16.png "Domů zobrazení Index pomocí definované LayoutPage a stylu")

    *Domácí zobrazení Index pomocí definované LayoutPage a stylu*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Cvičení 5: Vytvoření modelu zobrazení

Pokud jste provedli v zobrazeních Zobrazit pevně zakódované HTML, ale, aby bylo možné vytvářet dynamické webové aplikace, zobrazit šablonu získat informace z řadiče. Jednou z běžných technik má být použit pro tento účel je **ViewModel** vzor, který umožňuje Kontroleru Zabalit všechny informace potřebné ke generování odpovídající odpověď protokolu HTML.

V tomto cvičení se dozvíte, jak vytvořit třídu ViewModel a přidat požadované vlastnosti: počet žánry v úložišti a seznam těchto žánry. Je také aktualizovat StoreController používat vytvořený ViewModel a nakonec vytvoříte novou šablonu zobrazení, která se zobrazí vlastnosti uvedených na stránce.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Úloha 1 – Vytvoření třídy ViewModel

V této úloze vytvoříte ViewModel třídu, která bude implementovat scénář výpis genre úložiště.

1. Pokud už otevřený, spusťte **VS Express pro Web**.
2. V **soubor** nabídce zvolte **otevřít projekt**. V dialogovém okně Otevřít projekt přejděte do **Source\Ex05 CreatingAViewModel\Begin**, vyberte **Begin.sln** a klikněte na tlačítko **otevřete**. Alternativně můžete pokračovat s řešením jste získali po dokončení předchozím cvičení.

    1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
    2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
    3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

    > [!NOTE]
    > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Vytvoření **ViewModels** složku pro uložení ViewModel. Chcete-li to provést, klikněte pravým tlačítkem nejvyšší úrovně **MvcMusicStore** projekt, vyberte **přidat** a potom **novou složku**.

    ![Přidání do nové složky](aspnet-mvc-4-fundamentals/_static/image17.png "přidání do nové složky")

    *Přidání do nové složky*
4. Název složky *ViewModels*.

    ![ViewModels složky v Průzkumníku řešení](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels složky v Průzkumníku řešení")

    *ViewModels složky v Průzkumníku řešení*
5. Vytvoření **ViewModel** třídy. Chcete-li to provést, klikněte pravým tlačítkem na **ViewModels** složku nedávno vytvořili, vyberte **přidat** a potom **novou položku**. V části **kód**, vyberte **– třída** položky a název souboru *StoreIndexViewModel.cs*, pak klikněte na tlačítko **přidat**.

    ![Přidání nové třídy](aspnet-mvc-4-fundamentals/_static/image19.png "přidání nové třídy")

    *Přidání nové třídy*

    ![Vytvoření třídy StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "vytváření StoreIndexViewModel – třída")

    *Vytvoření třídy StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Úloha 2 – Přidání vlastnosti do třídy ViewModel

Existují dva parametry mají být předány z StoreController zobrazit šablonu Pokud chcete generovat očekávané odpovědi HTML: počet žánry v úložišti a seznam těchto žánry.

V této úloze přidáte tyto 2 vlastnosti, které chcete **StoreIndexViewModel** – třída: **NumberOfGenres** (celé číslo) a **žánry** (seznam řetězců).

1. Přidat **NumberOfGenres** a **žánry** vlastnosti, které chcete **StoreIndexViewModel** třídy. Chcete-li to provést, přidejte následující řádky 2 v definici třídy:

    (Code fragment kódu - *ASP.NET MVC 4 základy – vlastnosti Ex5 StoreIndexViewModel*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

    > [!NOTE]
    > **{Získat; nastavit;}**  zápis využívá jazyka C# na funkce automaticky implementované vlastnosti. Poskytuje výhody vlastnosti bez nutnosti nám základní pole deklarovat.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Úloha 3 - aktualizace StoreController používat StoreIndexViewModel

**StoreIndexViewModel** třída zapouzdří informace potřebné k předání z **StoreController**na **Index** metodu zobrazit šablonu, aby bylo možné generovat odpověď .

V této úloze budete aktualizovat **StoreController** používat **StoreIndexViewModel**.

1. Otevřete **StoreController** třídy.

    ![Otevírání StoreController třída](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController otevírání – třída")

    *Otevírání StoreController – třída*
2. Chcete-li použít **StoreIndexViewModel** třídy z **StoreController**, přidat následující obor názvů v horní části **StoreController** kódu:

    (Code fragment kódu - *ASP.NET MVC 4 základy - Ex5 StoreIndexViewModel pomocí ViewModels*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Změna **StoreController**na **Index** metody akce, které se vytvoří a naplní **StoreIndexViewModel** objektu a předává je pro šablonu zobrazení generování odpovědi HTML s ním.

    > [!NOTE]
    > V testovacím prostředí &quot;modely ASP.NET MVC a přístup k datům&quot; budete psát kód, který načte seznam žánry úložiště z databáze. V následujícím kódu vytvoříte **seznamu** z žánry fiktivní data, která se naplní **StoreIndexViewModel**.
    > 
    > Po vytvoření a nastavení **StoreIndexViewModel** objektu, bude předáno jako argument pro **zobrazení** metoda. To znamená, že zobrazit šablonu použije k vygenerování odpovědi HTML s ním tento objekt.
4. Nahraďte **Index** metoda následujícím kódem:

    (Code fragment kódu - *ASP.NET MVC 4 základy – metoda Ex5 StoreController Index*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

    > [!NOTE]
    > Pokud jste obeznámeni s C#, může předpokládat to pomocí **var** znamená, že **viewModel** proměnná je pozdní vazbu. Není správný – kompilátor jazyka C# používá odvození typu podle přiřadit proměnnou k určení, který **viewModel** je typu **StoreIndexViewModel**. Také pomocí kompilování místní **viewModel** jako proměnnou **StoreIndexViewModel** typ kontrolu kompilaci get a Visual Studio – podpora editor kódu.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Úloha 4 – vytvoření šablony zobrazení, která používá StoreIndexViewModel

V této úloze vytvoříte šablonu zobrazení, která se použije k zobrazení seznamu žánry objekt StoreIndexViewModel předán z řadiče.

1. Před vytvořením nové šablony zobrazení, Vytvořme projekt tak, aby **Přidat Dialog zobrazení** zná **StoreIndexViewModel** třídy. Sestavení projektu výběrem **sestavení** položky nabídky a pak **sestavení MvcMusicStore**.

    ![Sestavení projektu](aspnet-mvc-4-fundamentals/_static/image22.png "sestavení projektu")

    *Sestavení projektu*
2. Vytvořte novou šablonu zobrazení. Chcete-li provést, klikněte pravým tlačítkem uvnitř **Index** metoda a vyberte **přidat zobrazení**.

    ![Přidání zobrazení](aspnet-mvc-4-fundamentals/_static/image23.png "přidávání zobrazení")

    *Přidání zobrazení*
3. Protože **Přidat Dialog zobrazení** byl vyvolán z **StoreController**, přidá šablony zobrazení ve výchozím nastavení v **\Views\Store\Index.cshtml** souboru. Zkontrolujte **vytvoření důrazně zadali zobrazení** zaškrtávací políčko a potom vyberte **StoreIndexViewModel** jako **třída modelu**. Taky se ujistěte, že je modul zobrazení vybrané **Razor**. Klikněte na tlačítko **přidat**.

    ![Přidat Dialog zobrazení](aspnet-mvc-4-fundamentals/_static/image24.png "Přidat Dialog zobrazení")

    *Přidat Dialog zobrazení*

    **\Views\Store\Index.cshtml** je vytvořen a otevřít soubor šablony zobrazení. Podle informací uvedených na **přidat zobrazení** dialogové okno v posledním kroku, zobrazení, bude očekávat šablony **StoreIndexViewModel** instance jako data sloužící ke generování odpovědi HTML. Si všimnete, že šablona dědí `ViewPage<musicstore.viewmodels.storeindexviewmodel>` v jazyce C#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Úloha 5: aktualizace zobrazení šablony

V této úloze aktualizujte zobrazení šablony vytvořené v posledním úkolem načíst počet žánry a jejich názvy v rámci dané stránky.

> [!NOTE]
> Budete používat @ syntaxe (často označované jako &quot;code útržky&quot;) ke spouštění kódu v rámci šablony zobrazení.


1. V **Index.cshtml** v souboru **úložiště** složky, jeho kódu nahraďte následujícím kódem:


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > Jakmile dokončíte zadáním období za slovo **modelu**, Visual Studio Intellisense zobrazí seznam možných vlastnosti a metody, které lze vybírat.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Získávání modelu vlastnosti a metody pomocí sady Visual Studio IntelliSense*
    > 
    > **Modelu** vlastnost odkazy **StoreIndexViewModel** objekt, který řadičem předána do zobrazení šablony. To znamená, že máte přístup všechna data z řadiče předaný zobrazit šablonu prostřednictvím **modelu** vlastnost a naformátujte ho do odpovídající HTML odpověď v rámci šablony zobrazení.
    > 
    > Můžete vybrat jen **NumberOfGenres** vlastnost z Intellisense seznam místo zadání ho a pak se bude automatického dokončování je stisknutím klávesy **tabulátor**.
2. Smyčky přes seznamu genre v **StoreIndexViewModel** a vytvořit HTML  **&lt;ul&gt;**  seznamu pomocí **foreach** smyčky.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Stiskněte klávesu **F5** ke spuštění aplikace a Procházet **/úložiště**. Zobrazí se seznam žánry předaná **StoreIndexViewModel** objektu z **StoreController** do zobrazení šablony.

    ![Zobrazení s přehledem žánry](aspnet-mvc-4-fundamentals/_static/image26.png "zobrazení s přehledem žánry")

    *Zobrazení s přehledem žánry*
4. Zavřete prohlížeč.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Cvičení 6: Pomocí parametrů v zobrazení

Cvičení 3 jste zjistili, jak předat parametry do Kontroleru. V tomto cvičení se dozvíte, jak používat tyto parametry v šabloně zobrazení. K tomuto účelu můžete budou zavedeny nejprve do třídy modelu, které vám pomohou při správě logika dat a domény. Kromě toho se dozvíte, jak vytvořit odkazy na stránky v aplikaci ASP.NET MVC bez obav věcí, jako kódování cesty adresy URL.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Úloha 1 – přidání třídy modelu

Na rozdíl od ViewModels, které vytváří jenom k předání informací z řadiče zobrazení, jsou vytvořené třídy modelu obsahovat a spravovat data a domény logiku. V této úloze budete přidávat dvě třídy modelu představují tyto koncepty: **Genre** a **Album**.

1. Pokud už otevřený, spusťte **VS Express pro Web**
2. V **soubor** nabídce zvolte **otevřít projekt**. V dialogovém okně Otevřít projekt přejděte do **Source\Ex06 UsingParametersInView\Begin**, vyberte **Begin.sln** a klikněte na tlačítko **otevřete**. Alternativně můžete pokračovat s řešením jste získali po dokončení předchozím cvičení.

    1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
    2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
    3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

    > [!NOTE]
    > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
3. Přidat **Genre** třída modelu. Chcete-li to provést, klikněte pravým tlačítkem **modely** složky v **Průzkumníku řešení**, vyberte **přidat** a potom **nová položka** možnost. V části **kód**, vyberte **– třída** položky a název souboru *Genre.cs*, pak klikněte na tlačítko **přidat**.

    ![Přidání třídy](aspnet-mvc-4-fundamentals/_static/image27.png "přidání třídy")

    *Přidání nové položky*

    ![Přidání třídy modelu Genre](aspnet-mvc-4-fundamentals/_static/image28.png "přidejte třídu modelu Genre")

    *Přidání třídy modelu Genre*
4. Přidat **název** vlastnost k třídě Genre. Chcete-li to provést, přidejte následující kód:

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 Genre*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Stejný postup jako předtím, přidejte **Album** třídy. Chcete-li to provést, klikněte pravým tlačítkem **modely** složky v **Průzkumníku řešení**, vyberte **přidat** a potom **nová položka** možnost. V části **kód**, vyberte **– třída** položky a název souboru *Album.cs*, pak klikněte na tlačítko **přidat**.
6. Přidejte dvě vlastnosti do třídy alb: **Genre** a **název**. Chcete-li to provést, přidejte následující kód:

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 Album*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Úloha 2 – přidání StoreBrowseViewModel

A **StoreBrowseViewModel** se použije k zobrazení alb, které odpovídají vybrané Genre v této úloze. V této úloze, vytvoříte této třídy a pak přidejte dvě vlastnosti pro zpracování **Genre** a jeho **Album**je seznam.

1. Přidat **StoreBrowseViewModel** třídy. Chcete-li to provést, klikněte pravým tlačítkem **ViewModels** složky v **Průzkumníku řešení**, vyberte **přidat** a potom **nová položka** možnost. V části **kód**, vyberte **– třída** položky a název souboru *StoreBrowseViewModel.cs*, pak klikněte na tlačítko **přidat**.
2. Přidat odkaz na modely v **StoreBrowseViewModel** třídy. Chcete-li to provést, přidejte následující pomocí oboru názvů:

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 UsingModel*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Přidat dvě vlastnosti do **StoreBrowseViewModel** – třída: **Genre** a **alb**. Chcete-li to provést, přidejte následující kód:

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 ModelProperties*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

    > [!NOTE]
    > Co je **seznamu&lt;Album&gt;**  ?: používá tuto definici **seznamu&lt;T&gt;**  typu, kde **T** omezí typ, na které elementy tohoto **seznamu** patří, v takovém případě **Album** (nebo některého z jejich potomků).
    > 
    > Tato schopnost třídy a metody, které odložení specifikace jeden nebo více typů, dokud třída nebo metoda je deklarován a vytvoření instance kódem na straně klienta je funkce jazyka C# návrhu názvem **obecné typy**.
    > 
    > **Seznam&lt;T&gt;**  je obecný ekvivalent **ArrayList** zadejte a je k dispozici v **System.Collections.Generic** obor názvů. Jednou z výhod použití **obecné typy** je, že vzhledem k tomu, že je zadaný typ, není nutné provádět kontroly operací, jako je přetypování elementy do typu **Album** jako by se **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Úloha 3 – v nové ViewModel StoreController

V této úloze budete upravovat **StoreController**na **Procházet** a **podrobnosti** metody akce použití nového **StoreBrowseViewModel** .

1. Přidat odkaz na složku modely v **StoreController** třídy. Chcete-li to provést, rozbalte **řadiče** složky v **Průzkumníku řešení** a otevřete **StoreController** – třída. Přidejte následující kód:

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 UsingModelInController*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Nahraďte **Procházet** metoda akce se má použít **StoreViewBrowseController** třídy. Vytvoříte Genre a dvě nové objekty alb s fiktivní dat (v testovacím prostředí další Hands-on vám bude spotřebovávat reálná data z databáze). Chcete-li to provést, nahraďte **Procházet** metoda následujícím kódem:

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Nahraďte **podrobnosti** metoda akce se má použít **StoreViewBrowseController** třídy. Vytvoří nový **Album** objekt, který má být vrácen **zobrazení**. Chcete-li to provést, nahraďte **podrobnosti** metoda následujícím kódem:

    (Code fragment kódu - *Základy architektury ASP.NET MVC 4 - Ex6 DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Úloha 4 – přidání šablonu Procházet zobrazení

V této úloze budete přidávat **Procházet** zobrazení zobrazíte alb pro konkrétní Genre nalezen.

1. Před vytvořením nové šablony zobrazení, měli byste vytvořit projekt tak, aby **přidat zobrazení** dialogové okno, že zná **ViewModel** třídu se má použít. Sestavení projektu výběrem **sestavení** položky nabídky a pak **sestavení MvcMusicStore**.
2. Přidat **Procházet** zobrazení. Chcete-li to provést, klikněte pravým tlačítkem v **Procházet** metody akce **StoreController** a klikněte na tlačítko **přidat zobrazení**.
3. V **přidat zobrazení** dialogové okno pole, ověřte, zda je název zobrazení **Procházet**. Zkontrolujte **vytvořit zobrazení silného typu** zaškrtávací políčko a vyberte **StoreBrowseViewModel** z **třída modelu** rozevíracího seznamu. V ostatních polích nechte výchozí hodnoty. Pak klikněte na tlačítko **přidat**.

    ![Přidání zobrazení Procházet](aspnet-mvc-4-fundamentals/_static/image29.png "přidání Procházet zobrazení")

    *Přidání zobrazení procházení*
4. Změnit **Browse.cshtml** pro zobrazení informací Genre, přístup k **StoreBrowseViewModel** objekt, který je předán zobrazení šablony. K tomu, nahradí obsah následujícím kódem: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Úloha 5: spuštění aplikace

V této úloze budete testovat, **Procházet** metoda načte alb z **Procházet** metoda akce.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na   **/ukládání/Procházet? Genre = Disco** k ověření, že akce vrátí dvě alb.

    ![Procházení úložiště Disco alb](aspnet-mvc-4-fundamentals/_static/image30.png "procházení Disco alb úložiště")

    *Procházení Disco alb úložiště*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Úloha 6 - zobrazení informace o konkrétní Album

V této úloze budete implementovat **úložiště/podrobnosti** zobrazení zobrazíte informace o konkrétní alb. V tomto testovacím prostředí Hands-on všechno, co se zobrazí o alba je už obsažený v **zobrazení** šablony. Ano, místo vytvoření **StoreDetailsViewModel** třída, budete používat aktuální **StoreBrowseViewModel** předáním alba šablony.

1. Zavřete prohlížeč v případě potřeby se vraťte do okna Visual Studio. Přidejte nový **podrobnosti** zobrazit **StoreController**na **podrobnosti** metody akce. Chcete-li to provést, klikněte pravým tlačítkem **podrobnosti** metoda v **StoreController** třídu a klikněte na tlačítko **přidat zobrazení**.
2. V **přidat zobrazení** dialogové okno, ověřte, zda **název zobrazení** je **podrobnosti**. Zkontrolujte **vytvořit zobrazení silného typu** zaškrtávací políčko a vyberte **Album** z **třída modelu** rozevíracího seznamu. V ostatních polích nechte výchozí hodnoty. Pak klikněte na tlačítko **přidat**. Tato akce vytvoří a otevře **\Views\Store\Details.cshtml** souboru.

    ![Přidání zobrazení podrobností o](aspnet-mvc-4-fundamentals/_static/image31.png "přidávání zobrazení podrobností")

    *Přidání zobrazení podrobností*
3. Změnit **Details.cshtml** soubor k zobrazení informací na Album, přístup k **Album** objekt, který je předán zobrazení šablony. K tomu, nahradí obsah následujícím kódem: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Úloha 7 – spuštění aplikace

V této úloze budete testovat, **podrobnosti** zobrazení načte informace o alba z **podrobnosti akce** metoda.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí v **Domů** stránky. Změňte adresu URL na **/Store/Details/5** pro ověření informací o alba.

    ![Procházení podrobností alb](aspnet-mvc-4-fundamentals/_static/image32.png "procházení podrobností alb")

    *Procházení Album podrobností*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Úloha 8 – přidání odkazů mezi stránkami

V této úloze, přidáte pomocí odkazu v zobrazení úložiště tak, aby měl odkaz v každé Genre názvu na příslušné **/úložiště/Procházet** adresy URL. Tímto způsobem, když kliknete na Genre, například **Disco**, bude přejděte na **/úložiště/procházet? genre = Disco** adresy URL.

1. Zavřete prohlížeč v případě potřeby se vraťte do okna Visual Studio. Aktualizace **Index** stránku přidáte odkaz **Procházet** stránky. Chcete-li to provést, v **Průzkumníku řešení** rozbalte **zobrazení** složku, pak se **úložiště** složku a dvojím kliknutím **Index.cshtml** stránky.
2. Přidáte odkaz na zobrazení pro procházení označující genre vybrané. K tomuto účelu nahradit následující zvýrazněný kód v rámci  **&lt;li&gt;**  značky: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

    > [!NOTE]
    > jiná možnost by propojení přímo na stránku s kódem takto:
    > 
    > &lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;
    > 
    > I když tento přístup funguje, závisí na řetězci pevně zakódované. Pokud přejmenujete později Kontroleru, je nutné tento pokyn ručně změnit. Lepší alternativou je použít **pomocné rutiny HTML** metoda. ASP.NET MVC zahrnuje metodu pomocné rutiny HTML, která je k dispozici pro takové úlohy. **Html.ActionLink()** Pomocná metoda usnadňuje sestavení HTML  **&lt;&gt;**  odkazy, a ujistěte se, cest URL jsou správně kódovaná adresou URL.
    > 
    > Htlm.ActionLink má několik přetížení. V tomto cvičení budete používat ten, který přijímá tři parametry:
    > 
    > 1. Text odkazu, který se zobrazí název Genre
    > 2. Název akce kontroleru (**Procházet**)
    > 3. Hodnoty parametru, zadat jak název trasy (**Genre**) a hodnotu (**Genre název**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Úloha 9 – spuštění aplikace

V této úloze budete testovat, zobrazí se každý Genre s odkazem na příslušné **/úložiště/Procházet** adresy URL.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí na domovské stránce. Změňte adresu URL na **/úložiště** k ověření, že každý Genre odkazy na příslušné **/úložiště/Procházet** adresy URL.

    ![Procházení žánry s odkazy na stránce procházení](aspnet-mvc-4-fundamentals/_static/image33.png "procházení žánry s odkazy na stránce procházení")

    *Procházení žánry s odkazy na stránce procházení*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Úlohu 10 - pomocí dynamické ViewModel kolekce předávání hodnot

V této úloze se dozvíte jednoduchá ale účinná metoda předat hodnoty mezi Kontrolerem a zobrazení bez jakýchkoli změn v modelu. ASP.NET MVC 4 poskytuje kolekci &quot;ViewModel&quot;, což může být přiřazena na jakoukoli hodnotu dynamické a získat přístup k uvnitř řadiče a také zobrazení.

Teď použijete dynamické kolekce položek ViewBag předat seznam &quot; **Starred žánry** &quot; z řadiče zobrazení. Zobrazení Index úložiště bude přístup k **ViewModel** a zobrazení informací.

1. Zavřete prohlížeč v případě potřeby se vraťte do okna Visual Studio. Otevřete **StoreController.cs** a upravovat **Index** metodu pro vytvoření seznamu starred žánry do kolekce ViewModel:


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Můžete také použít syntaxi **ViewBag [&quot;Starred&quot;]** pro přístup k vlastnosti.
2. Na ikonu hvězdičky  **&quot;starred.png&quot;**  je součástí **Source\Assets\Images** složky tohoto testovacího prostředí. Chcete-li přidat ji do aplikace, přetáhněte obsah z **Průzkumníka Windows** okno na **Průzkumníku řešení** v aplikaci Visual Web Developer Express, jak je uvedeno níže:

    ![Přidání hvězdičkami bitovou kopii do řešení](aspnet-mvc-4-fundamentals/_static/image34.png "přidání hvězdičkami bitovou kopii do řešení")

    *Přidávání hvězdičkami bitové kopie do řešení*
3. Otevření zobrazení **Store/Index.cshtml** a změnit obsah. Budete ke čtení &quot;starred&quot; vlastnost **ViewBag** kolekce a požádejte, pokud aktuální genre název je v seznamu. V takovém případě se zobrazí genre odkaz přímo na ikonu hvězdičky.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Úloha 11 – spuštění aplikace

V této úloze budete testovat, označený hvězdičkou žánry zobrazit ikonu hvězdičky.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Projekt se spustí v **Domů** stránky. Změňte adresu URL na **/úložiště** ověřte, že každé vybrané genre má pečlivě štítku:

    ![Procházení žánry označený hvězdičkou elementy](aspnet-mvc-4-fundamentals/_static/image35.png "procházení žánry označený hvězdičkou elementy")

    *Procházení žánry označený hvězdičkou elementy*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Cvičení 7: Okruhu kolem nové šablony ASP.NET MVC 4

V tomto cvičení zaměříte vylepšení v šablonách projektu ASP.NET MVC 4, trvá podívejte se na nejdůležitější funkce nové šablony.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Úloha 1: Zkoumat šablony ASP.NET MVC 4 Internetové aplikace

1. Pokud už otevřený, spusťte **VS Express pro Web**
2. Vyberte **souboru | Nové | Projekt** příkazu nabídky. V **nový projekt** dialogovém okně, vyberte **Visual C# | Webové** šablony v levém podokně stromu a vyberte **webové aplikace ASP.NET MVC 4**. **Název** projektu *MusicStore* a aktualizovat **název řešení** k *začít*, pak vyberte umístění (nebo ponechte výchozí nastavení) a klikněte na tlačítko **OK** .

    ![Vytvoření nového projektu ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "vytvoření nového projektu ASP.NET MVC 4")

    *Vytvoření nového projektu ASP.NET MVC 4*
3. V **nový ASP.NET MVC 4 projekt** dialogovém okně, vyberte **Internetové aplikace** šablona projektu a klikněte na tlačítko **OK**. Všimněte si, že jste vybrali jako zobrazovací modul Razor nebo ASPX.

    ![Vytvoření nové aplikace ASP.NET MVC 4 Internet](aspnet-mvc-4-fundamentals/_static/image37.png "vytvoření nové aplikace ASP.NET MVC 4 Internetu")

    *Vytvoření nové aplikace ASP.NET MVC 4 Internetu*

    > [!NOTE]
    > Syntaxe Razor byla zavedena v architektuře ASP.NET MVC 3. Jeho cílem je minimalizovat počet znaků a stisknutí kláves požadované v souboru, povolení fast a kapaliny kódování pracovního postupu. Syntaxe Razor využívá existující C# / VB. (nebo jiné) znalosti jazyka a doručí syntaxi značek šablony, která umožňuje pracovním postupu vytváření Super HTML.
4. Stiskněte klávesu **F5** a spuštění řešení najdete v části obnoveného šablony. Můžete zkontrolovat na následující funkce:

    1. **Styl moderních šablony**

        Byla obnovena šablony, poskytuje další styly moderních vyhledávání.

        ![Šablony MVC ASP.NET 4 přepracován tak](aspnet-mvc-4-fundamentals/_static/image38.png "přepracován tak šablony ASP.NET MVC 4")

        *ASP.NET MVC 4 přepracován tak šablony*
    2. **Adaptivního vykreslování**

        Podívejte se na změnu velikosti okna prohlížeče a Všimněte si, jak rozložení stránky dynamicky přizpůsobení novou velikost okna. Tyto šablony pomocí technik adaptivního vykreslování stolní počítače a mobilní platformy bez nutnosti přizpůsobení řádně vykreslit.

        ![Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti](aspnet-mvc-4-fundamentals/_static/image39.png "šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti")

        *Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti*
5. Zavřete prohlížeč zastavení ladicího programu a vrátíte se k sadě Visual Studio.
6. Nyní budete moci zkoumat řešení a podívejte se na některé z nových funkcí, které jsou zavedené službou ASP.NET MVC 4 v šabloně projektů.

    ![ASP.NET MVC4-internet aplikace--šablona projektu](aspnet-mvc-4-fundamentals/_static/image40.png "šablona projektu ASP.NET MVC 4 Internetové aplikace")

    *Šablona projektu ASP.NET MVC 4 Internetové aplikace*

    1. **Kód jazyka HTML5**

        Procházet šablony zobrazení a zjistěte, nové značky motiv, který je třeba otevřít **About.cshtml** zobrazit v rámci **Domů** složky.

        ![Nové šablony, pomocí syntaxe Razor a HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "novou šablonu, pomocí syntaxe Razor a HTML5 značek")

        *Nové šablony, pomocí syntaxe Razor a HTML5 značek*
    2. **Zahrnuté knihoven jazyka JavaScript**

        1. **jQuery**: jQuery zjednodušuje procházení dokumentu HTML, zpracování událostí, animace a interakce Ajax.
        2. **jQuery UI**: Tato knihovna nabízí abstrakci pro nízké úrovně interakce a animace, pokročilé efekty a bylo pomůcky, nástavbou jQuery JavaScript Library.

            > [!NOTE]
            > Informace o jQuery a kalendáře jQuery UI najdete v [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).
        3. **Kódem KnockoutJS**: výchozí šablonu ASP.NET MVC 4 nyní zahrnuje **kódem KnockoutJS**, rozhraní MVVM JavaScript rozhraní, které umožňuje vytvářet bohaté a vysoce přizpůsobivém webových aplikací pomocí jazyka JavaScript a HTML. Jako v architektuře ASP.NET MVC 3, jQuery a knihovny uživatelského rozhraní jQuery jsou také zahrnuté v architektuře ASP.NET MVC 4.

            > [!NOTE]
            > Můžete získat další informace o knihovně kódem KnockOutJS v tento odkaz: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).
        4. **Modernizr**: tuto knihovnu pracuje automaticky, která váš web kompatibilní s starší prohlížeče, při použití jazyka HTML5 a CSS3 technologie.

            > [!NOTE]
            > Můžete získat další informace o knihovně Modernizr v tento odkaz: [http://www.modernizr.com/](http://www.modernizr.com/).
    3. **SimpleMembership součástí řešení**

        SimpleMembership jsou určeny jako náhrada za předchozí systému zprostředkovatele členství a Role technologie ASP.NET. Obsahuje mnoho nových funkcí, které bylo snazší pro vývojáře na zabezpečené webové stránky flexibilnější způsobem.

        Šablona Internet již má nastavit pár věcí integrovat SimpleMembership, například AccountController připravena k použití OAuthWebSecurity (pro registraci účtu OAuth, přihlášení, správu atd.) a zabezpečení webového.

        ![SimpleMembership součástí řešení](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership součástí řešení")

        *SimpleMembership součástí řešení*

        > [!NOTE]
        > Najít další informace o [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) na webu MSDN.

> [!NOTE]
> Kromě toho můžete nasadit tuto aplikaci do následující weby systému Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Provedením tohoto testovacího prostředí Hands-On jste se naučili základy ASP.NET MVC:

- Základní prvky aplikace MVC a jejich vzájemné interakce
- Jak vytvořit aplikaci ASP.NET MVC
- Jak přidávat a konfigurovat řadičům pracovat s parametry předána adresu URL a řetězce dotazu
- Postup přidání rozložení stránky předlohy nastavit šablonu pro běžné obsah HTML, šablony stylů k vylepšení vzhledu a chování a šablonu zobrazení na Zobrazit obsah HTML
- Postup použití vzoru ViewModel pro předávání vlastností šablony zobrazení pro zobrazení dynamické informace
- Jak používat parametry předané do řadičů v šabloně zobrazení
- Postup přidání odkazů na stránky v aplikaci ASP.NET MVC
- Jak přidat a používat dynamické vlastnosti v zobrazení
- Vylepšení v šablonách projektu ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze  **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; *Visual Studio Express 2012 pro Web se sadou Windows Azure SDK*&quot;.
2. Klikněte na **nyní nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.

    ![Nainstalovat Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "nainstalovat Visual Studio Express")

    *Nainstalovat Visual Studio Express*
4. Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Vyjádření souhlasu s podmínkami licence](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Vyjádření souhlasu s podmínkami licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Průběh instalace*
6. Po dokončení instalace, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.

    ![VS Express pro Web dlaždice](aspnet-mvc-4-fundamentals/_static/image47.png)

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

    ![Přihlaste se k portálu Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k Windows Azure Management Portal*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](aspnet-mvc-4-fundamentals/_static/image49.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **výpočetní** | **webu**. Potom vyberte **rychle vytvořit** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvoření webu**.

    > [!NOTE]
    > Web systému Windows Azure je hostitel pro spouštění v cloudu, ve kterém můžete řídit a spravovat webovou aplikaci. Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál. Postup pro nastavení databáze neobsahuje.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](aspnet-mvc-4-fundamentals/_static/image50.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** je vytvořena.
5. Po vytvoření webu klikněte na odkaz v části **URL** sloupce. Zkontrolujte, zda je funkční nový web.

    ![Procházení na nový web](aspnet-mvc-4-fundamentals/_static/image51.png "procházení na nový web")

    *Procházení na nový web*

    ![Webový server spuštěn](aspnet-mvc-4-fundamentals/_static/image52.png "webu systémem")

    *Spuštění webu*
6. Přejděte zpět na portál a klikněte na název webu v části **název** sloupec pro zobrazení stránky pro správu.

    ![Otevření stránky Správa webu](aspnet-mvc-4-fundamentals/_static/image53.png "otevření stránek správu webového serveru")

    *Otevření stránek správu webového serveru*
7. V **řídicí panel** v části **rychlého přehledu** klikněte na položku **stažení profilu publikování** odkaz.

    > [!NOTE]
    > *Profilu publikování* obsahuje všechny informace požadované pro publikování webové aplikace na web služby Windows Azure pro každou metodu povoleno publikace. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a řetězců databází, které jsou potřebné k připojení k a ověřování na základě těchto koncových bodů, pro které je metoda publikace povolena. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pro Web** a **sadu Microsoft Visual Studio 2012** podporu čtení publikační profily k automatické konfiguraci těchto programů pro publikování webové aplikace na weby služby Windows Azure.

    ![Na webu stažení profilu publikování](aspnet-mvc-4-fundamentals/_static/image54.png "stahování webové stránky profilu publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profil publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak používat tento soubor k publikování webové aplikace na weby systému Windows Azure ze sady Visual Studio.

    ![Ukládání souboru profilu publikování](aspnet-mvc-4-fundamentals/_static/image55.png "ukládání profilu publikování")

    *Ukládání souboru profilu publikování*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá systému SQL Server, databáze, budete muset vytvořit databázi SQL server. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá systém SQL Server může tuto úlohu přeskočit.

1. Budete potřebovat databázi SQL serveru pro ukládání databázi aplikace. Databáze SQL servery můžete zobrazit ze svého předplatného na portál Windows Azure Management **databází Sql** | **servery** | **serveru Řídicí panel**. Pokud nemáte server vytvořeno, můžete vytvořit jeden pomocí **přidat** tlačítka na panelu příkazů. Poznamenejte si **název serveru a adresa URL, správce přihlašovací jméno a heslo**, jako je použijete v další úkoly. Nevytvářejte databáze ještě, jak bude vytvořen v pozdější fázi.

    ![Řídicí panel serveru databáze SQL](aspnet-mvc-4-fundamentals/_static/image56.png "řídicího panelu serveru databáze SQL")

    *Řídicí panel serveru databáze SQL*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio, proto je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. To lze provést, klikněte na tlačítko **konfigurace**, vyberte IP adresu z **aktuální IP adresa klienta** a vkládání na **počáteční IP adresa** a **Koncová IP adresa** textová pole a kliknutím ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) tlačítko.

    ![Přidávání IP adresy klienta](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Přidávání IP adresy klienta*
3. Jednou **IP adresa klienta** je povolené IP adresy do seznamu, klikněte na **Uložit** potvrďte změny.

    ![Potvrzení změn](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Potvrzení změn*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu

1. Přejděte zpět na ASP.NET MVC 4 řešení. V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](aspnet-mvc-4-fundamentals/_static/image60.png "publikování aplikace")

    *Publikování webu*
2. Umožňuje naimportujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](aspnet-mvc-4-fundamentals/_static/image61.png "import profilu publikování")

    *Import profilu publikování*
3. Klikněte na tlačítko **ověření připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření je hotová, jakmile se zobrazí zelené zaškrtnutí zobrazí vedle tlačítko ověřit připojení.

    ![Ověření připojení](aspnet-mvc-4-fundamentals/_static/image62.png "ověřování připojení")

    *Ověření připojení*
4. V **nastavení** v části **databáze** části, klikněte na tlačítko vedle připojení databáze textové pole (tj. **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](aspnet-mvc-4-fundamentals/_static/image63.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Připojení k databázi nakonfigurujte následujícím způsobem:

    - V **název serveru** zadejte vaše databáze SQL serveru adresu URL pomocí *tcp:* předponu.
    - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
    - V **heslo** zadejte přihlašovací heslo správce serveru.
    - Zadejte nový název databáze, například: *MVC4SampleDB*.

    ![Konfigurace cílový připojovací řetězec](aspnet-mvc-4-fundamentals/_static/image64.png "konfigurace cílový připojovací řetězec")

    *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze, klikněte na tlačítko **Ano**.

    ![Vytvoření databáze](aspnet-mvc-4-fundamentals/_static/image65.png "vytváření řetězec databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL v systému Windows Azure je uvedené v rámci textbox výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na databázi SQL](aspnet-mvc-4-fundamentals/_static/image66.png "připojovací řetězec odkazující na databázi SQL")

    *Připojovací řetězec odkazující na databázi SQL*
8. V **Preview** klikněte na tlačítko **publikovat**.

    ![Publikování webové aplikace](aspnet-mvc-4-fundamentals/_static/image67.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Jakmile proces publikování dokončí, otevře se výchozí prohlížeč publikované webové stránky.

    ![Aplikace publikována do služby Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "aplikace publikována do služby Windows Azure")

    *Aplikace, které jsou publikovány do služby Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Příloha C: používání fragmentů kódu

S fragmenty kódu máte všechny kód, který je nutné na dosah ruky. Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-fundamentals/_static/image69.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")

*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.
4. Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).
5. Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.

![Začněte psát název fragmentu](aspnet-mvc-4-fundamentals/_static/image70.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](aspnet-mvc-4-fundamentals/_static/image71.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")

*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*

![Stisknutím klávesy Tab znovu a fragmentu rozšíří](aspnet-mvc-4-fundamentals/_static/image72.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")

*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.
2. Vyberte relevantní fragment kódu ze seznamu, kliknutím na.

![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-fundamentals/_static/image73.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-fundamentals/_static/image74.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")

*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*
