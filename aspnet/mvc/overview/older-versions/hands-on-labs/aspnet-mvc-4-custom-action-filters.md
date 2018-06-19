---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Vlastní akce filtrech rozhraní ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC poskytuje filtry akce pro provádění filtrování logiku před i po zavolání metody akce. Zadaná vlastní atributy jsou filtry akce...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 8b135b23aea64b0c7c7d4368eef9ee80914159e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877707"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Vlastní akce filtrech rozhraní ASP.NET MVC 4

Podle [webové táborech Team](https://twitter.com/webcamps)

[Stažení webové táborech cvičení Kit](https://aka.ms/webcamps-training-kit)

ASP.NET MVC poskytuje filtry akce pro provádění filtrování logiku před i po zavolání metody akce. Filtry akce jsou vlastní atributy, které poskytují deklarativní způsob, jak přidat chování akce před a po akce do metody akce kontroleru.

V tomto testovacím prostředí Hands-on vytvoříte vlastní akce atribut filtru do řešení MvcMusicStore catch požadavky řadiče a protokolovat činnost serveru do databázové tabulky. Bude moct přidat filtr protokolování injektáží do jakékoli kontroler nebo akce. Nakonec zobrazí se zobrazení protokolu, který zobrazuje seznam návštěvníky.

Toto testovací prostředí Hands-on předpokládá, že máte základní znalosti o **ASP.NET MVC**. Pokud jste nepoužili **ASP.NET MVC** před, doporučujeme si projít **ASP.NET MVC 4 Základy** Hands-on testovacího prostředí.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [verze Microsoft-webové/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 vlastní akce filtry](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí Hands-On se dozvíte, jak:

- Vytvořit vlastní akce atribut filtru rozšířit možnosti filtrování
- Použijte vlastní atribut filtru tak, že vkládání konkrétní úroveň
- Zaregistrovat globální filtry vlastní akce

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

Toto testovací prostředí Hands-On se skládá ve cvičeních následující:

1. [Cvičení 1: Protokolování akce](#Exercise1)
2. [Cvičení 2: Správa více filtrů Akce](#Exercise2)

Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**.

> [!NOTE]
> Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Cvičení 1: Protokolování akce

V tomto cvičení se dozvíte, jak vytvořit vlastní akce filtru protokolu s použitím technologie ASP.NET MVC 4 filtru zprostředkovatelů. K tomuto účelu použijete filtr protokolování MusicStore lokalitu, která bude zaznamenávat všechny aktivity v vybrané řadiče.

Filtr se rozšířit **ActionFilterAttributeClass** a přepsání **OnActionExecuting** metoda catch každého požadavku a provádět akce protokolování. Kontextové informace o požadavcích HTTP, provádění metody, výsledky a parametry budou poskytovat ASP.NET MVC **ActionExecutingContext** třída **.**

> [!NOTE]
> ASP.NET MVC 4 má také výchozí zprostředkovatele filtrů, které můžete použít bez vytvoření vlastního filtru. ASP.NET MVC 4 poskytuje následující typy filtrů:
> 
> - **Autorizace** filtrovat, takže zabezpečení rozhodnutí o tom, jestli spuštění metody akce, jako je například ověřování nebo ověřování vlastnosti požadavku.
> - **Akce** filtr, který zabalí provádění metody akce. Tento filtr můžete provádět další zpracování, např. poskytuje doplňující data na metody akce, kontrola návratovou hodnotu nebo zrušení provádění metody akce
> - **Výsledek** filtr, který zabalí provádění objektu ActionResult. Tento filtr můžete provést další zpracování výsledku, jako je například úprava odpověď HTTP.
> - **Výjimka** filtr, který provede, pokud dojde k neošetřené výjimce vyvolána někde v metodě akce, počínaje filtry autorizace a konče provádění výsledku. Filtry výjimek lze použít pro úlohy, jako je protokolování nebo zobrazení chybovou stránku.
> 
> Další informace o poskytovatelích filtry navštivte tento odkaz MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>O funkci protokolování aplikace MVC Hudba úložiště

Toto řešení úložiště Hudba má do nové tabulky datového modelu pro protokolování serveru **ActionLog**, s následující pole: název kontroleru, který přijal žádost, volané akce, IP adresa klienta a časové razítko.

![Datový model. Tabulka ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Datového modelu. Tabulka ActionLog.")

*Datový model - ActionLog tabulky*

Řešení poskytuje pro protokol akcí, který se nachází v zobrazení ASP.NET MVC **MvcMusicStores nebo zobrazení/ActionLog**:

![Akce protokolu zobrazení](aspnet-mvc-4-custom-action-filters/_static/image2.png "zobrazení protokolu akcí")

*Zobrazení protokolu akcí*

S tímto zadané struktura všechny práce se zaměří na přerušení řadiče požadavku a provedení protokolování pomocí vlastní filtrování.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Úloha 1 – Vytvoření vlastního filtru k zachycení žádosti řadiče

V této úloze vytvoříte třídu atributu vlastního filtru, která bude obsahovat logiku protokolování. K tomuto účelu rozšíříte ASP.NET MVC **ActionFilterAttribute** třídy a implementace rozhraní **IActionFilter**.

> [!NOTE]
> **ActionFilterAttribute** je základní třída pro všechny filtry atribut. Poskytuje následující metody, které spustit určitou logiku po a před spuštěním akce kontroleru:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): těsně před akci metoda je volána.
> - **OnActionExecuted**(ActionExecutedContext filterContext): po zavolání metody akce a před provedením výsledku (před vykreslením zobrazení).
> - **OnResultExecuting**(ResultExecutingContext filterContext): těsně před je výsledek spuštěn (před vykreslením zobrazení).
> - **OnResultExecuted**(ResultExecutedContext filterContext): Po provedení výsledku (po je zobrazení vykresleno).
> 
> Pomocí některé z těchto metod přepsání v odvozené třídě, můžete provést filtrování kódu.


1. Otevřete **začít** řešení nacházející se v **\Source\Ex01-LoggingActions\Begin** složky.

   1. Musíte se ke stažení některé chybějící balíčky NuGet, než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
      > 
      > Další informace najdete v tomto článku: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Přidejte novou C# třídu do **filtry** složku a pojmenujte ji *CustomActionFilter.cs*. Tato složka se uloží všechny vlastní filtry.
3. Otevřete **CustomActionFilter.cs** a přidejte odkaz na **System.Web.Mvc** a **MvcMusicStore.Models** oborů názvů:

    (Code fragment kódu - *vlastní akce filtrech rozhraní ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Zdědit **CustomActionFilter** třídy z **ActionFilterAttribute** a pak proveďte **CustomActionFilter** implementaci třídy **IActionFilter** rozhraní.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Ujistěte se, **CustomActionFilter** třída potlačení metody **OnActionExecuting** a přidejte pomocí potřebné logiky do protokolu jeho spuštění. K tomu, přidejte následující zvýrazněný kód v rámci **CustomActionFilter** třídy.

    (Code fragment kódu - *vlastní akce filtrech rozhraní ASP.NET MVC 4 - Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** používá metoda **Entity Framework** přidat nové ActionLog registrace. Vytvoří a vyplní novou instanci entity kontextové informace z **filterContext**.
    > 
    > Další informace o **parametrem ControllerContext** třídy v [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Úloha 2 – vložení kódu interceptoru do třídy Kontroleru úložiště

V této úloze přidáte vlastní filtr vložením pro všechny třídy kontroleru a akce kontroleru, které bude do protokolu. Pro účely tohoto cvičení bude mít třídy Kontroleru úložiště protokolu.

Metoda **OnActionExecuting** z **ActionLogFilterAttribute** vlastní filtr se spouští při volání vloženého elementu.

Je také možné zachytit metoda konkrétní řadič.

1. Otevřete **StoreController** v **MvcMusicStore\Controllers** a přidejte odkaz na **filtry** obor názvů:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Vložení vlastní filtr **CustomActionFilter** do **StoreController** třída přidáním **[CustomActionFilter]** atribut před deklaraci třídy.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Pokud je filtr vloženy do třídy kontroleru, jsou také vložit všechny jeho akce. Pokud chcete použít filtr pouze pro sadu akcí, budete muset vložit **[CustomActionFilter]** na každém z nich:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Úloha 3 – spuštění aplikace

V této úloze budete testovat, protokolování filtru funguje. Bude spustit aplikaci a navštívit obchod, a poté zkontroluje protokolu aktivit.

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Přejděte do **/ActionLog** zobrazíte počáteční stav zobrazení protokolu:

    ![Sledování stavu před stránky aktivity protokolu](aspnet-mvc-4-custom-action-filters/_static/image3.png "sledovací modul stavu před stránky aktivity protokolu")

    *Stav sledovací modul protokolu před stránky aktivity*

   > [!NOTE]
   > Ve výchozím nastavení je vždy zobrazí jednu položku, který se vygeneruje, když načítání existující žánry pro v nabídce.
   > 
   > Pro účely jednoduchost jsme čištění **ActionLog** tabulky pokaždé, když je aplikace spuštěná, zobrazí pouze protokoly ověřování každý konkrétní úkol.
   > 
   > Možná budete muset odebrat následující kód z **relace\_spustit** – metoda (v **Global.asax** třída), aby bylo možné uložit Historický protokol pro všechny akce provést v úložišti Řadiče.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Klikněte na jednu z **žánry** z nabídky a provádět některé akce, jako je k dispozici album procházení.
4. Přejděte do **/ActionLog** a pokud je protokol prázdný stiskněte **F5** obnovíte stránku. Zkontrolujte, že byly sledovány vaší návštěvy:

    ![Protokol akcí s aktivitou protokolována](aspnet-mvc-4-custom-action-filters/_static/image4.png "protokolu akcí s aktivitou přihlášení")

    *Protokol akcí s aktivitou přihlášení*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Cvičení 2: Správa více filtrů Akce

V tomto cvičení přidáte druhý filtr akce vlastní třídu StoreController a definovat konkrétní pořadí, ve kterém bude proveden oba filtry. Potom aktualizujte kód k registraci globální filtr.

Existují různé možnosti vzít v úvahu při definování pořadí zpracování pro filtry. Například vlastnost pořadí a oboru pro filtry:

Můžete definovat **oboru** pro každou z filtrů, například může oboru všechny filtry akce ke spuštění v rámci **řadiče oboru**a všechny filtry autorizace ke spuštění **globální obor** . Obory mít definovaný provádění pořadí.

Kromě toho každý filtr akce má vlastnost pořadí sloužící k určení pořadí spouštění v oboru filtru.

Další informace o pořadí spuštění filtrů Akce vlastní, navštivte tohoto článku na webu MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Úloha 1: Vytvoření nové vlastní filtr akce

V této úloze vytvoříte nový filtr vlastní akce se vložit do třídy StoreController naučit, jak spravovat pořadí spuštění filtrů.

1. Otevřete **začít** řešení nacházející se v **\Source\Ex02-ManagingMultipleActionFilters\Begin** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

    1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
    2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
    3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

        > [!NOTE]
        > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
        > 
        > Další informace najdete v tomto článku: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Přidejte novou C# třídu do **filtry** složku a pojmenujte ji *MyNewCustomActionFilter.cs*
3. Otevřete **MyNewCustomActionFilter.cs** a přidejte odkaz na **System.Web.Mvc** a **MvcMusicStore.Models** obor názvů:

    (Code fragment kódu - *vlastní akce filtrech rozhraní ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Deklarace třídy výchozí nahraďte následujícím kódem.

    (Code fragment kódu - *vlastní akce filtrech rozhraní ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Tato vlastní akce filtru je téměř stejný než ten, který jste vytvořili v předchozím cvičení. Hlavní rozdíl je, že je *&quot;přihlášení pomocí&quot;* aktualizováno tato nová třída název pro identifikaci požadovaly filtru atributu zaregistrován v protokolu.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Úloha 2: Vložení nové interceptoru kódu do StoreController – třída

V této úloze přidáte nový vlastní filtr do třídy StoreController a spuštění řešení Chcete-li ověřit, jak oba filtry spolupracují.

1. Otevřete **StoreController** třída nacházející se v **MvcMusicStore\Controllers** a vložit nový vlastní filtr **MyNewCustomActionFilter** do  **StoreController** třídy, jako je znázorněno v následujícím kódu.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Nyní spusťte aplikaci chcete-li zobrazit, jak tyto dva filtry akce vlastní fungují. Chcete-li to provést, stiskněte **F5** a počkejte, než aplikaci spustí.
3. Přejděte do **/ActionLog** zobrazíte počáteční stav zobrazení protokolu.

    ![Sledování stavu před stránky aktivity protokolu](aspnet-mvc-4-custom-action-filters/_static/image5.png "sledovací modul stavu před stránky aktivity protokolu")

    *Stav sledovací modul protokolu před stránky aktivity*
4. Klikněte na jednu z **žánry** z nabídky a provádět některé akce, jako je k dispozici album procházení.
5. Zkontrolujte, zda tento čas; vaše návštěvy byly sledovány dvakrát: jednou pro každou z vlastní filtry akce, který jste přidali v **StorageController** třídy.

    ![Protokol akcí s aktivitou protokolována](aspnet-mvc-4-custom-action-filters/_static/image6.png "protokolu akcí s aktivitou přihlášení")

    *Protokol akcí s aktivitou přihlášení*
6. Zavřete prohlížeč.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Úloha 3: Správa řazení filtrů

V této úloze se dozvíte, jak spravovat pořadí spuštění filtrů se pomocí určeno pořadí.

1. Otevřete **StoreController** třída nacházející se v **MvcMusicStore\Controllers** a zadejte **pořadí** vlastnost v obou filtrů, jako vidíte níže.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Teď zkontrolujte, jak filtry jsou spouštěny v závislosti na hodnotě jeho vlastnost pořadí. Bude zjistíte, že filtr s nejmenší hodnotou pořadí (**CustomActionFilter**) je první ten, který se spustí. Stiskněte klávesu **F5** a počkejte, než aplikaci spustí.
3. Přejděte do **/ActionLog** zobrazíte počáteční stav zobrazení protokolu.

    ![Sledování stavu před stránky aktivity protokolu](aspnet-mvc-4-custom-action-filters/_static/image7.png "sledovací modul stavu před stránky aktivity protokolu")

    *Stav sledovací modul protokolu před stránky aktivity*
4. Klikněte na jednu z **žánry** z nabídky a provádět některé akce, jako je k dispozici album procházení.
5. Zkontrolujte, že byly sledovány této doby vaší návštěvy seřazené podle hodnota pořadí pro filtry: **CustomActionFilter** protokoly se první.

    ![Protokol akcí s aktivitou protokolována](aspnet-mvc-4-custom-action-filters/_static/image8.png "protokolu akcí s aktivitou přihlášení")

    *Protokol akcí s aktivitou přihlášení*
6. Nyní provede aktualizaci hodnota pořadí pro filtry a ověřte, jak se změní pořadí protokolování. V **StoreController** třídy, aktualizaci hodnoty pořadí pro filtry jako vidíte níže.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Znovu spusťte aplikaci stisknutím **F5**.
8. Klikněte na jednu z **žánry** z nabídky a provádět některé akce, jako je k dispozici album procházení.
9. Zkontrolujte, že tentokrát protokoly vytvořené **MyNewCustomActionFilter** filtru objeví jako první.

    ![Protokol akcí s aktivitou protokolována](aspnet-mvc-4-custom-action-filters/_static/image9.png "protokolu akcí s aktivitou přihlášení")

    *Protokol akcí s aktivitou přihlášení*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Úloha 4: Registrace globální filtry.

V této úloze budete aktualizovat řešení zaregistrovat nový filtr (**MyNewCustomActionFilter**) jako globální filtr. Díky tomu se spustí ve všech perfomed akce v aplikaci a ne jenom v StoreController ty, které jsou jako v předchozí úloze.

1. V **StoreController** třídy, odeberte **[MyNewCustomActionFilter]** atribut a vlastnost pořadí z **[CustomActionFilter]**. By měl vypadat jako následující:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Otevřete **Global.asax** souborů a vyhledejte **aplikace\_spustit** metoda. Všimněte si, že pokaždé, když aplikace spustí se registrují globální filtry voláním **RegisterGlobalFilters** metoda v rámci **FilterConfig** třídy.

    ![Registrace globálních filtrů v souboru Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "registrace v souboru Global.asax globálních filtrů")

    *Registrace v souboru Global.asax globálních filtrů*
3. Otevřete **FilterConfig.cs** souboru v rámci **aplikace\_spustit** složky.
4. Přidat odkaz na pomocí System.Web.Mvc; pomocí MvcMusicStore.Filters; obor názvů.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Aktualizace **RegisterGlobalFilters** metoda přidání vlastního filtru. Chcete-li to provést, přidejte zvýrazněný kód:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Spusťte aplikaci stisknutím **F5**.
7. Klikněte na jednu z **žánry** z nabídky a provádět některé akce, jako je k dispozici album procházení.
8. Zkontrolujte, že nyní **[MyNewCustomActionFilter]** je právě v HomeController a ActionLogController vložit příliš.

    ![Protokol akcí s aktivitou protokolována](aspnet-mvc-4-custom-action-filters/_static/image11.png "protokolu akcí s aktivitou přihlášení")

    *Protokol akcí s globální aktivitou přihlášení*

> [!NOTE]
> Kromě toho můžete nasadit tuto aplikaci do následující weby systému Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Provedením tohoto testovacího prostředí Hands-On jste se naučili, jak rozšířit filtr akce k provedení vlastní akce. Naučili jste postup vložit libovolný filtr k vašim řadičům stránky. Následující koncepty se používaly:

- Jak vytvořit vlastní akce filtry pomocí třídy ASP.NET MVC ActionFilterAttribute
- Postup vložení filtry do řadiče ASP.NET MVC
- Jak spravovat pomocí vlastnosti pořadí řazení filtrů
- Postup registrace globální filtry

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nyní nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.

    ![Nainstalovat Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "nainstalovat Visual Studio Express")

    *Nainstalovat Visual Studio Express*
4. Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Vyjádření souhlasu s podmínkami licence](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Vyjádření souhlasu s podmínkami licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Průběh instalace*
6. Po dokončení instalace, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.

    ![VS Express pro Web dlaždice](aspnet-mvc-4-custom-action-filters/_static/image16.png)

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

    ![Přihlaste se k portálu Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k Windows Azure Management Portal*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](aspnet-mvc-4-custom-action-filters/_static/image18.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **výpočetní** | **webu**. Potom vyberte **rychle vytvořit** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvoření webu**.

    > [!NOTE]
    > Web systému Windows Azure je hostitel pro spouštění v cloudu, ve kterém můžete řídit a spravovat webovou aplikaci. Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál. Postup pro nastavení databáze neobsahuje.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](aspnet-mvc-4-custom-action-filters/_static/image19.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** je vytvořena.
5. Po vytvoření webu klikněte na odkaz v části **URL** sloupce. Zkontrolujte, zda je funkční nový web.

    ![Procházení na nový web](aspnet-mvc-4-custom-action-filters/_static/image20.png "procházení na nový web")

    *Procházení na nový web*

    ![Webový server spuštěn](aspnet-mvc-4-custom-action-filters/_static/image21.png "webu systémem")

    *Spuštění webu*
6. Přejděte zpět na portál a klikněte na název webu v části **název** sloupec pro zobrazení stránky pro správu.

    ![Otevření stránky Správa webu](aspnet-mvc-4-custom-action-filters/_static/image22.png "otevření stránek správu webového serveru")

    *Otevření stránek správu webového serveru*
7. V **řídicí panel** v části **rychlého přehledu** klikněte na položku **stažení profilu publikování** odkaz.

    > [!NOTE]
    > *Profilu publikování* obsahuje všechny informace požadované pro publikování webové aplikace na web služby Windows Azure pro každou metodu povoleno publikace. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a řetězců databází, které jsou potřebné k připojení k a ověřování na základě těchto koncových bodů, pro které je metoda publikace povolena. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pro Web** a **sadu Microsoft Visual Studio 2012** podporu čtení publikační profily k automatické konfiguraci těchto programů pro publikování webové aplikace na weby služby Windows Azure.

    ![Na webu stažení profilu publikování](aspnet-mvc-4-custom-action-filters/_static/image23.png "stahování webové stránky profilu publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profil publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak používat tento soubor k publikování webové aplikace na weby systému Windows Azure ze sady Visual Studio.

    ![Ukládání souboru profilu publikování](aspnet-mvc-4-custom-action-filters/_static/image24.png "ukládání profilu publikování")

    *Ukládání souboru profilu publikování*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá systému SQL Server, databáze, budete muset vytvořit databázi SQL server. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá systém SQL Server může tuto úlohu přeskočit.

1. Budete potřebovat databázi SQL serveru pro ukládání databázi aplikace. Databáze SQL servery můžete zobrazit ze svého předplatného na portál Windows Azure Management **databází Sql** | **servery** | **serveru Řídicí panel**. Pokud nemáte server vytvořeno, můžete vytvořit jeden pomocí **přidat** tlačítka na panelu příkazů. Poznamenejte si **název serveru a adresa URL, správce přihlašovací jméno a heslo**, jako je použijete v další úkoly. Nevytvářejte databáze ještě, jak bude vytvořen v pozdější fázi.

    ![Řídicí panel serveru databáze SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "řídicího panelu serveru databáze SQL")

    *Řídicí panel serveru databáze SQL*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio, proto je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. To lze provést, klikněte na tlačítko **konfigurace**, vyberte IP adresu z **aktuální IP adresa klienta** a vkládání na **počáteční IP adresa** a **Koncová IP adresa** textová pole a kliknutím ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) tlačítko.

    ![Přidávání IP adresy klienta](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Přidávání IP adresy klienta*
3. Jednou **IP adresa klienta** je povolené IP adresy do seznamu, klikněte na **Uložit** potvrďte změny.

    ![Potvrzení změn](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Potvrzení změn*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu

1. Přejděte zpět na ASP.NET MVC 4 řešení. V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](aspnet-mvc-4-custom-action-filters/_static/image29.png "publikování aplikace")

    *Publikování webu*
2. Umožňuje naimportujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](aspnet-mvc-4-custom-action-filters/_static/image30.png "import profilu publikování")

    *Import profilu publikování*
3. Klikněte na tlačítko **ověření připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření je hotová, jakmile se zobrazí zelené zaškrtnutí zobrazí vedle tlačítko ověřit připojení.

    ![Ověření připojení](aspnet-mvc-4-custom-action-filters/_static/image31.png "ověřování připojení")

    *Ověření připojení*
4. V **nastavení** v části **databáze** části, klikněte na tlačítko vedle připojení databáze textové pole (tj. **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](aspnet-mvc-4-custom-action-filters/_static/image32.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Připojení k databázi nakonfigurujte následujícím způsobem:

   - V **název serveru** zadejte vaše databáze SQL serveru adresu URL pomocí *tcp:* předponu.
   - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
   - V **heslo** zadejte přihlašovací heslo správce serveru.
   - Zadejte nový název databáze.

     ![Konfigurace cílový připojovací řetězec](aspnet-mvc-4-custom-action-filters/_static/image33.png "konfigurace cílový připojovací řetězec")

     *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze, klikněte na tlačítko **Ano**.

    ![Vytvoření databáze](aspnet-mvc-4-custom-action-filters/_static/image34.png "vytváření řetězec databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL v systému Windows Azure je uvedené v rámci textbox výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na databázi SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "připojovací řetězec odkazující na databázi SQL")

    *Připojovací řetězec odkazující na databázi SQL*
8. V **Preview** klikněte na tlačítko **publikovat**.

    ![Publikování webové aplikace](aspnet-mvc-4-custom-action-filters/_static/image36.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Jakmile proces publikování dokončí, otevře se výchozí prohlížeč publikované webové stránky.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Příloha C: používání fragmentů kódu

S fragmenty kódu máte všechny kód, který je nutné na dosah ruky. Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-custom-action-filters/_static/image37.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")

*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.
4. Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).
5. Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.

![Začněte psát název fragmentu](aspnet-mvc-4-custom-action-filters/_static/image38.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](aspnet-mvc-4-custom-action-filters/_static/image39.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")

*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*

![Stisknutím klávesy Tab znovu a fragmentu rozšíří](aspnet-mvc-4-custom-action-filters/_static/image40.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")

*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.
2. Vyberte relevantní fragment kódu ze seznamu, kliknutím na.

![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-custom-action-filters/_static/image41.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-custom-action-filters/_static/image42.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")

*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*
