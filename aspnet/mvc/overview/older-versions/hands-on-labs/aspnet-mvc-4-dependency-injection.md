---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "Vkládání závislostí architektury ASP.NET MVC 4 | Microsoft Docs"
author: rick-anderson
description: "Poznámka: Toto testovací prostředí Hands-on předpokládá, že máte základní znalosti o architektuře ASP.NET MVC a ASP.NET MVC 4 filtry. Pokud jste nepoužili ASP.NET MVC 4 filtry před jsme rec..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 48a7d7fdb670aebb72450fc4eb12a364ef595c53
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-dependency-injection"></a>Vkládání závislostí architektury ASP.NET MVC 4
====================
podle [webové táborech Team](https://twitter.com/webcamps)

> [!NOTE]
> Toto testovací prostředí Hands-on předpokládá, že máte základní znalosti o **ASP.NET MVC** a **ASP.NET MVC 4 filtry**. Pokud jste nepoužili **ASP.NET MVC 4 filtry** před, doporučujeme si projít **ASP.NET MVC vlastní akce filtry** Hands-on testovacího prostředí.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).


V **zaměřené na konkrétní objekt programování** zlepší, objekty spolupracují v modelu spolupráce tam, kde existují přispěvatelé a příjemce. Tento model komunikace samozřejmě generuje závislosti mezi objekty a součásti, aby se aktivovala obtížné spravovat v případě, že zvyšuje složitost.

![Třídy závislosti a model složitost](aspnet-mvc-4-dependency-injection/_static/image1.png "třídy závislosti a model složitost")

*Třída závislosti a složitost modelu*

Pravděpodobně jste slyšeli o **vzor objektu pro vytváření** a oddělení mezi rozhraní a implementace pomocí služeb, kde jsou objekty klienta často zodpovědní za umístění služby.

Vzor vkládání závislostí je konkrétní implementace inverzi ovládacího prvku. **Inverze ovládacího prvku (IoC)** znamená, že objekty není vytvořit jiné objekty, které spoléhají při své práci. Místo toho budou získat objekty, které potřebují z vnějšího zdroje (například konfiguračního souboru xml).

**Vkládání závislostí (DI)** znamená, že to je potřeba bez zásahu objekt obvykle součásti framework, který předává parametry konstruktor a nastavte vlastnosti.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Vzor návrhu závislostí vkládání (DI)

Na vysoké úrovni, který je cílem vkládání závislostí třída klienta (například *golfer*) vyžaduje něco, který splňuje rozhraní (například *IClub*). Nebude vědět, co je konkrétní typ (například *WoodClub, IronClub, WedgeClub* nebo *PutterClub*), ho někdo jiný, který chce (například dobrou *caddy*). Překladač závislostí v architektuře ASP.NET MVC lze umožňují registraci logika závislostí někde jinde (třeba kontejner nebo *balík kluby*).

![Diagram vkládání závislostí](aspnet-mvc-4-dependency-injection/_static/image2.png "vkládání závislostí obrázku")

*Vkládání závislostí - Golf analogie*

Výhody používání vzor vkládání závislostí a v inverzi řízení jsou následující:

- Snižuje párování tříd
- Zvyšuje opětovné použití kódu
- Zvyšuje udržovatelnost kódu
- Zlepšuje testování aplikací

> [!NOTE]
> Vkládání závislostí někdy ve srovnání s abstraktní vzoru návrhu objektu pro vytváření, ale je malého rozdílu mezi obou přístupů. DI má rozhraní práce za k volání objekty pro vytváření a registrované služby vyřešit závislosti.


Teď, když znáte vzoru vkládání závislostí, se dozvíte v celém tomto testovacím prostředí jak se má použít v architektuře ASP.NET MVC 4. Se spustí pomocí vkládání závislostí v **řadiče** zahrnout služby přístupu k databázi. V dalším kroku použijete vkládání závislostí **zobrazení** využívat služby a zobrazit informace. Nakonec rozšíříte DI ASP.NET MVC 4 filtrů, vložení vlastní akce filtru v řešení.

V tomto testovacím prostředí Hands-on se dozvíte, jak:

