---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: "Rukou na testovacím: Sestavení jednostránkové aplikace (SPA) s rozhraním ASP.NET Web API a Angular.js | Microsoft Docs"
author: rick-anderson
description: "V tradiční webových aplikací klient (prohlížeč) inicializuje komunikaci se serverem tím, že požádá stránky. Server pak zpracuje požadavek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2015
ms.topic: article
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 9a748628d53878be380869ac5327de0111d2284d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Rukou na testovacího prostředí: Jednostránkové aplikace (SPA) s rozhraním ASP.NET Web API a Angular.js sestavení
====================
podle [webové táborech Team](https://twitter.com/webcamps)

[Stažení webové táborech cvičení Kit](http://aka.ms/webcamps-training-kit)

> V tradiční webových aplikací klient (prohlížeč) inicializuje komunikaci se serverem tím, že požádá stránky. Server pak zpracuje žádost a odešle klientovi HTML stránky. V následných interakce s stránce – například uživatel přejde na odkaz nebo odešle formulář s daty – nový požadavek odeslán do serveru a znovu spustí toku: server zpracuje žádost a odešle novou stránku v prohlížeči v odpovědi na požadavek na novou akci ED klientem.
> 
> V jednostránkové aplikace (SPA) je po počáteční žádosti o celou stránku načítání do prohlížeče, ale následné interakce provést prostřednictvím požadavky Ajax. To znamená, že v prohlížeči je aktualizovat jenom ta část stránky, která se změnila; není nutné znovu načíst celou stránku. Přístup SPA snižuje čas potřebný aplikací reagovat na akce uživatele, což vede k více plynulá práce prostředí.
> 
> Architektura SPA zahrnuje některé problémy, které se nenacházejí v tradiční webových aplikací. Ale rozvíjející se technologií, jako je webové rozhraní API ASP.NET, jako jsou rozhraní JavaScript AngularJS a nový styl funkce poskytované službou CSS3 usnadňují skutečně návrh a vytváření SPA.
> 
> V tomto prostředí na straně bude využít výhod těchto technologií implementovat Geek kvízu, trivia web založený na SPA konceptu. Nejprve budete implementovat vrstvě služby s rozhraním ASP.NET Web API vystavit požadované koncové body načtení kvízu s časovým limitem otázky a odpovědi uložit. Pak vytvoříte bohaté a dobře reagovaly uživatelského rozhraní pomocí AngularJS a CSS3 důsledky transformace.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Přehled

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí praktických se dozvíte, jak:

- Vytvoření služby ASP.NET Web API odesílat a přijímat JSON data
- Vytvořit citlivé uživatelské rozhraní pomocí AngularJS
- Zajištění lepších možností uživatelského rozhraní s CSS3 transformace

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Pro dokončení této praktické cvičení je vyžadován následující text:

- [Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/) nebo vyšší

<a id="Setup"></a>
### <a name="setup"></a>Instalace

Aby bylo možné spustit praktická cvičení v tomto testovacím prostředí praktické, musíte nejdřív nastavit svoje prostředí.

1. Otevřete Průzkumníka Windows a přejděte do testovacího prostředí na **zdroj** složky.
2. Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a instalaci sady Visual Studio fragmenty kódu pro toto testovací prostředí.
3. Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, která bude pokračovat.

> [!NOTE]
> Zkontrolujte, zda že jste zkontrolovali všechny závislosti pro toto testovací prostředí před spuštěním instalačního programu.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Používání fragmentů kódu

V dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu. Pro usnadnění vaší práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, kterým můžete přistupovat z v rámci Visual Studio 2013, abyste nemuseli přidat ručně.

> [!NOTE]
> Každý úkol je přiložena počáteční řešení umístěný v **začít** složky cvičení, která umožňuje postupujte podle jednotlivých cvičení nezávisle na ostatních. Upozorňujeme, že fragmenty kódu, které jsou přidány během cvičení chybí z nich spuštění řešení a nemusí fungovat, dokud nedokončíte výkonu. Uvnitř zdrojový kód pro cvičení, je také k dispozici **End** složku obsahující řešení sady Visual Studio s kódem, který je výsledkem kroků v odpovídající cvičení. Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické cvičení.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické cvičení zahrnuje následující cvičení:

1. [Vytvoření rozhraní Web API](#Exercise1)
2. [Vytváření rozhraní SPA](#Exercise2)

Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**

> [!NOTE]
> Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce. Každé předdefinované kolekce je navržené tak, aby odpovídala konkrétním vývoj styl a určuje rozložení oken, editor chování, IntelliSense – fragmenty kódu a možnosti dialogového okna. Postupy v tomto testovacím prostředí popisovat akce, které jsou nutné k dokončení daného úkolu v sadě Visual Studio, při použití **obecné nastavení vývoj** kolekce. Pokud si zvolíte jiné nastavení kolekce pro vývojové prostředí, může být rozdíly v krocích, které byste měli vzít v úvahu.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Cvičení 1: Vytvoření rozhraní Web API

Jedním z klíčových částí SPA je vrstvě služby. Je zodpovědná za zpracování volání Ajax odesílají uživatelského rozhraní a vrácení dat v reakci na toto volání. Data načtená by měl být uvedeny ve formátu strojově čitelným k analýze a spotřebovávají klienta.

Rozhraní Web API je součástí zásobníku technologie ASP.NET a je navržený tak, aby ji snadno implementovat služeb HTTP, obecně odesílání a příjmu dat prostřednictvím rozhraní RESTful API ve formátu JSON nebo XML. V tomto cvičení vytvoříte webu hostovat Geek kvízu aplikaci a potom implementovat službě back-end pro vystavení a zachovat data kvízu s časovým limitem pomocí rozhraní ASP.NET Web API.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Úloha 1 – vytvoření počáteční projekt pro Geek kvízu s časovým limitem

V této úloze se spustit vytvoření nového projektu ASP.NET MVC pomocí podporu pro ASP.NET Web API založené na **jeden ASP.NET** projektu typ, který se dodává s Visual Studio. **Jeden ASP.NET** kombinuje všechny technologie ASP.NET a vám dává možnost kombinovat a párovat je podle potřeby. Potom přidáte třídy modelu Entity Framework a databáze initializator vložit otázky kvízu s časovým limitem.

1. Otevřete **Visual Studio Express 2013 pro Web** a vyberte **souboru | Nový projekt...**  zahájíte nové řešení.

    ![Vytvoření nového projektu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "vytvoření nového projektu")

    *Vytvoření nového projektu*
2. V **nový projekt** dialogové okno, vyberte **webové aplikace ASP.NET** pod **Visual C# | Webové** kartě. Zajistěte, aby **rozhraní .NET Framework 4.5** je vybrána, pojmenujte ji *GeekQuiz*, vyberte **umístění** a klikněte na tlačítko **OK**.

    ![Vytvoření nového projektu webové aplikace ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "vytvoření nového projektu webové aplikace ASP.NET")

    *Vytvoření nového projektu webové aplikace ASP.NET*
3. V **nový projekt ASP.NET** dialogové okno, vyberte **MVC** šablony a vyberte **webového rozhraní API** možnost. Také zkontrolujte, zda **ověřování** je možnost nastavena na **jednotlivé uživatelské účty**. Klikněte na tlačítko **OK** pokračujte.

    ![Vytvoření nového projektu pomocí šablony MVC, včetně součásti webového rozhraní API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Vytvoření nového projektu pomocí šablony MVC, včetně součásti webového rozhraní API*
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **modely** složky **GeekQuiz** projektu a vyberte **přidat | Existující položka...** .

    ![Přidat existující položku](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "přidání existující položky")

    *Přidání existující položky*
5. V **přidat existující položku** dialogové okno pole, přejděte na **zdroje nebo prostředky nebo modely** složky a vyberte všechny soubory. Klikněte na tlačítko **přidat**.

    ![Přidání modelu prostředky](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "přidání prostředky modelu")

    *Přidání modelu prostředky*

    > [!NOTE]
    > Přidáním tyto soubory se přidat datový model, kontext databáze Entity Framework a aplikace Geek kvízu inicializátoru databáze.
    > 
    > **Entity Framework (EF)** je objekt relační mapper (ORM), která umožňuje vytvořit programování s modelem koncepční aplikace místo programování přímo pomocí schématu relační úložiště dat přístup k aplikacím. Další informace o rozhraní Entity Framework [zde](../../../entity-framework.md).
    > 
    > Následuje popis třídy, kterou jste právě přidali:
    > 
    > - **TriviaOption:** představuje jednu možnost související s otázkou kvízu s časovým limitem
    > - **TriviaQuestion:** reprezentuje kvízu s časovým limitem otázku a zveřejňuje související možnosti prostřednictvím **možnosti** vlastnost
    > - **TriviaAnswer:** představuje možnosti vybrané uživatelem jako odpověď na otázku kvízu s časovým limitem
    > - **TriviaContext:** představuje kontext databáze Entity Framework kvízu Geek aplikace. Tato třída odvozená z **DContext** a zveřejňuje **DbSet** vlastnosti, které představují kolekce entit popsané výše.
    > - **TriviaDatabaseInitializer:** implementace inicializátoru Entity Framework pro **TriviaContext** třída, která dědí z **CreateDatabaseIfNotExists**. Výchozí chování této třídy je vytvořit databázi pouze v případě, že neexistuje, vkládání entity zadaný v **počáteční hodnoty** metoda.
6. Otevřete **Global.asax.cs** souboru a přidejte následující příkaz using.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Přidejte následující kód na začátku **aplikace\_spustit** metodu a nastavit **TriviaDatabaseInitializer** jako inicializátoru databáze.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Změnit **Domů** řadiče omezit přístup k ověření uživatele. Chcete-li to provést, otevřete **HomeController.cs** souboru uvnitř **řadiče** složky a přidejte **Authorize** atribut **HomeController**definici třídy.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > **Autorizovat** filtrovat kontroluje, pokud je uživatel ověřený. Pokud není uživatel ověřen, vrátí stavový kód HTTP 401 (Neautorizováno) bez vyvolání akce. Můžete použít filtr globálně, na úrovni kontroleru, nebo na úrovni jednotlivých akcí.
9. Rozložení webové stránky a branding bude nyní přizpůsobit. Chcete-li to provést, otevřete  **\_Layout.cshtml** souboru uvnitř **zobrazení | Sdílené** složky a aktualizovat obsah  **&lt;název&gt;**  element nahrazením *Moje aplikace technologie ASP.NET* s *Geek kvízu* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. Ve stejném souboru aktualizovat odebráním navigačním panelu *o* a *kontaktujte* odkazy a přejmenování *Domů* propojit *přehrání*. Kromě toho přejmenovat *název aplikace* propojit *Geek kvízu*. Kód HTML pro navigační panel by měl vypadat jako následující kód.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Aktualizovat zápatí stránky rozložení nahrazením *Moje aplikace technologie ASP.NET* s *Geek kvízu*. Chcete-li to provést, nahradí obsah  **&lt;zápatí&gt;**  element s následující zvýrazněný kód.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Úloha 2 – Vytvoření TriviaController webové rozhraní API

V předchozí úloze vytvořit strukturu počáteční Geek kvízu webové aplikace. Teď vytvoříte jednoduchý služba webového rozhraní API, která komunikuje s kvízu s časovým limitem datový model a zveřejňuje následující akce:

- **GET/api/trivia**: načte další otázku ze seznamu kvízu s časovým limitem odpovídat pouze ověřeného uživatele.
- **POST/api/trivia**: ukládá odpovědí kvízu s časovým limitem určeného ověřeného uživatele.

ASP.NET generování uživatelského rozhraní nástroje poskytované subsystémem pro Visual Studio použije k vytvoření standardních hodnot pro třídy kontroleru webového rozhraní API.

1. Otevřete **WebApiConfig.cs** souboru uvnitř **aplikace\_spustit** složky. Tento soubor definuje konfiguraci webového rozhraní API služby, například jak trasy jsou mapované na akce kontroleru webového rozhraní API.
2. Přidejte následující příkaz using na začátku souboru.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Přidejte následující zvýrazněný kód, který **zaregistrovat** metoda globální konfigurace formátování data JSON získaných pomocí metody akce webového rozhraní API.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver** automaticky převede názvy vlastností, abyste *formátu camelCase* případu, kdy je obecné konvence pro názvy vlastností v jazyce JavaScript.
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **řadiče** složky **GeekQuiz** projektu a vyberte **přidat | Nové vygenerované položky...** .

    ![Vytvoření nové položky vygenerované](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "vytvoření nové položky vygenerované")

    *Vytvoření nové položky vygenerované*
5. V **přidat vygenerované uživatelské rozhraní** dialogové okno pole, ujistěte se, že **běžné** výběru uzlu v levém podokně. Pak vyberte **webové rozhraní API 2 řadiče - prázdný** šablony v prostředním podokně a klikněte na **přidat**.

    ![Výběr webové rozhraní API 2 řadiče prázdné šablonu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "výběr webového rozhraní API 2 řadiče prázdné šablony")

    *Výběr webové rozhraní API 2 řadiče prázdné šablony*

    > [!NOTE]
    > **Generování uživatelského rozhraní ASP.NET** je architektura generování kódu pro webové aplikace ASP.NET. Visual Studio 2013 zahrnuje generátory předem nainstalovaná kódu pro projekty MVC a webového rozhraní API. Pokud chcete rychle přidáte kód, který komunikuje s modely dat za účelem snížení množství času potřebné k vývoji operace standardní dat, používejte generování uživatelského rozhraní v projektu.
    > 
    > Proces generování uživatelského rozhraní také zajistí, že jsou v projektu nainstalovat všechny požadované závislosti. Například když začínat prázdný projekt ASP.NET a přidání kontroleru webového rozhraní API pomocí generování uživatelského rozhraní, požadované balíčky NuGet webové rozhraní API a odkazy se přidají do projektu automaticky.
6. V **přidat kontroler** dialogové okno, typ *TriviaController* v **názvu Kontroleru** textového pole a klikněte na tlačítko **přidat**.

    ![Přidání řadiče Trivia](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "přidání Trivia řadiče")

    *Přidání Trivia řadiče*
7. **TriviaController.cs** souboru se pak přidá do **řadiče** složky **GeekQuiz** projektu, který obsahuje prázdnou **TriviaController** třídy. Přidejte následující příkazy using na začátku souboru.

    (Code fragment kódu - *AspNetWebApiSpa - Ex1 - TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Přidejte následující kód na začátku **TriviaController** třídy definují, inicializace a uvolnění **TriviaContext** instance v kontroleru.

    (Code fragment kódu - *AspNetWebApiSpa - Ex1 - TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > **Dispose** metodu **TriviaController** vyvolá **Dispose** metodu **TriviaContext** instanci, která zajistí, že všechny uvolnění prostředků používá objekt context při **TriviaContext** je instance zrušena nebo uvolňování paměti. To zahrnuje zavřít všechna připojení databáze otevřít rozhraní Entity Framework.
9. Přidejte následující metodu helper na konci **TriviaController** třídy. Tato metoda načítá následující otázku kvízu s časovým limitem z databáze k zodpovězení zadaným uživatelem.

    (Code fragment kódu - *AspNetWebApiSpa - Ex1 - TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Přidejte následující **získat** metodu akce **TriviaController** třídy. Tato metoda akce volá **NextQuestionAsync** Pomocná metoda definované v předchozím kroku pro načtení další otázky ověřeného uživatele.

    (Code fragment kódu - *AspNetWebApiSpa - Ex1 - TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Přidejte následující metodu helper na konci **TriviaController** třídy. Tato metoda ukládá zadanou odpověď do databáze a vrátí logickou hodnotu, která určuje, zda je odpověď správná.

    (Code fragment kódu - *AspNetWebApiSpa - Ex1 - TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Přidejte následující **Post** metodu akce **TriviaController** třídy. Tato metoda akce přidruží odpověď na ověřeného uživatele a počet volání **StoreAsync** metodu helper. Pak odešle odpověď se logická hodnota vrácená metodou helper.

    (Code fragment kódu - *AspNetWebApiSpa - Ex1 - TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Upravit kontroleru webového rozhraní API k omezení přístupu k ověření uživatelé přidáním **Authorize** atribut **TriviaController** definici třídy.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Úloha 3 – spuštění řešení

V této úloze ověříte, že webového rozhraní API služby, kterou jste vytvořili v předchozí úloze funguje podle očekávání. Budete používat v Internet Exploreru **F12 Developer Tools** k zachycení síťového provozu a kontrola úplnou odpověď ze služby webového rozhraní API.

> [!NOTE]
> Ujistěte se, že **Internet Explorer** vybrán **spustit** tlačítko nachází na panelu nástrojů Visual Studio.
> 
> ![Možnost aplikace Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Stiskněte klávesu **F5** ke spuštění řešení. **Přihlásit** stránka by se měla objevit v prohlížeči.

    > [!NOTE]
    > Při spuštění aplikace se aktivuje výchozí trasu MVC, která ve výchozím nastavení se mapuje na **Index** akce **HomeController** třídy. Vzhledem k tomu **HomeController** omezen na ověřené uživatele (mějte na paměti, že jste dekorované třídy s **Autorizovat** atribut v cvičení 1) a neexistuje žádný uživatel ověřený, ale aplikace původní žádost přesměruje na přihlašovací stránky.

    ![Spuštění řešení](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "systémem řešení")

    *Spuštění řešení*
2. Klikněte na tlačítko **zaregistrovat** k vytvoření nového uživatele.

    ![Registrace nového uživatele](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "registrace nového uživatele")

    *Registrace nového uživatele*
3. V **zaregistrovat** stránky, zadejte **uživatelské jméno** a **heslo**a potom klikněte na **zaregistrovat**.

    ![Zaregistrovat stránky](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "stránku registrace")

    *Zaregistrovat stránky*
4. Aplikace zaregistruje nový účet a uživatel je ověřen a přesměrován zpět na domovskou stránku.

    ![Ověření uživatele](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "uživatel ověřený")

    *Ověření uživatele*
5. V prohlížeči stiskněte **F12** otevřete **Developer Tools** panelu. Stiskněte klávesu **CTRL + 4** nebo klikněte na tlačítko **sítě** ikonu a pak klikněte na tlačítko se zelenou šipkou zahájíte zachytávání síťového provozu.

    ![Probíhá inicializace zachycení dat ze sítě webového rozhraní API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "zachycení dat ze sítě inicializaci webového rozhraní API")

    *Inicializace webového rozhraní API zachycení dat ze sítě*
6. Připojit **rozhraní api nebo trivia** na adresu URL v adresním řádku prohlížeče. Nyní bude kontrolovat podrobnosti o odpověď **získat** metodu akce v **TriviaController**.

    ![Načítání dat Další otázka prostřednictvím webového rozhraní API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "načítání Další otázka dat prostřednictvím webového rozhraní API")

    *Načítání dat Další otázka prostřednictvím webového rozhraní API*

    > [!NOTE]
    > Po dokončení stahování se výzva, aby akce s stažený soubor. Ponechte dialogové okno otevřené, aby bylo možné sledovat obsahu odpovědi prostřednictvím okno nástroje pro vývojáře.
7. Nyní bude kontrolovat text odpovědi. Chcete-li to provést, klikněte na tlačítko **podrobnosti** a pak klikněte **odpovědi**. Můžete zkontrolovat, že stažená data je objekt s vlastnostmi **možnosti** (což je seznam **TriviaOption** objekty), **id** a **název** , odpovídají **TriviaQuestion** třídy.

    ![Zobrazení webové rozhraní API odpovědi](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "zobrazení webové rozhraní API odpovědi")

    *Zobrazení webové rozhraní API odpovědi*
8. Přejděte zpět na Visual Studio a stiskněte klávesu **SHIFT + F5** Zastavit ladění.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Cvičení 2: Vytvoření rozhraní SPA

V tomto cvičení nejprve vytvoříte front-end část webové Geek kvízu, zaměřené na interakce jednostránkové aplikace pomocí **AngularJS**. Pak se rozšířit činnost koncového uživatele s CSS3 k provedení bohaté animace a poskytují vizuální efekt kontextu přepínání při přechodu z jednoho otázku na další.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Úloha 1 – vytvoření pomocí AngularJS SPA rozhraní

V této úloze budete používat **AngularJS** implementovat kvízu Geek aplikace na straně klienta. **AngularJS** je architekturu JavaScript open source, která navyšuje aplikace založené na prohlížeči s *Model-View-Controller* (MVC) funkce, které usnadňují i vývoj a testování.

Začněte instalací AngularJS z konzoly Správce balíčků sady Visual Studio. Pak vytvoříte řadič zajistit chování aplikace Geek kvízu a zobrazení k vykreslení kvízu s časovým limitem otázky a odpovědi pomocí šablony modul AngularJS.

> [!NOTE]
> Další informace o AngularJS, najdete v části [ [http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/).


1. Otevřete **Visual Studio Express 2013 pro Web** a otevřete **GeekQuiz.sln** řešení umístěný v **zdroj/Ex2-CreatingASPAInterface/Begin** složky. Alternativně můžete pokračovat v řešení jste získali v předchozím cvičení.
2. Otevřete **Konzola správce balíčků** z **nástroje** | **Správce balíčků knihoven**. Zadejte následující příkaz k instalaci **AngularJS.Core** balíček NuGet.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **skripty** složky **GeekQuiz** projektu a vyberte **přidat | Nová složka**. Název složky **aplikace** a stiskněte klávesu **Enter**.
4. Klikněte pravým tlačítkem myši **aplikace** složku, kterou jste právě vytvořili a vyberte **přidat | Soubor JavaScript**.

    ![Vytvoření nového souboru JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Vytvoření nového souboru JavaScript*
5. V **zadejte název položky** dialogové okno, typ *kvízu s časovým limitem controller* v **název položky** textového pole a klikněte na tlačítko **OK**.

    ![Pojmenování nový soubor JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Pojmenování nový soubor JavaScript*
6. V **kvízu s časovým limitem controller.js** soubor, přidejte následující kód do deklarace a inicializace AngularJS **QuizCtrl** řadiče.

    (Code fragment kódu - *AspNetWebApiSpa - Ex2 - AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > Funkce konstruktoru **QuizCtrl** řadiče očekává, že injekčních parametr s názvem **$scope**. Počáteční stav oboru by měly být nastavené v konstruktoru funkce připojením vlastnosti, které chcete **$scope** objektu. Vlastnosti obsahovat **modelu zobrazení**a budou přístupné v šabloně při registraci kontroleru.
    > 
    > **QuizCtrl** řadiče je definován modul s názvem **QuizApp**. Moduly jsou jednotky práce, která umožňují rozdělit na samostatné součásti aplikace. Hlavní výhody používání modulů je, že kód je jednodušší zjistit a usnadňuje testování částí, – opětovné použití a jeho udržovatelnost.
7. Nyní přidáte chování do oboru aby reagování na události aktivované ze zobrazení. Přidejte následující kód na konci **QuizCtrl** řadiče k definování **nextQuestion** fungovat v **$scope** objektu.

    (Code fragment kódu - *AspNetWebApiSpa - Ex2 - AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Tato funkce načte další otázka z **Trivia** webového rozhraní API vytvořili v předchozím cvičení a připojí data otázku, která mají **$scope** objektu.
8. Vložte následující kód na konci **QuizCtrl** řadiče k definování **sendAnswer** fungovat v **$scope** objektu.

    (Code fragment kódu - *AspNetWebApiSpa - Ex2 - AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Tato funkce odesílá odpověď vybrané uživatelem **Trivia** webového rozhraní API a ukládá výsledek – tj. Pokud odpověď není správný, nebo není – v **$scope** objektu.
    > 
    > **NextQuestion** a **sendAnswer** funkce z výše použít AngularJS **$http** objekt, který chcete abstraktní komunikaci se službou webového rozhraní API prostřednictvím XMLHttpRequest Objekt jazyka JavaScript z prohlížeče. AngularJS podporuje jiné službě, která přináší vyšší úrovni abstrakce k provádění operací CRUD proti prostředků prostřednictvím rozhraní RESTful API. AngularJS **$resource** objekt má metody akce, které poskytují základní chování bez nutnosti využívat **$http** objektu. Zvažte použití **$resource** objektu ve scénářích, která vyžaduje CRUD modelu (fore informace najdete v článku [$resource dokumentace](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Dalším krokem je vytvoření šablony AngularJS, který definuje zobrazení kvízu. Chcete-li to provést, otevřete **Index.cshtml** souboru uvnitř **zobrazení | Domů** složky a nahraďte obsah následujícím kódem.

    (Code fragment kódu - *AspNetWebApiSpa - Ex2 - GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > Šablona AngularJS je deklarativní specifikace, která používá informace z modelu a řadič transformace statické značek na dynamické zobrazení, které uživateli se zobrazí v prohlížeči. Následují příklady AngularJS elementů a atributů elementu, které lze použít v šabloně:
    > 
    > - **Ng aplikace** – direktiva informuje AngularJS elementu DOM, který reprezentuje kořenový element aplikace.
    > - **Ng-controller** – direktiva řadič připojí k modelu DOM v místě, kde je deklarovaná direktivu.
    > - Zápis složené závorky **{{}}** označuje vazby na vlastnosti oboru definované v kontroleru.
    > - **Ng kliknutím** – direktiva slouží k vyvolání funkce definované v oboru v reakci na uživatel klikne na.
10. Otevřete **Site.css** souboru uvnitř **obsahu** složky a přidejte následující zvýrazněný stylů na konci souboru zajistit vzhled a chování pro zobrazení kvízu s časovým limitem.

    (Code fragment kódu - *AspNetWebApiSpa - Ex2 - GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Úloha 2 – spuštění řešení

V této úloze, bude vykonán řešení pomocí nové uživatelské rozhraní je vytvořené s AngularJS k odpovědi na některé dotazy kvízu s časovým limitem.

1. Stiskněte klávesu **F5** ke spuštění řešení.
2. Zaregistrujte nový uživatelský účet. Postupujte podle uvedeného postupu registrace v cvičení 1, úloze 3.

    > [!NOTE]
    > Pokud používáte řešení od předchozím cvičení, můžete se přihlaste se pomocí uživatelského účtu, který jste vytvořili před.
3. **Domů** by se měla objevit stránky zobrazující na první otázku kvízu. Kliknutím na jednu z možností odpovězte na otázku. Tím se aktivuje **sendAnswer** funkce definovaného dříve, který odesílá vybranou možnost **Trivia** webového rozhraní API.

    ![Odpovědi na dotaz](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "odpovědi na dotaz")

    *Odpovědi na dotaz*
4. Po kliknutí na tlačítko požadovaného, by se zobrazit odpověď. Klikněte na tlačítko **Další otázka** zobrazíte následující otázky. Tím se aktivuje **nextQuestion** funkce, které jsou definované v kontroleru.

    ![Vyžaduje další otázka](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "vyžaduje další otázky")

    *Vyžaduje další otázky*
5. Další otázka by se zobrazit. Pokračujte zodpovězení otázek tolikrát, kolikrát chcete. Po dokončení všechny otázky by měl vrátit na první otázku.

    ![Další otázka](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "jinou otázku")

    *Další otázka*
6. Přejděte zpět na Visual Studio a stiskněte klávesu **SHIFT + F5** Zastavit ladění.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Úloha 3 – vytvoření Flip animace pomocí CSS3

V této úloze bude používat CSS3 vlastnosti k provedení bohaté animací přidáním flip vliv, pokud je odpověď otázku, a když se načte další otázka.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **obsahu** složky **GeekQuiz** projektu a vyberte **přidat | Existující položka...** .

    ![Přidání existující položky do složky obsahu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "přidání existující položky do složky obsahu")

    *Přidání existující položky do složky obsahu*
2. V **přidat existující položku** dialogové okno pole, přejděte **zdroje nebo prostředky** složky a vyberte **Flip.css**. Klikněte na tlačítko **přidat**.

    ![Přidání souboru Flip.css z prostředků](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "přidání souboru Flip.css z prostředků")

    *Přidání souboru Flip.css z prostředků*
3. Otevřete **Flip.css** jste právě přidali soubor a zkontrolujte jeho obsah.
4. Vyhledejte **překlopit transformace** komentář. Styly níže tento komentář pomocí šablon stylů CSS **perspektivy** a **rotateY** transformací pro vygenerování &quot;karty překlopit&quot; vliv.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Vyhledejte **Skrýt zadní podokně během překlopit** komentář. Styl níže tento komentář skryje na druhé straně ploch, když budou jsou od prohlížeč nastavením **backface viditelnost** vlastnost CSS, která má *Skrytá*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Otevřete **BundleConfig.cs** souboru uvnitř **aplikace\_spustit** složky a přidat odkaz na **Flip.css** v soubor  **&quot;~/Content/css&quot;**  styl sady

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Stiskněte klávesu **F5** ke spuštění řešení a přihlaste se pomocí přihlašovacích údajů.
8. Odpověď na otázku kliknutím na jednu z možností. Všimněte si flip účinek při přechodu mezi zobrazení.

    ![Odpovědi na dotaz s flip účinek](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "odpovědi na dotaz s flip účinek")

    *Odpovědi na dotaz s flip účinek*
9. Klikněte na tlačítko **Další otázka** pro načtení následující otázky. Flip účinek by se zobrazit znovu.

    ![Načítání následující dotaz s flip účinek](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "načítání následující dotaz s flip účinek")

    *Následující dotaz s flip účinek načítání*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Pomocí této praktické cvičení jste se naučili, jak:

- Vytvoření řadiče rozhraní ASP.NET Web API pomocí ASP.NET generování uživatelského rozhraní
- Implementace akce získat webové rozhraní API pro načtení další otázky kvízu s časovým limitem
- Provedení akce Post webové rozhraní API pro ukládání odpovědi kvízu s časovým limitem
- Nainstalovat AngularJS z konzoly Správce balíčků Visual Studio
- Implementace AngularJS šablony a řadiče
- Pomocí CSS3 přechody provádět efekty animace
