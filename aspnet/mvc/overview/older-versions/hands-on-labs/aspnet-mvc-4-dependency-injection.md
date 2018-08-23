---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Injektáž závislostí architektury ASP.NET MVC 4 | Dokumentace Microsoftu
author: rick-anderson
description: 'Poznámka: Tohoto praktického testovacího prostředí se předpokládá, že máte základní znalosti ASP.NET MVC a ASP.NET MVC 4 filtry. Pokud jste nepoužili ASP.NET MVC 4 filtry před jsme rec...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3f9222c7b485f552da91f4875c882db7e03cdd0a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755017"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>Injektáž závislostí architektury ASP.NET MVC 4

podle [Campy Web týmu](https://twitter.com/webcamps)

[Stáhněte si Web Campy školení Kit](https://aka.ms/webcamps-training-kit)

Tohoto praktického testovacího prostředí předpokládá, že máte základní znalosti o **ASP.NET MVC** a **ASP.NET MVC 4 filtruje**. Pokud jste ještě nepoužívali **ASP.NET MVC 4 filtry** před, doporučujeme si projít **ASP.NET MVC vlastní akce filtry** praktického testovacího prostředí.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [injektáž závislostí aplikace ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

V **orientované programování objekt** paradigma, objekty spolu fungují v modelu spolupráce tam, kde jsou přispěvatelé a spotřebitele. Tento komunikační model přirozeně, generuje závislosti mezi objekty a jejich komponent, stávají obtížné spravovat, pokud se zvyšuje složitost.

![Třída závislosti a model složitost](aspnet-mvc-4-dependency-injection/_static/image1.png "třídy závislosti a složitosti modelu")

*Třída závislostí a složitosti modelu*

Pravděpodobně jste slyšeli o **Factory vzor** a oddělení mezi rozhraní a implementaci služby, ve kterém jsou objekty klienta často za umístění služeb.

Injektáž závislostí vzor je konkrétní implementaci ovládacího prvku inverzi. **IOC (Inversion) ovládacího prvku** znamená, že objekty ne vytvořit další objekty, na kterých jsou závislé ke své práci. Vývojáři získají objekty, které potřebují z vnějšího zdroje (například konfigurační soubor xml).

**Dependency Injection (DI)** znamená, že to se provádí bez zásahu objekt obvykle komponentou architektury, které předává parametry konstruktoru a nastavte vlastnosti.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Návrhový vzor závislostí vkládání (DI)

Na vysoké úrovni, který je cílem injektáž závislostí třída klienta (například *golfer*) potřebuje něco, co splňuje rozhraní (třeba *IClub*). Není vás, co je konkrétní typ (například *WedgeClub WoodClub IronClub,* nebo *PutterClub*), někdo jiný, který chce (například dobré *caddy*). Překladač závislostí v architektuře ASP.NET MVC můžete povolit registraci logiky závislost někde jinde (třeba kontejner nebo *kontejner kluby*).

![Diagram injektáž závislostí](aspnet-mvc-4-dependency-injection/_static/image2.png "obrázek injektáž závislostí")

*Injektáž závislostí - přirovnání Golf*

Výhody použití vzoru injektáž závislostí a IOC ovládacího prvku jsou následující:

- Snižuje párování tříd
- Zvyšuje opětovné použití kódu
- Zlepšuje udržovatelnosti kódu
- Zlepšuje testování aplikace

> [!NOTE]
> Injektáž závislostí někdy ve srovnání s abstraktní Factory návrhový vzor, ale není lehké rozdíl mezi oba přístupy. DI má rámec práce za vyřešit závislosti voláním továren a registrovaných služeb.


Teď, když rozumíte vzor injektáž závislostí, se naučíte v celém tomto testovacím prostředí použít v architektuře ASP.NET MVC 4. Se spustí pomocí vkládání závislostí v **řadiče** zahrnout databázová služba. V dalším kroku použijete injektáž závislostí do **zobrazení** využívání služby a zobrazit informace. Nakonec rozšíříte DI na ASP.NET MVC 4 filtry, vkládání filtr vlastních akcí v řešení.

V tomto praktického testovacího prostředí se dozvíte, jak:

- Integrace architektury ASP.NET MVC 4 s Unity pro vkládání závislostí pomocí balíčků NuGet
- Pomocí vkládání závislostí uvnitř kontroler ASP.NET MVC
- Pomocí vkládání závislostí uvnitř zobrazení ASP.NET MVC
- Pomocí vkládání závislostí uvnitř filtr akce s ASP.NET MVC

> [!NOTE]
> Toto testovací prostředí používá Unity.Mvc3 balíček NuGet pro řešení závislostí, ale je možné přizpůsobit libovolné architektury injektáž závislostí pro práci s ASP.NET MVC 4.


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

Podle následující praktická cvičení se skládá tohoto praktického testovacího prostředí:

1. [Cvičení 1: Injektáž Kontroleru](#Exercise1)
2. [Cvičení 2: Vložení zobrazení](#Exercise2)
3. [Cvičení 3: Injektáž filtry](#Exercise3)

> [!NOTE]
> Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.


Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Cvičení 1: Injektáž Kontroleru

V tomto cvičení se dozvíte, jak používat injektáž závislostí v Kontrolery architektury ASP.NET MVC integrací Unity pomocí balíčku NuGet. Z tohoto důvodu bude obsahovat služby řadičům MvcMusicStore oddělení logiky od přístupu k datům. Služby se vytvoří novou závislost v kontroleru konstruktor, který bude vyřešen pomocí vkládání závislostí pomocí **Unity**.

Tento přístup se ukazují, jak generovat méně provázané aplikace, které jsou větší flexibilitu a jednodušší údržbu a testování. Také se dozvíte, jak integrovat technologie ASP.NET MVC pomocí Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>O službě StoreManager

Zahrnuje službu, která spravuje data Store řadiče s názvem Music Store MVC, nyní k dispozici v řešení begin **StoreService**. Níže najdete implementace služby Store. Všimněte si, že všechny metody vrátí modelu entity.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** z zahájit řešení teď využívá **StoreService**. Byly odebrány všechny odkazy na data z **StoreController**a je nyní možné změnit aktuální přístup poskytovatele dat beze změny libovolné metody, která využívá **StoreService**.

Vás bude nižší, než **StoreController** implementace má závislost s **StoreService** uvnitř konstruktoru třídy.

> [!NOTE]
> Závislost zavedené v tomto cvičení souvisí s **ovládacího prvku inverzi** (Inversion).
> 
> **StoreController** obdrží konstruktoru třídy **IStoreService** parametr typu, což je velmi důležité provést volání služby z uvnitř třídy. Ale **StoreController** neimplementuje výchozí konstruktor (bez parametrů), žádné řadiče, musí mít pro práci s architekturou ASP.NET MVC.
> 
> Chcete-li vyřešit závislost, kontroleru je potřeba vytvořit abstraktní výrobou (třída, která vrací libovolný objekt zadaného typu).


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Obdržíte chybu třídy se pokusí vytvořit StoreController bez odeslání objekt služby, protože neexistuje žádný konstruktor deklarovat.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Úloha 1 – spuštění aplikace

V této úloze budete spouštět aplikaci Begin, které zahrnují službu do Kontroleru Store, který odděluje přístup k datům z aplikace logiky.

Při spuštění aplikace, zobrazí se výjimka, jak službu řadiče se předá jako parametr ve výchozím nastavení:

1. Otevřít **začít** řešení nachází v **vkládá Source\Ex01 Controller\Begin**.

   1. Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Stisknutím klávesy **Ctrl + F5** ke spuštění aplikace bez ladění. Zobrazí se chybová zpráva &quot; **žádný konstruktor bez parametrů definovaných pro tento objekt**&quot;:

    ![Chyba při spouštění aplikace začít ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "Chyba při spouštění aplikace začít ASP.NET MVC")

    *Chyba při spouštění aplikace začít ASP.NET MVC*
3. Zavřete prohlížeč.

V následujícím postupu bude fungovat u řešení Music Store vložení závislosti, musí tento kontroler.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Úloha 2 – včetně Unity do MvcMusicStore řešení

V této úloze bude obsahovat **Unity.Mvc3** balíček NuGet do řešení.

> [!NOTE]
> Balíček Unity.Mvc3 je navržená pro ASP.NET MVC 3, ale je plně kompatibilní s ASP.NET MVC 4.
> 
> Unity pro instanci je kontejner vkládání závislostí jednoduchých, rozšiřitelných s volitelnou podporou a zadejte zachycení. Je kontejner pro obecné účely pro použití v jakéhokoli typu aplikace .NET. Poskytuje společné funkce v včetně mechanismy injektáž závislostí: vytvoření objektu, abstrakce požadavků podle určení závislostí v modulu runtime, flexibilitu a odložením Konfigurace komponent do kontejneru.


1. Nainstalujte **Unity.Mvc3** balíček NuGet v **MvcMusicStore** projektu. Chcete-li to provést, otevřete **Konzola správce balíčků** z **zobrazení** | **ostatní Windows**.
2. Spusťte následující příkaz.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Instaluje se balíček NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "instaluje se balíček NuGet Unity.Mvc3")

    *Instaluje se balíček NuGet Unity.Mvc3*
3. Jakmile **Unity.Mvc3** balíček nainstalován, prozkoumejte soubory a složky, aby bylo možné zjednodušit konfiguraci Unity automaticky přidá.

    ![Nainstalovaný balíček Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "nainstalovaný balíček Unity.Mvc3")

    *Nainstalovaný balíček Unity.Mvc3*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Úloha 3 – registrace Unity v aplikaci Global.asax.cs\_Start

V této úloze budete aktualizovat **aplikace\_Start** metoda umístěný v **Global.asax.cs** volání inicializátor zaváděcího nástroje Unity a potom aktualizujte registraci souboru zaváděcího nástroje Služba a řadič bude používat pro vkládání závislostí.

1. Teď jste připojili zaváděcí nástroj, který je soubor, který inicializuje kontejneru Unity a překladač závislostí. Chcete-li to provést, otevřete **Global.asax.cs** a přidejte následující zvýrazněný kód v rámci **aplikace\_Start** metody.

    (Fragment - kódu *Lab vkládání závislostí ASP.NET - Ex01 - inicializace Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Otevřít **Bootstrapper.cs** souboru.
3. Následující obory názvů: **MvcMusicStore.Services** a **MusicStore.Controllers**.

    (Fragment - kódu *ASP.NET závislost vkládání Lab – Ex01 - Bootstrapperu přidání oborů názvů*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Nahraďte **BuildUnityContainer** obsah následujícím kódem, který registruje Store Kontroleru a službu Store vašeho metody.

    (Fragment - kódu *ASP.NET závislost vkládání Lab – Ex01 - Store Register řadič a služby*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Úloha 4 – spouštění aplikace

V této úloze budete spouštět aplikace kvůli ověření, že jej lze načíst teď po zahrnutí Unity.

1. Stisknutím klávesy **F5** ke spuštění aplikace, aplikace by se měly načíst nyní bez toho, abych jakákoli chybová zpráva.

    ![Spuštění aplikace pomocí vkládání závislostí](aspnet-mvc-4-dependency-injection/_static/image6.png "spuštění aplikace pomocí vkládání závislostí")

    *Spuštění aplikace pomocí vkládání závislostí*
2. Přejděte do **/Store**. Tím se vyvolá **StoreController**, který je vytvořený pomocí **Unity**.

    ![Aplikace MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")

    *Aplikace MVC Music Store*
3. Zavřete prohlížeč.

V následujícím cvičení se dozvíte, jak rozšířit rozsah injektáž závislostí ji používat uvnitř zobrazení ASP.NET MVC a filtrů akce.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Cvičení 2: Vložení zobrazení

V tomto cvičení se dozvíte, jak pomocí vkládání závislostí v zobrazení s novými funkcemi technologie ASP.NET MVC 4 pro integraci Unity. Aby bylo možné provést, se volání vlastní služby uvnitř zobrazení procházet Store, které se zobrazí zpráva a bitovou kopii níže.

Pak bude integrovat projektu pomocí Unity a vytvořit překladače vlastní závislost při vložení závislosti.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Úloha 1 – vytváření zobrazení, které využívá službu

V této úloze vytvoříte zobrazení, která provádí volání služby ke generování novou závislost. Služba se skládá z jednoduchého zasílání zpráv služby zahrnuté v tomto řešení.

1. Otevřít **začít** řešení nachází v **vkládá Source\Ex02 View\Begin** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
      > 
      > Další informace najdete v tomto článku: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Zahrnout **MessageService.cs** a **IMessageService.cs** třídy umístěny v **zdroje \Assets** složky v **/Services**. Chcete-li to provést, klikněte pravým tlačítkem na **služby** a pak zvolte položku **přidat existující položku**. Přejděte do umístění souborů a zahrnout je.

    ![Přidání zprávy služby a služby rozhraní](aspnet-mvc-4-dependency-injection/_static/image8.png "přidání zprávy služby a služby rozhraní")

    *Přidání zprávy služby a služby rozhraní*

    > [!NOTE]
    > **IMessageService** rozhraní definuje dvě vlastnosti, které jsou implementované **MessageService** třídy. Tyto vlastnosti -**zpráva** a **ImageUrl**– ukládání zprávu a adresu URL obrázku, který se má zobrazit.
3. Vytvořit složku **/stránky** v projektu kořenová složka a pak přidejte existující třídu **MyBasePage.cs** z **Source\Assets**. Základní stránka, kterou bude dědit z má následující strukturu.

    ![Složka stránek](aspnet-mvc-4-dependency-injection/_static/image9.png "složce stránky")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Otevřít **Browse.cshtml** zobrazení z **/zobrazení/Store** složky a nastavte ji zdědit **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. V **Procházet** zobrazovat, přidávat volání **MessageService** zobrazíte bitovou kopii a napište zprávu, nenačte služba.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Úloha 2 – včetně překladače vlastní závislost a aktivátoru stránky vlastního zobrazení

V předchozí úloze vloží novou závislost uvnitř zobrazení provádět volání služby dovnitř. Nyní, vyřeší dané závislosti implementací rozhraní injektáž závislostí ASP.NET MVC **IViewPageActivator** a **IDependencyResolver**. V řešení bude obsahovat implementace **IDependencyResolver** , který se bude zabývat načítání služby pomocí Unity. Potom bude obsahovat jiné vlastní implementaci **IViewPageActivator** rozhraní, které se vyřeší vytváření zobrazení.

> [!NOTE]
> Od verze technologie ASP.NET MVC 3 měla implementace pro vkládání závislostí zjednodušené rozhraní pro registraci služby. **IDependencyResolver** a **IViewPageActivator** jsou součástí sady funkcí technologie ASP.NET MVC 3 pro vkládání závislostí.
> 
> **-IDependencyResolver** rozhraní nahradí předchozí IMvcServiceLocator. Implementátoři IDependencyResolver musí vracet instanci služby nebo služby kolekce.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** rozhraní poskytuje jemně odstupňovanou kontrolu nad jak zobrazit stránky jsou vytvořeny pomocí vkládání závislostí. Třídy, které implementují **IViewPageActivator** rozhraní můžete vytvořit zobrazení instance pomocí informací o kontextu.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Vytvořte /**továren** složky v kořenové složce projektu.
2. Zahrnout **CustomViewPageActivator.cs** do svého řešení z **/zdroje/prostředky/** k **továren** složky. Chcete-li to mohli udělat, klikněte pravým tlačítkem **/Factories** složky, vyberte **přidat | Existující položka** a pak vyberte **CustomViewPageActivator.cs**. Tato třída implementuje **IViewPageActivator** rozhraní pro uložení kontejneru Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** zodpovídá za správu vytváření zobrazení pomocí Unity kontejneru.
3. Zahrnout **UnityDependencyResolver.cs** souboru z **/zdroje/prostředky** k **/Factories** složky. Chcete-li to mohli udělat, klikněte pravým tlačítkem **/Factories** složky, vyberte **přidat | Existující položka** a pak vyberte **UnityDependencyResolver.cs** souboru.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** třída je vlastní překladače závislostí pro Unity. Když služba nebyla nalezena uvnitř kontejneru Unity, je invocated základní překladač.

V následující úlohy se zaregistruje obou implementacích Pokud chcete, aby model znát umístění služby a zobrazení.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Úloha 3 – registrace pro injektáž závislostí v rámci kontejneru Unity

V této úloze zařadí všechny předchozí věci dohromady a ujistěte se, vkládání závislostí fungovat.

Až teď vaše řešení obsahuje následující prvky:

- A **Procházet** zobrazení, která dědí z **MyBaseClass** a využívá **MessageService**.
- Zprostředkující třída -**MyBaseClass**–, který má deklarován pro rozhraní služby vkládání závislostí.
- Služby - **MessageService** - a jeho rozhraní **IMessageService**.
- Překladače vlastní závislost pro Unity – **UnityDependencyResolver** –, který se zabývá načítání služby.
- Aktivátor stránky zobrazení - **CustomViewPageActivator** –, který vytvoří danou stránku.

Chcete-li vložit **Procházet** zobrazení, se nyní zaregistrujete překladače vlastní závislost v kontejneru Unity.

1. Otevřít **Bootstrapper.cs** souboru.
2. Zaregistrovat instanci **MessageService** do kontejneru Unity na inicializaci služby:

    (Fragment - kódu *zpráva služba testovacího prostředí – Ex02 – registrace technologie ASP.NET závislost vkládání*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Přidejte odkaz na **MvcMusicStore.Factories** oboru názvů.

    (Fragment - kódu *ASP.NET závislost vkládání Lab – Ex02 – objekty pro vytváření Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Zaregistrujte **CustomViewPageActivator** jako aktivátoru stránky zobrazení do kontejneru Unity:

    (Fragment - kódu *ASP.NET závislost vkládání Lab – Ex02 - Register CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. ASP.NET MVC 4 výchozího překladače závislostí provede nahraďte instance **UnityDependencyResolver**. Chcete-li to provést, nahraďte **inicializační** metoda obsah následujícím kódem:

    (Fragment - kódu *překladač závislostí ASP.NET závislost vkládání Lab – Ex02 – aktualizace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC poskytuje výchozí třídu překladače závislostí. Pro práci s překladače vlastní závislostí, které jsme vytvořili pro unity, má tento překladač se nahradí.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Úloha 4 – spouštění aplikace

V této úloze budete spouštět aplikace kvůli ověření, že prohlížeč Store využívá službu a zobrazí obrázek a zprávy načtené:

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Klikněte na tlačítko **Rock** v rámci nabídky žánry a najdete v tématu Jak **MessageService** se vloží do zobrazení a načíst zobrazení uvítací zprávy a image. V tomto příkladu jsme zadáváte &quot; **Rock**&quot;:

    ![MVC Music Store – zobrazení vkládání](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - vkládání zobrazení")

    *MVC Music Store - vkládání zobrazení*
3. Zavřete prohlížeč.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Cvičení 3: Injektáž filtrů Akce

V předchozím praktických cvičení **vlastní filtry akce** jste už pracovali s filtry přizpůsobení a vkládání. V tomto cvičení se dozvíte, jak vkládat pomocí Unity kontejneru filtrů pomocí vkládání závislostí. K tomu, které přidáte do řešení Music Store filtr vlastní akce, který bude sledovat aktivitu webu.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Úloha 1 – včetně sledování filtru v řešení

V této úloze zahrnete do Music Store vlastní akce filtru na události trasování. Jako filtr vlastních akcí jsou již považovány koncepty v předchozím prostředí &quot;vlastní filtry akce&quot;, se jednoduše uvést třídy filtru ve složce prostředky tohoto testovacího prostředí a pak vytvořte zprostředkovatele filtrů pro Unity:

1. Otevřít **začít** řešení nachází v **Source\Ex03 - vkládá Filter\Begin akce** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
      > 
      > Další informace najdete v tomto článku: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Zahrnout **TraceActionFilter.cs** souboru z **/zdroje/prostředky** k **/filtruje** složky.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Tento filtr vlastních akcí provádí trasování rozhraní ASP.NET. Můžete zkontrolovat &quot;místní ASP.NET MVC 4 a dynamické filtry akce&quot; testovacího prostředí pro odkaz na další.
3. Přidat prázdnou třídu **FilterProvider.cs** do projektu ve složce   **/filtry.**
4. Přidat **System.Web.Mvc** a **Microsoft.Practices.Unity** obory názvů v **FilterProvider.cs**.

    (Fragment - kódu *ASP.NET závislost vkládání Lab – Ex03 - zprostředkovatele filtrů přidání oborů názvů*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Ujistěte se, třída dědila z **IFilterProvider** rozhraní.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Přidat **IUnityContainer** vlastnost **FilterProvider** třídy a pak vytvořte konstruktor třídy přiřadit kontejneru.

    (Fragment - kódu *ASP.NET závislost vkládání Lab – Ex03 - filtru poskytovatele konstruktor*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Konstruktor třídy zprostředkovatele filtru není vytváření **nové** objektu. Kontejner je předán jako parametr a závislost je vyřešen Unity.
7. V **FilterProvider** třídy, implementovat metodu **GetFilters** z **IFilterProvider** rozhraní.

    (Fragment - kódu *ASP.NET závislost vkládání Lab – Ex03 - filtru poskytovatele GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Úloha 2 – registrace a povolení filtr

V této úloze povolíte sledování webu. K tomuto účelu se zaregistrovat tohoto filtru v **Bootstrapper.cs BuildUnityContainer** metodu spustit trasování:

1. Otevřít **Web.config** v kořenu projektu a povolit trasování sledování na skupinu System.Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Otevřít **Bootstrapper.cs** v kořenu projektu.
3. Přidejte odkaz na **MvcMusicStore.Filters** oboru názvů.

    (Fragment - kódu *ASP.NET závislost vkládání Lab – Ex03 - Bootstrapperu přidání oborů názvů*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Vyberte **BuildUnityContainer** metoda a zaregistrujte filtru v kontejneru Unity. Budete muset zaregistrovat zprostředkovatele filtrů, stejně jako filtr akce.

    (Fragment - kódu *ASP.NET závislost vkládání Lab – Ex03 - Register FilterProvider a ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Úloha 3 – spuštění aplikace

V této úloze se spustit aplikaci a otestovat, zda filtr vlastních akcí je trasování aktivity:

1. Stisknutím klávesy **F5** ke spuštění aplikace.
2. Klikněte na tlačítko **Rock** v rámci nabídky žánrů. Můžete vyhledat další žánry Pokud budete chtít.

    ![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")

    *Aplikace Music Store*
3. Přejděte do **/Trace.axd** zobrazíte trasování aplikace stránce a potom klikněte na tlačítko **zobrazit podrobnosti o**.

    ![Protokol trasování aplikace](aspnet-mvc-4-dependency-injection/_static/image12.png "protokolu trasování aplikace")

    *Protokol trasování aplikace*

    ![Trasování aplikace – podrobnosti o žádosti](aspnet-mvc-4-dependency-injection/_static/image13.png "trasování aplikace – podrobnosti o požadavku")

    *Trasování aplikace – podrobnosti o žádosti*
4. Zavřete prohlížeč.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Po dokončení tohoto praktického testovacího prostředí jste se dozvěděli, jak pomocí vkládání závislostí v architektuře ASP.NET MVC 4 integrací Unity pomocí balíčku NuGet. Abychom toho dosáhli, používáte injektáž závislostí do kontrolerů, zobrazení a filtrů akce.

Následující koncepty byly pokryty:

- Funkce vkládání závislostí aplikace ASP.NET MVC 4
- Pomocí balíčku NuGet Unity.Mvc3 integrace Unity
- Injektáž závislostí do Kontrolerů
- Injektáž závislostí v zobrazeních.
- Injektáž závislostí filtrů Akce

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.

    ![Instalace sady Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "instalace sady Visual Studio Express")

    *Instalace sady Visual Studio Express*
4. Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Přijetí podmínek licence](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Přijetí podmínek licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Průběh instalace*
6. Až instalace skončí, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.

    ![VS Express for Web dlaždice](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express for Web dlaždice*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Příloha B: používání fragmentů kódu

Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky. Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-dependency-injection/_static/image19.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")

*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.
4. Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).
5. Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.

![Začněte psát název fragmentu kódu](aspnet-mvc-4-dependency-injection/_static/image20.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](aspnet-mvc-4-dependency-injection/_static/image21.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")

*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*

![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](aspnet-mvc-4-dependency-injection/_static/image22.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")

*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.
2. Kliknutím na vyberte relevantní fragment kódu ze seznamu.

![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-dependency-injection/_static/image23.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-dependency-injection/_static/image24.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")

*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*
