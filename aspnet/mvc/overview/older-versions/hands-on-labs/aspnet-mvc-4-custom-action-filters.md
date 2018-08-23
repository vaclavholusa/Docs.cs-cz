---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Vlastní akce filtrech rozhraní ASP.NET MVC 4 | Dokumentace Microsoftu
author: rick-anderson
description: ASP.NET MVC poskytuje filtry akcí pro provádění logiku filtrování před nebo po zavolání metody akce. Filtry akce jsou vlastní atributy tha...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 0170fda6849c1dfb53b44908ea55ba2cad0dd067
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755039"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>ASP.NET MVC 4 – filtr vlastních akcí

podle [Campy Web týmu](https://twitter.com/webcamps)

[Stáhněte si Web Campy školení Kit](https://aka.ms/webcamps-training-kit)

ASP.NET MVC poskytuje filtry akcí pro provádění logiku filtrování před nebo po zavolání metody akce. Filtry akce jsou vlastní atributy, které poskytují deklarativní způsob, jak přidat akci před a po akci chování metody akce kontroleru.

Ve tohoto praktického testovacího prostředí vytvoříte vlastní akci atribut filtru do řešení MvcMusicStore zachytit kontroleru požadavků a protokolování aktivity webu do databázové tabulky. Budete moct přidat filtr protokolování injektáží kontroler nebo akce. A konečně zobrazí se zobrazení protokolu, který zobrazuje seznam návštěvníků.

Tohoto praktického testovacího prostředí předpokládá, že máte základní znalosti o **ASP.NET MVC**. Pokud jste ještě nepoužívali **ASP.NET MVC** před, doporučujeme si projít **základy ASP.NET MVC 4** praktického testovacího prostředí.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [ASP.NET MVC 4 vlastní akce filtry](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto praktického testovacího prostředí se dozvíte, jak:

- Vytvoření vlastní akce atribut filtru rozšířit možnosti filtrování
- Použít vlastní atribut filtru injektáží konkrétní úroveň
- Registrace, který globálně – filtr vlastních akcí

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

1. [Cvičení 1: Protokolování akcí](#Exercise1)
2. [Cvičení 2: Správa více filtrů Akce](#Exercise2)

Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**.

> [!NOTE]
> Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Cvičení 1: Protokolování akcí

V tomto cvičení se dozvíte, jak vytvořit filtr protokolu vlastní akci s použitím technologie ASP.NET MVC 4 filtr zprostředkovatelů. K tomuto účelu použijete filtr přihlašování k lokalitě MusicStore, zaznamená všechny aktivity v seznamu vybraných zařízení.

Filtr se rozšíří **ActionFilterAttributeClass** a přepsat **OnActionExecuting** metoda zachytit každého požadavku a provádět protokolování. Kontextové informace o požadavcích HTTP, provedení metody, výsledky a parametry budou poskytovat ASP.NET MVC **ActionExecutingContext** třídy **.**

> [!NOTE]
> ASP.NET MVC 4 obsahuje také výchozí filtry poskytovatele, které lze použít bez vytvoření vlastního filtru. ASP.NET MVC 4 poskytuje následující typy filtrů:
> 
> - **Autorizace** filtru, který představuje bezpečnostní rozhodnutí o provedení metody akce, jako je například ověřování nebo ověřování vlastnosti požadavku.
> - **Akce** filtru, která zabalí provádění metody akce. Tento filtr můžete provést další zpracování, jako jsou například poskytuje doplňující data na metodu akce, kontrola vrácené hodnoty nebo zrušil provádění metody akce
> - **Výsledek** filtru, která zabalí provádění objektu ActionResult. Tento filtr můžete provést další zpracování výsledku, jako je třeba změna odpověď HTTP.
> - **Výjimka** filtr, který se spustí, pokud je neošetřená výjimka vyvolaná někde v metodě akce, počínaje filtry autorizace a konče spuštění výsledku. Filtry výjimek je možné pro úlohy, jako je protokolování nebo zobrazení chybové stránky.
> 
> Další informace o poskytovatelích filtry najdete v tomto odkazu MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Informace o funkci protokolování aplikace MVC Music Store

Toto řešení Music Store má nové tabulky datového modelu pro protokolování serveru **ActionLog**, u následujících polí: název kontroleru, který obdržel požadavek, volá akci, IP adresa klienta a časové razítko.

![Datový model. Tabulka ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Datového modelu. ActionLog tabulky.")

*Datový model - ActionLog tabulky*

Řešení poskytuje zobrazení ASP.NET MVC do protokolu akcí, které lze nalézt v **MvcMusicStores/zobrazení/ActionLog**:

![Zobrazení protokolu akce](aspnet-mvc-4-custom-action-filters/_static/image2.png "zobrazit protokol akcí")

*Zobrazení protokolu akcí*

S tímto zadané struktury bude se veškerá práce, zaměřuje na by bylo třeba přerušit kontrolor žádosti a provedení protokolování pomocí vlastní filtrování.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Úloha 1 – Vytvoření vlastního filtru pro zachycení požadavek Kontroleru

V této úloze vytvoříte třídu atributu vlastní filtr, který bude obsahovat logiky protokolování. K tomuto účelu rozšíříte ASP.NET MVC **ActionFilterAttribute** třídy a implementovat rozhraní **IActionFilter**.

> [!NOTE]
> **ActionFilterAttribute** je základní třídou pro všechny filtry atribut. Poskytuje následující metody k provedení konkrétní logiky po a před spuštěním akce kontroleru:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): těsně před akce je volána metoda.
> - **OnActionExecuted**(ActionExecutedContext filterContext): po zavolání metody akce a před provedením výsledku (před vykreslením zobrazení).
> - **OnResultExecuting**(ResultExecutingContext filterContext): těsně před je výsledek spuštěn (před vykreslením zobrazení).
> - **OnResultExecuted**(ResultExecutedContext filterContext): Po provedení výsledku (po je zobrazení vykresleno).
> 
> Tak, že přepíšete některé z těchto metod na odvozenou třídu, můžete provést filtrování kódu.


1. Otevřít **začít** řešení nachází v **\Source\Ex01-LoggingActions\Begin** složky.

   1. Je potřeba předtím, než budete pokračovat, stáhněte si některé chybějící balíčky NuGet. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
      > 
      > Další informace najdete v tomto článku: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Přidání nové třídy C# do **filtry** složku a pojmenujte ho *CustomActionFilter.cs*. Tato složka uloží všechny vlastní filtry.
3. Otevřít **CustomActionFilter.cs** a přidejte odkaz na **System.Web.Mvc** a **MvcMusicStore.Models** oborů názvů:

    (Fragment - kódu *filtry vlastní akce architektury ASP.NET MVC 4 – Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Dědí **CustomActionFilter** třídy z **ActionFilterAttribute** a pak proveďte **CustomActionFilter** implementaci třídy **IActionFilter** rozhraní.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Ujistěte se, **CustomActionFilter** třídě přepsat metodu **OnActionExecuting** a přidat logiku potřebnou k protokolu spuštění filtru. Chcete-li to provést, přidejte následující zvýrazněný kód v rámci **CustomActionFilter** třídy.

    (Fragment - kódu *filtry vlastní akce architektury ASP.NET MVC 4 – Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** pomocí metody **Entity Framework** přidáte nový ActionLog registru. Vytvoří a vyplní novou instanci entity kontextové informace z **filterContext**.
    > 
    > Další informace o **parametrem ControllerContext** třídy v [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Úloha 2 – injektáž kódu zachycování do třídy Kontroleru Store

V této úloze přidáte vlastní filtr vložením na všechny třídy kontroleru a akce kontroleru, které budou protokolovány. Pro účely tohoto cvičení bude mít třídy Kontroleru Store protokolu.

Metoda **OnActionExecuting** z **ActionLogFilterAttribute** vlastní filtr se spouští při volání vloženého elementu.

Je také možné zachytit metodu konkrétní ovladač.

1. Otevřít **StoreController** na **MvcMusicStore\Controllers** a přidejte odkaz na **filtry** obor názvů:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Vložit vlastní filtr **CustomActionFilter** do **StoreController** třídy tak, že přidáte **[CustomActionFilter]** atribut před deklaraci třídy.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Když filtr se vloží do třídy kontroleru, jsou také vloží všechny jeho akce. Pokud chcete použít filtr jenom pro sadu akcí, je třeba vložit **[CustomActionFilter]** na každém z nich:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Úloha 3 – spuštění aplikace

V této úloze budete testovat, že filtr přihlašování funguje. Bude spusťte aplikaci a přejděte úložiště, a poté zkontroluje protokolu aktivit.

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Přejděte do **/ActionLog** zobrazíte počáteční stav zobrazení protokolu:

    ![Sledování stavu před aktivity stránku protokolu](aspnet-mvc-4-custom-action-filters/_static/image3.png "sledování stavu před aktivity stránku protokolu")

    *Protokolu sledování stavu před aktivity stránku*

   > [!NOTE]
   > Ve výchozím nastavení se vždy zobrazí jednu položku, který je generován při získávání existujících žánry nabídky.
   > 
   > Pro účely jednoduchosti čistíme **ActionLog** tabulky pokaždé, když je aplikace spuštěná, zobrazí pouze protokoly ověřování každý konkrétní úkol.
   > 
   > Možná budete muset odebrat následující kód z **relace\_Start** – metoda (v **Global.asax** třídy), aby bylo možné uložit Historický protokol pro všechny akce provést v rámci Store Kontroler.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Klikněte na některou **žánry** z nabídky a provést některé akce, třeba k procházení alba k dispozici.
4. Přejděte do **/ActionLog** a pokud je protokol prázdný stiskněte **F5** aktualizovat stránku. Zkontrolujte, že byly sledovány vaší návštěvy:

    ![Protokol akcí s aktivitou protokoluje](aspnet-mvc-4-custom-action-filters/_static/image4.png "protokol akcí s aktivitou přihlášení")

    *Protokol akcí s aktivita přihlášení*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Cvičení 2: Správa více filtrů Akce

V tomto cvičení přidáte druhý filtr vlastních akcí na třídu StoreController a definovat konkrétní pořadí, ve kterém se spustí i filtry. Pak aktualizujete kód pro registraci filtr globálně.

Vezměte v úvahu při definování pořadí spuštění filtrů se různými způsoby. Například vlastnost pořadí a oboru filtrů:

Můžete definovat **oboru** pro každý z těchto filtrů, třeba rozsahu všechny filtry akce ke spuštění v rámci **řadič oboru**a všechny filtry autorizace pro spuštění v **globálního rozsahu** . Obory mít definované provádění pořadí.

Kromě toho má každý filtr akce pořadí vlastnost, která se používá k určení pořadí spouštění v oboru filtru.

Další informace o pořadí spuštění filtrů vlastní akce, najdete v článku na webu MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Úloha 1: Vytvoření nové filtr vlastních akcí

V této úloze vytvoříte nový filtr vlastních akcí vložit do třídy StoreController učit, jak spravovat pořadí spuštění filtrů.

1. Otevřít **začít** řešení nachází v **\Source\Ex02-ManagingMultipleActionFilters\Begin** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

    1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
    2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
    3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

        > [!NOTE]
        > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
        > 
        > Další informace najdete v tomto článku: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Přidání nové třídy C# do **filtry** složku a pojmenujte ho *MyNewCustomActionFilter.cs*
3. Otevřít **MyNewCustomActionFilter.cs** a přidejte odkaz na **System.Web.Mvc** a **MvcMusicStore.Models** obor názvů:

    (Fragment - kódu *filtry vlastní akce architektury ASP.NET MVC 4 – Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Deklarace třídy výchozí nahraďte následujícím kódem.

    (Fragment - kódu *filtry vlastní akce architektury ASP.NET MVC 4 – Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Tento filtr vlastních akcí je téměř stejný než ta, kterou jste vytvořili v předchozím cvičení. Hlavní rozdíl je, že na něm *&quot;protokolovány podle&quot;* atribut aktualizovat tato nová třída název pro identifikaci začínajícího filtr registrován v protokolu.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Úloha 2: Vkládá nový sběrač kód do třídy StoreController

V této úloze se přidat nový vlastní filtr do třídy StoreController a spuštění řešení k ověření, jak oba filtry spolupracují.

1. Otevřít **StoreController** třídy umístění **MvcMusicStore\Controllers** a vložit nový vlastní filtr **MyNewCustomActionFilter** do  **StoreController** třídy, jako je znázorněno v následujícím kódu.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Dál spuštěním aplikace, abyste zjistili, jak tyto dva vlastní filtry akce fungují. Chcete-li to provést, stiskněte **F5** a počkejte na spuštění aplikace.
3. Přejděte do **/ActionLog** zobrazíte počáteční stav zobrazení protokolu.

    ![Sledování stavu před aktivity stránku protokolu](aspnet-mvc-4-custom-action-filters/_static/image5.png "sledování stavu před aktivity stránku protokolu")

    *Protokolu sledování stavu před aktivity stránku*
4. Klikněte na některou **žánry** z nabídky a provést některé akce, třeba k procházení alba k dispozici.
5. Zkontrolujte, že tento čas; vaší návštěvy byly sledovány dvakrát: jednou pro každý vlastní filtry akce, který jste přidali v kroku **kontroler úložiště** třídy.

    ![Protokol akcí s aktivitou protokoluje](aspnet-mvc-4-custom-action-filters/_static/image6.png "protokol akcí s aktivitou přihlášení")

    *Protokol akcí s aktivita přihlášení*
6. Zavřete prohlížeč.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Úloha 3: Správa řazení filtrů

V této úloze se dozvíte, jak spravovat pořadí spuštění filtrů se s použitím určeno pořadí.

1. Otevřít **StoreController** třídy nachází v **MvcMusicStore\Controllers** a zadejte **pořadí** vlastnost v obou filtrů, jako jsou uvedené dole.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Ověřte nyní, jak filtry jsou provedeny v závislosti na hodnotě vlastnosti jeho pořadí. Zjistíte, že filtr s nejmenší hodnotou pořadí (**CustomActionFilter**) je první z nich, který se spouští. Stisknutím klávesy **F5** a počkejte na spuštění aplikace.
3. Přejděte do **/ActionLog** zobrazíte počáteční stav zobrazení protokolu.

    ![Sledování stavu před aktivity stránku protokolu](aspnet-mvc-4-custom-action-filters/_static/image7.png "sledování stavu před aktivity stránku protokolu")

    *Protokolu sledování stavu před aktivity stránku*
4. Klikněte na některou **žánry** z nabídky a provést některé akce, třeba k procházení alba k dispozici.
5. Zkontrolujte, že byly sledovány tentokrát vaší návštěvy seřazené podle hodnoty pořadí filtrů.: **CustomActionFilter** protokoly první.

    ![Protokol akcí s aktivitou protokoluje](aspnet-mvc-4-custom-action-filters/_static/image8.png "protokol akcí s aktivitou přihlášení")

    *Protokol akcí s aktivita přihlášení*
6. Nyní aktualizujte hodnotu pořadí filtrů a ověření, jak se mění pořadí protokolování. V **StoreController** třídy, aktualizujte hodnotu pořadí filtrů jako vidíte níže.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Znovu spusťte aplikaci stisknutím klávesy **F5**.
8. Klikněte na některou **žánry** z nabídky a provést některé akce, třeba k procházení alba k dispozici.
9. Zkontrolujte, že současné době protokoly vytvořené **MyNewCustomActionFilter** filtru se zobrazí jako první.

    ![Protokol akcí s aktivitou protokoluje](aspnet-mvc-4-custom-action-filters/_static/image9.png "protokol akcí s aktivitou přihlášení")

    *Protokol akcí s aktivita přihlášení*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Úloha 4: Registrace globálně filtry.

V této úloze budete aktualizovat řešení k registraci nového filtru (**MyNewCustomActionFilter**) jako globální filtr. Tímto způsobem se aktivuje ve všech akce základě nastaveného v aplikaci a ne jenom v StoreController ty stejně jako v předchozí úloze.

1. V **StoreController** třídy, odeberte **[MyNewCustomActionFilter]** atribut a vlastnosti prostředí z **[CustomActionFilter]**. By měl vypadat nějak takto:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Otevřít **Global.asax** souboru a vyhledejte **aplikace\_Start** metody. Všimněte si, že pokaždé, když spuštění aplikace se registruje globální filtry voláním **RegisterGlobalFilters** metody v rámci **FilterConfig** třídy.

    ![Registrace globální filtry v souboru Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "registrace globální filtry v souboru Global.asax")

    *Registrace v souboru Global.asax globálních filtrů*
3. Otevřít **FilterConfig.cs** souborů v rámci **aplikace\_Start** složky.
4. Přidejte odkaz na použití System.Web.Mvc; pomocí MvcMusicStore.Filters; obor názvů.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Aktualizace **RegisterGlobalFilters** metoda přidání vlastního filtru. Chcete-li to provést, přidejte zvýrazněný kód:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Spusťte aplikaci stisknutím klávesy **F5**.
7. Klikněte na některou **žánry** z nabídky a provést některé akce, třeba k procházení alba k dispozici.
8. Kontrola, která teď **[MyNewCustomActionFilter]** se se vloží v HomeController a ActionLogController příliš.

    ![Protokol akcí s aktivitou protokoluje](aspnet-mvc-4-custom-action-filters/_static/image11.png "protokol akcí s aktivitou přihlášení")

    *Protokol akcí s globální aktivita přihlášení*

> [!NOTE]
> Kromě toho můžete nasadit tuto aplikaci následující weby Windows Azure [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Po dokončení tohoto praktického testovacího prostředí jste se dozvěděli, jak rozšířit filtru akce k provedení vlastní akce. Můžete také se naučili, jak vložit libovolný filtr k vašim řadičům stránky. Byly použity následující pojmy:

- Vytvoření vlastní akce filtry k třídě ASP.NET MVC ActionFilterAttribute
- Jak vložit filtry do kontrolery ASP.NET MVC
- Správa pomocí vlastnosti pořadí řazení filtrů
- Postup při registraci filtry globálně

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.

    ![Instalace sady Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "instalace sady Visual Studio Express")

    *Instalace sady Visual Studio Express*
4. Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Přijetí podmínek licence](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Přijetí podmínek licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Průběh instalace*
6. Až instalace skončí, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.

    ![VS Express for Web dlaždice](aspnet-mvc-4-custom-action-filters/_static/image16.png)

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

    ![Přihlaste se k portálu Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k portálu pro správu Azure Windows*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](aspnet-mvc-4-custom-action-filters/_static/image18.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **Compute** | **webu**. Potom vyberte **rychlé vytvoření** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvořit web**.

    > [!NOTE]
    > Hostitel pro webovou aplikaci spuštěnou v cloudu, který může řídit a spravovat je web Windows Azure. Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál. Nezahrnuje kroky pro vytvoření databáze.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](aspnet-mvc-4-custom-action-filters/_static/image19.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** se vytvoří.
5. Po vytvoření webové stránky, klikněte na odkaz v části **URL** sloupce. Zkontrolujte, jestli funguje nový web.

    ![Na nový web](aspnet-mvc-4-custom-action-filters/_static/image20.png "přechodu na nový web")

    *Procházení na nový web*

    ![Spuštění webu](aspnet-mvc-4-custom-action-filters/_static/image21.png "spuštění webové stránky")

    *Spuštění webové stránky*
6. Přejděte zpět na portál a klikněte na název webové stránky v části **název** sloupec, který se zobrazí na stránkách správy.

    ![Otevřete správu webových stránek](aspnet-mvc-4-custom-action-filters/_static/image22.png "otevřete správu webových stránek")

    *Otevřete správu webových stránek*
7. V **řídicí panel** stránce v části **rychlý přehled** klikněte na tlačítko **stáhnout profil publikování** odkaz.

    > [!NOTE]
    > *Profil publikování* obsahuje všechny informace požadované pro publikování webových aplikací na webu Windows Azure pro každou metodu povoleno publikování. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a databázové řetězce požadované k připojení a ověřování na základě jednotlivých koncových bodů, u kterých je povolena metoda publikace. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** a **Microsoft Visual Studio 2012** podporují čtení publikační profily k automatizaci konfigurace z těchto programů pro publikování webových aplikací na Windows Azure websites.

    ![Stahování webové stránky publikovat profil](aspnet-mvc-4-custom-action-filters/_static/image23.png "stahování webové stránky profil publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profilu publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak publikovat webovou aplikaci na weby Windows Azure ze sady Visual Studio pomocí tohoto souboru.

    ![Ukládání souboru profilu publikování](aspnet-mvc-4-custom-action-filters/_static/image24.png "ukládá se profil publikování")

    *Ukládá se profil publikování*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá SQL Server databáze, budete muset vytvořit server služby SQL Database. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá SQL Server může tuto úlohu přeskočit.

1. Pro uložení databáze aplikace budete potřebovat databázi SQL serveru. Servery SQL Database můžete zobrazit ze svého předplatného na portálu správy Windows Azure na **databází Sql** | **servery** | **serveru Řídicí panel**. Pokud nemáte server vytvořili, můžete vytvořit jednu **přidat** tlačítko na panelu příkazů. Poznamenejte si **název serveru a adresu URL, správce přihlašovací jméno a heslo**, jak je použijete v další úkoly. Nevytvářet databáze, protože vytvoří se v pozdější fázi.

    ![Řídicí panel serveru SQL Database](aspnet-mvc-4-custom-action-filters/_static/image25.png "řídicího panelu serveru SQL Database")

    *Řídicí panel serveru SQL Database*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio z tohoto důvodu je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. Chcete-li to mohli udělat, klikněte na tlačítko **konfigurovat**, vyberte IP adresu z **aktuální IP adresa klienta** a vložte ho na **počáteční IP adresa** a **Koncová IP adresa** textová pole a klikněte na tlačítko ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) tlačítko.

    ![Přidat IP adresu klienta](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Přidat IP adresu klienta*
3. Jednou **IP adresa klienta** je přidat do povolených IP adres klikněte na tlačítko na **Uložit** potvrďte provedené změny.

    ![Potvrzení změn](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Potvrzení změn*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nasazení webu

1. Vraťte se do řešení ASP.NET MVC 4. V **Průzkumníka řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](aspnet-mvc-4-custom-action-filters/_static/image29.png "publikování aplikace")

    *Publikování na webu*
2. Importujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](aspnet-mvc-4-custom-action-filters/_static/image30.png "import profilu publikování")

    *Import publikačního profilu*
3. Klikněte na tlačítko **ověřit připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření bylo dokončeno, jakmile se zobrazí zelené zaškrtnutí vedle tlačítka ověřit připojení.

    ![Ověřuje se připojení](aspnet-mvc-4-custom-action-filters/_static/image31.png "ověřuje se připojení")

    *Ověřuje se připojení*
4. V **nastavení** stránce v části **databází** klikněte na tlačítko vedle textového pole připojení k databázi (to znamená **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](aspnet-mvc-4-custom-action-filters/_static/image32.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Konfigurace připojení k databázi následujícím způsobem:

   - V **název serveru** zadejte vaše pomocí adresy URL databáze SQL serveru *tcp:* předponu.
   - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
   - V **heslo** zadejte heslo pro přihlašovací jméno správce serveru.
   - Zadejte nový název databáze.

     ![Konfigurace cílový připojovací řetězec](aspnet-mvc-4-custom-action-filters/_static/image33.png "konfigurace cílový připojovací řetězec")

     *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze klikněte na **Ano**.

    ![Vytvoření databáze](aspnet-mvc-4-custom-action-filters/_static/image34.png "vytvoření řetězce databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL ve službě Windows Azure se zobrazí v textovém poli výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "připojovací řetězec odkazující na SQL Database")

    *Připojovací řetězec odkazující na SQL Database*
8. V **ve verzi Preview** klikněte na **publikovat**.

    ![Publikování webové aplikace](aspnet-mvc-4-custom-action-filters/_static/image36.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Až se proces publikování dokončí, otevře se váš výchozí prohlížeč publikovaného webu.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Příloha C: používání fragmentů kódu

Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky. Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-custom-action-filters/_static/image37.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")

*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.
4. Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).
5. Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.

![Začněte psát název fragmentu kódu](aspnet-mvc-4-custom-action-filters/_static/image38.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](aspnet-mvc-4-custom-action-filters/_static/image39.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")

*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*

![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](aspnet-mvc-4-custom-action-filters/_static/image40.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")

*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.
2. Kliknutím na vyberte relevantní fragment kódu ze seznamu.

![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-custom-action-filters/_static/image41.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-custom-action-filters/_static/image42.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")

*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*