- Integrovat architektury ASP.NET MVC 4 s Unity vkládání závislostí pomocí balíčků NuGet
- Pomocí vkládání závislostí uvnitř řadič rozhraní ASP.NET MVC
- Pomocí vkládání závislostí uvnitř zobrazení ASP.NET MVC
- Pomocí vkládání závislostí uvnitř filtr akce rozhraní ASP.NET MVC

> [!NOTE]
> Toto testovací prostředí používá Unity.Mvc3 balíček NuGet pro řešení závislostí, ale je možné upravit libovolnou architekturu vkládání závislostí pro práci s ASP.NET MVC 4.


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

Toto testovací prostředí Hands-On se skládá ve cvičeních následující:

1. [Cvičení 1: Vložení řadič](#Exercise1)
2. [Cvičení 2: Vložení zobrazení](#Exercise2)
3. [Cvičení 3: Vložení filtry](#Exercise3)

> [!NOTE]
> Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.


Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Cvičení 1: Vložení řadič

V tomto cvičení se dozvíte, jak používat vkládání závislostí v Kontrolery architektury ASP.NET MVC integrací Unity pomocí balíčku NuGet. Z tohoto důvodu bude obsahovat služby do řadičů MvcMusicStore oddělit logiku z přístup k datům. Služby se vytvoří nové závislosti v konstruktoru řadiče, které budou vyřešeny pomocí vkládání závislostí za pomoci **Unity**.

Tento přístup vám ukáže, jak vygenerovat méně párované aplikace, které jsou větší flexibilitu a jednodušší pro údržbu a testování. Taky se dozvíte, jak integrovat Unity ASP.NET MVC.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>O StoreManager služby

Úložišti Hudba MVC součástí řešení begin nyní zahrnuje službu, která spravuje data řadič úložiště s názvem **StoreService**. Níže najdete implementace služby úložiště. Všimněte si, že všechny metody vrácení modelu entit.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** z začátek řešení teď využívá **StoreService**. Všechny odkazy na dat byly odebrány z **StoreController**a teď lze změnit aktuální poskytovatel dat přístupu beze změny libovolné metody, která využívá **StoreService**.

Zjistíte, pod kterou **StoreController** implementace má závislosti s **StoreService** uvnitř konstruktoru třídy.

> [!NOTE]
> Závislost byla zavedená v tomto cvičení souvisí s **inverzi řízení** (IoC).
> 
> **StoreController** obdrží konstruktoru třídy **IStoreService** parametr typu, který je nezbytné provést volání služby z uvnitř třídy. Ale **StoreController** neimplementuje výchozí konstruktor (s žádné parametry), který musí mít všechny řadič pro práci s architekturou ASP.NET MVC.
> 
> Vyřešit závislost, řadičem musí být vytvořený objekt factory abstraktní (třídu, která vrátí všechny objekt zadaného typu).


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Když se pokusí vytvořit StoreController bez odeslání objektu služby, protože neexistuje žádný bezparametrový konstruktor deklarovat třídy bude dojde k chybě.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Úloha 1 – spuštění aplikace

V této úloze budete spouštět aplikaci Begin, které zahrnuje službu do řadiče úložiště, který odděluje přístup k datům z aplikace logiky.

Při spuštění aplikace, zobrazí se výjimku, jako služba řadiče není jako parametr předaný ve výchozím nastavení:

1. Otevřete **začít** řešení umístěný v **vložení Source\Ex01 Controller\Begin**.

    1. Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
    2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
    3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

    > [!NOTE]
    > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Stiskněte klávesu **kombinaci kláves Ctrl + F5** ke spuštění aplikace bez ladění. Zobrazí se chybová zpráva &quot; **žádný konstruktor bez parametrů definované pro tento objekt**&quot;:

    ![Chyba při spouštění aplikace začít ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "Chyba při spouštění aplikace začít ASP.NET MVC")

    *Chyba při spouštění aplikace začít ASP.NET MVC*
3. Zavřete prohlížeč.

V následujících krocích budete pracovat v řešení úložiště Hudba vložení závislostí, které tento řadič potřebuje.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Úloha 2 – včetně Unity do MvcMusicStore řešení

V této úloze bude obsahovat **Unity.Mvc3** balíček NuGet pro řešení.

> [!NOTE]
> Balíček Unity.Mvc3 byl navržený pro ASP.NET MVC 3, ale je plně kompatibilní s ASP.NET MVC 4.
> 
> Unity je kontejner vkládání závislostí lightweight, extensible s volitelnou podporu pro instanci a zadejte zachycení. Je kontejner pro obecné účely pro použití v jakéhokoli typu aplikace .NET. Poskytuje společné funkce najít v mechanismy vkládání závislostí, včetně: vytvoření objektu, abstrakce požadavky zadáním závislosti na modulu runtime a flexibilitu, rozlišením konfigurace komponent do kontejneru.


1. Nainstalujte **Unity.Mvc3** balíček NuGet v **MvcMusicStore** projektu. Chcete-li to provést, otevřete **Konzola správce balíčků** z **zobrazení** | **ostatní okna**.
2. Spusťte následující příkaz.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Instalace balíčku NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "instalace balíčku NuGet Unity.Mvc3")

    *Instalace balíčku NuGet Unity.Mvc3*
3. Jednou **Unity.Mvc3** balíček nainstalován, prozkoumejte soubory a složky, se automaticky přidá, aby bylo možné zjednodušit konfiguraci Unity.

    ![Unity.Mvc3 balíček nainstalován](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 balíček nainstalován")

    *Unity.Mvc3 balíček nainstalován*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Úloha 3 – registrace Unity v aplikaci Global.asax.cs\_Start

V této úloze budete aktualizovat **aplikace\_spustit** metoda umístěný v **Global.asax.cs** volání inicializátoru Unity zaváděcího nástroje a potom aktualizovat registraci soubor zaváděcího nástroje Služba a řadič bude používat pro vkládání závislostí.

1. Teď můžete spojit zaváděcí nástroj, který je soubor, který inicializuje kontejneru Unity a překladače závislostí. Chcete-li to provést, otevřete **Global.asax.cs** a přidejte následující zvýrazněný kód v rámci **aplikace\_spustit** metoda.

    (Code fragment kódu - *testovacím vkládání závislostí ASP.NET - Ex01 - inicializovat Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Otevřete **Bootstrapper.cs** souboru.
3. Zahrnout následujících oborů názvů: **MvcMusicStore.Services** a **MusicStore.Controllers**.

    (Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex01 - zaváděcího nástroje Přidání obory názvů*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Nahraďte **BuildUnityContainer** metoda je obsah s následující kód, který registruje řadič úložiště a služby úložiště.

    (Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex01 – registrace úložiště řadič a služby*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Úloha 4 – spuštění aplikace

V této úloze budete spouštět aplikace k ověření, že je lze načíst teď po včetně Unity.

1. Stiskněte klávesu **F5** ke spuštění aplikace, aplikace by se měly načíst teď bez zobrazení všech chybová zpráva.

    ![Aplikace s vkládání závislostí](aspnet-mvc-4-dependency-injection/_static/image6.png "s vkládání závislostí aplikací")

    *Aplikace s vkládání závislostí*
2. Přejděte do **/úložiště**. To bude vyvolání **StoreController**, který je teď vytvořený pomocí **Unity**.

    ![Úložiště Hudba MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Hudba úložiště")

    *Úložiště Hudba MVC*
3. Zavřete prohlížeč.

V následujícím cvičení se dozvíte, jak rozšířit oboru vkládání závislostí pro použití v zobrazení ASP.NET MVC a filtrů akce.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Cvičení 2: Vložení zobrazení

V tomto cvičení se dozvíte, jak používat vkládání závislostí v zobrazení s nových funkcí technologie ASP.NET MVC 4 pro integraci Unity. Aby bylo možné provést, bude volat vlastní služba uvnitř zobrazení procházet úložiště, které se zobrazí zprávu a bitovou kopii níže.

Potom bude projekt integrovat Unity a vytvořte překladač závislostí Vlastní vložení závislosti.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Úloha 1 – Vytvoření zobrazení, který využívá službu

V této úloze vytvoříte zobrazení, která provádí volání služby generovat nové závislosti. V jednoduchých službou zasílání zpráv zahrnuté v tomto řešení se skládá službu.

1. Otevřete **začít** řešení umístěný v **vložení Source\Ex02 View\Begin** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

    1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
    2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
    3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

    > [!NOTE]
    > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
    > 
    > Další informace najdete v tomto článku: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Zahrnout **MessageService.cs** a **IMessageService.cs** třídy umístěny ve **zdroje \Assets** složky v **/služeb**. Chcete-li to provést, klikněte pravým tlačítkem **služby** složky a vyberte **přidat existující položku**. Přejděte do umístění tyto soubory a zahrnout je.

    ![Přidání služba zpráv a rozhraní služby](aspnet-mvc-4-dependency-injection/_static/image8.png "přidání služba zpráv a rozhraní služby")

    *Přidání služba zpráv a rozhraní služby*

    > [!NOTE]
    > **IMessageService** rozhraní definuje dvě vlastnosti implementované **MessageService** třídy. Tyto vlastnosti -**zpráva** a **ImageUrl**-ukládat zprávy a adresa URL obrázku, který se má zobrazit.
3. Vytvořit složku **/stránky** v projektu kořenová složka a poté přidejte existující třída **MyBasePage.cs** z **Source\Assets**. Základní stránka, kterou bude dědit ze má následující strukturu.

    ![Složka stránek](aspnet-mvc-4-dependency-injection/_static/image9.png "složky stránky")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Otevřete **Browse.cshtml** zobrazit z **/zobrazení/úložiště** složky a nastavit jej dědí **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. V **Procházet** zobrazit, že přidáte volání **MessageService** zobrazíte bitovou kopii a zprávu načíst službou.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Úloha 2 – včetně překladač závislostí vlastní a aktivátoru stránky vlastní zobrazení

V předchozí úloze vložit nové závislosti uvnitř zobrazení volání služby uvnitř ho provést. Nyní, bude vyřešení této závislosti implementací rozhraní vkládání závislostí ASP.NET MVC **IViewPageActivator** a **IDependencyResolver**. V řešení bude obsahovat implementace **IDependencyResolver** který se bude zabývat načtení služby pomocí Unity. Pak bude obsahovat jiné vlastní implementaci **IViewPageActivator** rozhraní, které se řešení s vytvořením zobrazení.

> [!NOTE]
> Od verze ASP.NET MVC 3 měl implementaci pro vkládání závislostí zjednodušené rozhraní k registraci služeb. **IDependencyResolver** a **IViewPageActivator** jsou součástí ASP.NET MVC 3 funkcí pro vkládání závislostí.
> 
> **-IDependencyResolver** rozhraní nahrazuje předchozí IMvcServiceLocator. Implementátory IDependencyResolver musí vrátit instanci služby nebo služby kolekce.
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** rozhraní poskytuje jemně odstupňovanou kontrolu nad jak instance stránky zobrazení pomocí vkládání závislostí. Třídy, které implementují **IViewPageActivator** rozhraní můžete vytvořit zobrazení instancí pomocí informace o kontextu.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Vytvořte nebo**objekty Factory** složky v kořenové složce projektu.
2. Zahrnout **CustomViewPageActivator.cs** do řešení z **/zdroje/prostředky/** k **objekty Factory** složky. Chcete-li provést, klikněte pravým tlačítkem **/Factories** složky, vyberte **přidat | Existující položka** a pak vyberte **CustomViewPageActivator.cs**. Tato třída implementuje **IViewPageActivator** rozhraní pro uložení kontejneru Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** je zodpovědný za správu vytvoření zobrazení pomocí kontejner Unity.
3. Zahrnout **UnityDependencyResolver.cs** souboru z **/zdroje/prostředky** k **/Factories** složky. Chcete-li provést, klikněte pravým tlačítkem **/Factories** složky, vyberte **přidat | Existující položka** a pak vyberte **UnityDependencyResolver.cs** souboru.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** třída je vlastní překladače závislostí pro Unity. Pokud služba nebyla nalezena uvnitř kontejneru Unity, je invocated základní překladač.

V následující úlohu obou implementace registrována umožníte modelu vědět, umístění služeb a zobrazení.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Úloha 3 – registrace pro vkládání závislostí v rámci kontejneru Unity

V této úloze umístíte všechny předchozí věci a vytvořit tak pracovat vkládání závislostí.

Až teď vaše řešení obsahuje následující prvky:

- A **Procházet** zobrazení, která dědí z **MyBaseClass** a odebírá **MessageService**.
- Třídu zprostředkující -**MyBaseClass**-má deklarovat pro rozhraní služby vkládání závislostí.
- Může služba - **MessageService** - a jeho rozhraní **IMessageService**.
- Překladač závislostí vlastní for Unity - **UnityDependencyResolver** – který se zabývá načítání služby.
- Aktivátor stránky zobrazení - **CustomViewPageActivator** -vytvářející stránky.

Vložení **Procházet** zobrazení, se nyní zaregistrujete překladač závislostí vlastní v kontejneru Unity.

1. Otevřete **Bootstrapper.cs** souboru.
2. Registrace instance **MessageService** do kontejneru Unity k chybě při inicializaci služby:

    (Code fragment kódu - *služba ASP.NET závislostí vkládání laboratoř - Ex02 – registrace zpráv*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Přidat odkaz na **MvcMusicStore.Factories** oboru názvů.

    (Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex02 - továrny Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Zaregistrovat **CustomViewPageActivator** jako aktivátoru stránky zobrazení do kontejneru Unity:

    (Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex02 – registrace CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Nahraďte překladač závislostí ASP.NET MVC 4 výchozí instanci **UnityDependencyResolver**. Chcete-li to provést, nahraďte **inicializační** metoda obsahu s následujícím kódem:

    (Code fragment kódu - *překladač závislostí ASP.NET závislostí vkládání laboratoř - Ex02 - aktualizace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC poskytuje výchozí třídu překladače závislostí. Pro práci s překladače závislostí vlastní jako ten, který jsme vytvořili pro unity, má tento překladač vyměnit.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Úloha 4 – spuštění aplikace

V této úloze budete spouštět aplikaci ověřte, zda prohlížeč úložiště využívá službu a ukazuje bitovou kopii a načíst zprávu:

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Klikněte na tlačítko **Rock** v nabídce žánry a najdete v tématu Jak **MessageService** byl aplikován na zobrazení a načíst zobrazení uvítací zprávy a bitovou kopii. V tomto příkladu jsme k zadávání &quot; **Rock**&quot;:

    ![Úložiště Hudba MVC – zobrazení vkládání](aspnet-mvc-4-dependency-injection/_static/image10.png "úložiště Hudba MVC – zobrazení vkládání")

    *Úložiště Hudba MVC – zobrazení vkládání*
3. Zavřete prohlížeč.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Cvičení 3: Vložení filtrů Akce

V testovacím prostředí předchozí praktických **vlastní filtry akce** jste pracovali s filtry přizpůsobení a vkládání. V tomto cvičení se dozvíte, jak se pomocí kontejneru Unity vložit filtry pomocí vkládání závislostí. K tomu, přidáte do řešení úložiště Hudba vlastní akce filtr, který bude trasování aktivity lokality.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Úloha 1 – včetně sledování filtru v řešení

V této úloze bude v úložišti Hudba obsahovat vlastní akce filtru k události trasování. Jako vlastní akce filtru koncepty již zpracovány v testovacím prostředí předchozí &quot;vlastní filtry akce&quot;, bude jenom obsahují třídu filtru ve složce prostředky tohoto testovacího prostředí a pak vytvořte zprostředkovatele filtrů pro Unity:

1. Otevřete **začít** řešení umístěný v **Source\Ex03 - vložení Filter\Begin akce** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

    1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
    2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
    3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

    > [!NOTE]
    > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
    > 
    > Další informace najdete v tomto článku: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Zahrnout **TraceActionFilter.cs** souboru z **/zdroje/prostředky** k **/filtry** složky.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Tento filtr vlastní akce se provádí trasování rozhraní ASP.NET. Můžete zkontrolovat &quot;místní ASP.NET MVC 4 a dynamické filtry akce&quot; testovacího prostředí pro odkaz na další.
3. Přidání prázdné třídy **FilterProvider.cs** na projekt ve složce   **/filtry.**
4. Přidat **System.Web.Mvc** a **Microsoft.Practices.Unity** obory názvů v **FilterProvider.cs**.

    (Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex03 - zprostředkovatele filtrů přidání obory názvů*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Ujistěte se, dědí z třídy **IFilterProvider** rozhraní.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Přidat **IUnityContainer** vlastnost **FilterProvider** třídy a pak vytvořte konstruktoru třídy přiřadit kontejneru.

    (Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex03 - filtru zprostředkovatele konstruktor*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Konstruktoru třídy zprostředkovatele filtru není vytváření **nové** objektu uvnitř. Kontejner je předán jako parametr a závislost je vyřešeno Unity.
7. V **FilterProvider** třídy, implementovat metodu **GetFilters** z **IFilterProvider** rozhraní.

    (Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex03 - filtru zprostředkovatele GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Úloha 2 – registrace a povolení filtru

V této úloze povolíte sledování lokality. Kvůli tomu zaregistrujete filtru v **Bootstrapper.cs BuildUnityContainer** metoda spusťte trasování:

1. Otevřete **Web.config** nachází v kořenu projektu a povolit trasování sledování na skupinu System.Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Otevřete **Bootstrapper.cs** v kořenu projektu.
3. Přidat odkaz na **MvcMusicStore.Filters** oboru názvů.

    (Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex03 - zaváděcího nástroje Přidání obory názvů*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Vyberte **BuildUnityContainer** metoda a zaregistrujte filtru v kontejneru Unity. Budete mít k registraci zprostředkovatele filtrů, jakož i filtr akce.

    (Code fragment kódu - *ASP.NET závislostí vkládání laboratoř - Ex03 – registrace FilterProvider a ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Úloha 3 – spuštění aplikace

V této úloze bude aplikaci spustit a otestovat, zda filtr vlastní akce je trasování aktivity:

1. Stiskněte klávesu **F5** ke spuštění aplikace.
2. Klikněte na tlačítko **Rock** v nabídce žánry. Pokud chcete, můžete procházet na další žánry.

    ![Hudba úložiště](aspnet-mvc-4-dependency-injection/_static/image11.png "Hudba úložiště")

    *Aplikace Music Store*
3. Přejděte do **/Trace.axd** zobrazíte trasování aplikací a pak klikněte na tlačítko **zobrazit podrobnosti**.

    ![Protokol trasování aplikace](aspnet-mvc-4-dependency-injection/_static/image12.png "protokolu trasování aplikace")

    *Trasování protokolu aplikace*

    ![Trasování aplikací - podrobnosti požadavku](aspnet-mvc-4-dependency-injection/_static/image13.png "trasování aplikací - podrobnosti požadavku")

    *Trasování aplikací - podrobnosti požadavku*
4. Zavřete prohlížeč.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Provedením tohoto testovacího prostředí Hands-On jste se naučili, jak pomocí vkládání závislostí v architektuře ASP.NET MVC 4 integrací Unity pomocí balíčku NuGet. K dosažení, která se mají použít vkládání závislostí uvnitř řadiče, zobrazení a filtrů akce.

Následující koncepty byly zahrnuté:

- Funkce vkládání závislostí aplikace ASP.NET MVC 4
- Pomocí balíčku NuGet Unity.Mvc3 integrace Unity
- Vkládání závislostí v řadiče
- Vkládání závislostí v zobrazeních.
- Vkládání závislostí filtrů Akce

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze  **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; *Visual Studio Express 2012 pro Web se sadou Windows Azure SDK*&quot;.
2. Klikněte na **nyní nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.

    ![Nainstalovat Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "nainstalovat Visual Studio Express")

    *Nainstalovat Visual Studio Express*
4. Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Vyjádření souhlasu s podmínkami licence](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Vyjádření souhlasu s podmínkami licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Průběh instalace*
6. Po dokončení instalace, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.

    ![VS Express pro Web dlaždice](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express pro Web dlaždice*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Příloha B: používání fragmentů kódu

S fragmenty kódu máte všechny kód, který je nutné na dosah ruky. Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](aspnet-mvc-4-dependency-injection/_static/image19.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")

*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.
4. Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).
5. Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.

![Začněte psát název fragmentu](aspnet-mvc-4-dependency-injection/_static/image20.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](aspnet-mvc-4-dependency-injection/_static/image21.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")

*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*

![Stisknutím klávesy Tab znovu a fragmentu rozšíří](aspnet-mvc-4-dependency-injection/_static/image22.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")

*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.
2. Vyberte relevantní fragment kódu ze seznamu, kliknutím na.

![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](aspnet-mvc-4-dependency-injection/_static/image23.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](aspnet-mvc-4-dependency-injection/_static/image24.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")

*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*
