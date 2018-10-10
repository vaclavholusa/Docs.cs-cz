---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Praktické cvičení: Sestavení jednostránkové aplikaci (SPA) pomocí webového rozhraní API ASP.NET a Angular.js | Dokumentace Microsoftu'
author: rick-anderson
description: Tradiční webových aplikací inicializuje klienta (prohlížeč) komunikaci se serverem můžete si vyžádat stránku. Server zpracuje požadavek...
ms.author: riande
ms.date: 09/30/2015
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 3c3557bb2be2807b11874937fcc629b5b773e463
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912251"
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Praktické cvičení: Sestavení jednostránkové aplikaci (SPA) pomocí webového rozhraní API ASP.NET a Angular.js
====================
podle [Campy Web týmu](https://twitter.com/webcamps)

[Stáhněte si Web Campy školení Kit](http://aka.ms/webcamps-training-kit)

> Tradiční webových aplikací inicializuje klienta (prohlížeč) komunikaci se serverem můžete si vyžádat stránku. Server poté zpracuje žádost a odešle klientovi HTML na stránce. V dalších interakcích se stránkou – například uživatel přejde na odkaz nebo odešle formulář s daty – je nová žádost odeslány na server a znovu spustí tok: server zpracuje žádost a odešle nové stránky do prohlížeče v reakci na žádost o nové akce ED klientem.
> 
> V jednostránkové aplikace (SPA) celý načtení stránky v prohlížeči po počáteční žádosti, ale následné interakce probíhat přes odesílání požadavků Ajax. To znamená, že prohlížeč musí aktualizovat pouze části stránky, které se změnily; není nutné znovu načíst celou stránku. Jednostránková aplikace přístup snižuje doba, za kterou aplikaci reagovat na akce uživatelů, což vede k více plynulé prostředí.
> 
> Architektura SPA zahrnuje některé problémy, které nejsou k dispozici v tradiční webových aplikací. Ale nově vznikající technologie, jako je ASP.NET Web API, například rozhraní JavaScript AngularJS a nový styl funkce poskytované službou CSS3 usnadňují skutečně navrhovat a vytvářet SPA.
> 
> V tomto ručně v testovacím prostředí bude využívat těchto technologií ještě používáte k implementaci Informatik kvíz, triviální prvek Web založený na konceptu jednostránková aplikace. Nejprve budete implementovat vrstvě služby s rozhraním ASP.NET Web API k vystavení požadované koncové body načíst kvíz otázky a odpovědi uložit. Potom sestavíte bohaté a interaktivní uživatelské rozhraní pomocí AngularJS a CSS3 účinky transformace.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Přehled

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V této praktická cvičení se dozvíte, jak:

- Vytvoření služby ASP.NET Web API můžete odesílat a přijímat JSON data
- Vytvoření přizpůsobivého uživatelského rozhraní pomocí AngularJS
- Vylepšete možnosti uživatelského rozhraní CSS3 transformace

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

K dokončení této praktické testovací prostředí jsou vyžadovány následující položky:

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) nebo vyšší

<a id="Setup"></a>
### <a name="setup"></a>Instalace

Chcete-li spustit praktická cvičení v této praktické testovací prostředí, musíte nejdřív nastavit prostředí.

1. Otevřete Průzkumníka Windows a přejděte do testovacího prostředí **zdroj** složky.
2. Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a nainstalujte Visual Studio fragmenty kódu pro toto testovací prostředí.
3. Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, aby bylo možné pokračovat.

> [!NOTE]
> Ujistěte se, že jste zaškrtli všechny závislosti pro toto testovací prostředí před spuštěním instalace.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Používání fragmentů kódu

V celém dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu. Pro usnadnění práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, který se dá dostat z v rámci Visual Studio 2013, abyste ho nemuseli znovu přidat ručně.

> [!NOTE]
> Každý cvičení se sadou počáteční řešení nachází v **začít** složky výkonu, který umožňuje postupovat podle jednotlivých výkon nezávisle na ostatních. Uvědomte si, že chybí z těchto řešení od fragmenty kódu, které se přidávají během cvičení a nemusí fungovat, dokud nedokončíte výkonu. Uvnitř zdrojový kód pro cvičení, můžete také najdete **End** složku, která obsahuje řešení sady Visual Studio s kódem, který je výsledkem dokončení kroků v odpovídající cvičení. Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické vyzkoušení.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické testovací prostředí obsahuje následující praktická cvičení:

1. [Vytvoření webového rozhraní API](#Exercise1)
2. [Vytváří se rozhraní SPA](#Exercise2)

Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**

> [!NOTE]
> Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce. Každé předdefinované kolekce je navržená tak, aby odpovídala konkrétním vývojářským styl a určuje rozložení oken, chování editoru, fragmenty kódu technologie IntelliSense a možnosti dialogového okna. Postupy v tomto testovacím prostředí jsou uvedené akce potřebné k provedení dané úlohy v sadě Visual Studio při použití **obecným vývojovým nastavením** kolekce. Pokud se rozhodnete různá nastavení kolekce pro vaše vývojové prostředí, mohou existovat rozdíly v krocích, které byste měli vzít v úvahu.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Cvičení 1: Vytvoření webového rozhraní API

Jednou z klíčových částí SPA je vrstva služby. Je zodpovědná za zpracování volání Ajax odesílaných uživatelského rozhraní a vrácení dat v reakci na toto volání. Data načtená by se vám ve strojově čitelném formátu, aby bylo možné analyzovat a používat klienta.

Rozhraní Web API je součástí technologie ASP.NET zásobníku a je navržený tak, aby snadno se implementuje služeb HTTP, obecně odesílání a přijímání dat ve formátu JSON nebo XML pomocí rozhraní RESTful API. V tomto cvičení vytvoříte na webu pro hostování aplikace Informatik kvíz a pak implementace služby back-end pro vystavení a uložení dat kvízu s časovým limitem pomocí rozhraní ASP.NET Web API.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Úloha 1 – vytváření původního projektu pro nadšence kvíz

Tato úloha se spustí, vytvoření nového projektu ASP.NET MVC s podporou rozhraní ASP.NET Web API založeného na **One ASP.NET** projektu typu, který je součástí sady Visual Studio. **One ASP.NET** sjednocuje všechny technologie ASP.NET a dává vám možnost kombinovat a párovat podle potřeby. Potom přidáte tříd modelu Entity Framework a initializator databáze k vložení dotazy kvízu s časovým limitem.

1. Otevřít **Visual Studio Express 2013 for Web** a vyberte **soubor | Nový projekt...**  spusťte nové řešení.

    ![Vytvoření nového projektu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "vytvoření nového projektu")

    *Vytvoření nového projektu*
2. V **nový projekt** dialogu **webová aplikace ASP.NET** pod **Visual C# | Web** kartu. Ujistěte se, že **rozhraní .NET Framework 4.5** je vybrána, pojmenujte ho *GeekQuiz*, zvolte **umístění** a klikněte na tlačítko **OK**.

    ![Vytvoření nového projektu webové aplikace ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "vytvoření nového projektu webové aplikace ASP.NET")

    *Vytvoření nového projektu webové aplikace ASP.NET*
3. V **nový projekt ASP.NET** dialogové okno, vyberte **MVC** šablony a vyberte **webového rozhraní API** možnost. Také, ujistěte se, že **ověřování** je možnost nastavená na **jednotlivé uživatelské účty**. Klikněte na tlačítko **OK** pokračujte.

    ![Vytvoření nového projektu pomocí šablony MVC, včetně součástí webového rozhraní API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Vytvoření nového projektu pomocí šablony MVC, včetně součástí webového rozhraní API*
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **modely** složky **GeekQuiz** projektu a vyberte **přidat | Existující položka...** .

    ![Přidání existující položky](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "přidat existující položku")

    *Přidat existující položku*
5. V **přidat existující položku** dialogové okno, přejděte na **zdroj/prostředky/modely** složky a vyberte všechny soubory. Klikněte na tlačítko **přidat**.

    ![Přidání prostředků modelu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "přidání prostředků modelu")

    *Přidání prostředků modelu*

    > [!NOTE]
    > Přidáním těchto souborů přidáváte datový model, Entity Framework kontext databáze a inicializátor databáze pro aplikaci Informatik kvíz.
    > 
    > **Entity Framework (EF)** je objektově relační Mapovač (ORM), který vám umožní vytvářet aplikace přistupující k datům v programování s modelem koncepční aplikace namísto programování přímo pomocí schématu relační úložiště. Další informace o rozhraní Entity Framework [tady](../../../entity-framework.md).
    > 
    > Následuje popis třídy, které jste právě přidali:
    > 
    > - **TriviaOption:** představuje jednu možnost spojené s otázkou kvíz
    > - **TriviaQuestion:** reprezentuje kvíz otázku a zveřejňuje přidružené možností prostřednictvím **možnosti** vlastnost
    > - **TriviaAnswer:** představuje možnost vybraných uživatelem v odpovědi na dotaz kvíz
    > - **TriviaContext:** představuje kontext Entity Framework databáze aplikace Informatik kvíz. Tato třída je odvozena z **DContext** a zveřejňuje **DbSet** vlastnosti, které představují kolekce entit je popsáno výše.
    > - **TriviaDatabaseInitializer:** implementace Entity Framework inicializátor pro **TriviaContext** třída, která dědí z **CreateDatabaseIfNotExists**. Výchozím chováním této třídy je vytvořit databázi jenom v případě, že buď neexistuje, vkládání entity podle **počáteční hodnoty** metody.
6. Otevřít **Global.asax.cs** soubor a přidejte následující příkaz using.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Přidejte následující kód na začátku **aplikace\_Start** metody nastavte **TriviaDatabaseInitializer** jako inicializátor databáze.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Upravit **Domů** kontroleru k omezení přístupu k ověření uživatele. Chcete-li to provést, otevřete **HomeController.cs** soubor uvnitř **řadiče** složky a přidejte **Authorize** atribut **HomeController**definici třídy.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > **Authorize** filtrovat kontroluje, pokud je uživatel ověřený. Pokud uživatel není ověřen, vrátí stavový kód HTTP 401 (Neautorizováno) bez vyvolání akce. Můžete použít filtr na úrovni kontroleru globálně, nebo na úrovni jednotlivých akcí.
9. Teď budete přizpůsobovat rozložení webové stránky a značce. Chcete-li to provést, otevřete  **\_Layout.cshtml** uvnitř souboru **zobrazení | Sdílené** složky a aktualizovat obsah **&lt;title&gt;** element tak, že nahradíte *My ASP.NET Application* s *kvíz Informatik* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. Ve stejném souboru, aktualizovat tak, že odeberete na navigačním panelu *o* a *kontakt* odkazy a přejmenování *Domů* propojit *Přehrát*. Kromě toho přejmenovat *název aplikace* propojit *kvíz Informatik*. Kód HTML pro navigační panel by měl vypadat jako v následujícím kódu.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Aktualizace v zápatí je uvedené na stránce rozložení tak, že nahradíte *My ASP.NET Application* s *kvíz Informatik*. Chcete-li to provést, nahradí obsah **&lt;zápatí&gt;** element s následující zvýrazněný kód.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Úloha 2 – Vytvoření TriviaController webového rozhraní API

V předchozí úloze vytvoření počáteční struktury webové aplikace než kvíz. Teď vytvoříte jednoduchou službou webového rozhraní API, která komunikuje s datovým modelem kvíz a zveřejňuje následující akce:

- **GET/api/triviální prvek**: načte další otázku ze seznamu kvíz k zodpovězení od ověřeného uživatele.
- **PŘÍSPĚVEK/api/triviální prvek**: uloží odpovědi kvízu s časovým limitem určené ověřeného uživatele.

ASP.NET generování uživatelského rozhraní nástroje poskytovaný sadou Visual Studio použije k vytvoření standardních hodnot pro třídu kontroleru webového rozhraní API.

1. Otevřít **WebApiConfig.cs** soubor uvnitř **aplikace\_Start** složky. Tento soubor definuje konfiguraci služby webového rozhraní API, jako jsou mapovány směrování na akce kontroleru webového rozhraní API.
2. Přidejte následující příkaz using na začátku souboru.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Přidejte následující zvýrazněný kód do **zaregistrovat** metoda globálně konfigurace formátování data JSON pomocí metody akce webového rozhraní API.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver** automaticky převádí názvy vlastností, abyste *camel* případu, kdy je obecné vytváření názvů pro názvy vlastností v jazyce JavaScript.
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **řadiče** složky **GeekQuiz** projektu a vyberte **přidat | Nová vygenerovaná položka...** .

    ![Vytváří se nová vygenerovaná položka](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "vytváří se nová vygenerovaná položka")

    *Vytváří se nová vygenerovaná položka*
5. V **přidat vygenerované uživatelské rozhraní** dialogové okno pole, ujistěte se, že **běžné** při výběru uzlu v levém podokně. Vyberte **kontroler rozhraní Web API 2 – prázdný** šablon v prostředním podokně a klepněte na **přidat**.

    ![Výběr šablony webové rozhraní API 2 řadiče prázdný](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "výběr webového rozhraní API 2 řadiče prázdné šablony")

    *Výběr webové rozhraní API 2 řadiče prázdné šablony*

    > [!NOTE]
    > **Generování uživatelského rozhraní ASP.NET** je architektura generování kódu pro webové aplikace ASP.NET. Visual Studio 2013 obsahuje generátory předinstalované kódu pro projekty MVC a webového rozhraní API. Pokud chcete rychle přidat kód, který komunikuje s datovými modely za účelem snížení množství času potřebné k vývoji standardních datových operací, používejte generování uživatelského rozhraní ve vašem projektu.
    > 
    > Proces generování uživatelského rozhraní také zajišťuje, že jsou nainstalované všechny požadované závislosti v projektu. Například pokud začínat prázdný projekt ASP.NET a pak přidat kontroler Web API pomocí generování uživatelského rozhraní, požadované balíčky NuGet webové rozhraní API a odkazy jsou přidány do projektu automaticky.
6. V **přidat kontroler** dialogovém okně *TriviaController* v **názvu Kontroleru** textového pole a klikněte na tlačítko **přidat**.

    ![Přidání Kontroleru triviální prvek](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "přidání Kontroleru triviální prvek")

    *Přidání Kontroleru triviální prvek*
7. **TriviaController.cs** souboru se pak přidá do **řadiče** složky **GeekQuiz** projektu, který obsahuje prázdnou **TriviaController** třídy. Přidejte následující příkazy using na začátku souboru.

    (Fragment - kódu *AspNetWebApiSpa - Ex1 - TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Přidejte následující kód na začátku **TriviaController** tříd k definování, inicializovat a uvolnění **TriviaContext** instance v kontroleru.

    (Fragment - kódu *AspNetWebApiSpa - Ex1 - TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > **Dispose** metoda **TriviaController** vyvolá **Dispose** metodu **TriviaContext** instanci, která zajistí, že všechny prostředky využívané třídou objektu context se vydávají při **TriviaContext** instance je odstraněn nebo uvolnění paměti. Jedná se o zavření všech připojení k databázi otevřít rozhraním Entity Framework.
9. Přidejte následující pomocnou metodu na konci **TriviaController** třídy. Tato metoda načte z databáze k zodpovězení zadaným uživatelem na následující otázku kvízu s časovým limitem.

    (Fragment - kódu *AspNetWebApiSpa - Ex1 - TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Přidejte následující **získat** metoda akce, která **TriviaController** třídy. Volá tuto metodu akce **NextQuestionAsync** Pomocná metoda, definována v předchozím kroku, a načíst další otázka pro ověřeného uživatele.

    (Fragment - kódu *AspNetWebApiSpa - Ex1 - TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Přidejte následující pomocnou metodu na konci **TriviaController** třídy. Tato metoda ukládá zadané odpovědí do databáze a vrátí logickou hodnotu označující, zda je odpověď správná.

    (Fragment - kódu *AspNetWebApiSpa - Ex1 - TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Přidejte následující **příspěvek** metoda akce, která **TriviaController** třídy. Tato metoda akce přidruží odpověď na ověřeného uživatele a volání **StoreAsync** Pomocná metoda. Pak odešle odpověď se logická hodnota vrácená metodou helper.

    (Fragment - kódu *AspNetWebApiSpa - Ex1 - TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Změna kontroleru webového rozhraní API k omezení přístupu k ověřeným uživatelům tak, že přidáte **Authorize** atribut **TriviaController** definici třídy.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Úloha 3 – spuštění řešení

V této úloze ověříte, že služba webového rozhraní API, kterou jste vytvořili v předchozí úloze funguje podle očekávání. Budete používat Internet Explorer **vývojářské nástroje F12 pomáhají** pro zachycení síťového provozu a zkontrolujte úplnou odpověď ze služby webového rozhraní API.

> [!NOTE]
> Ujistěte se, že **aplikace Internet Explorer** výběru v **Start** tlačítka umístěny na panelu nástrojů sady Visual Studio.
> 
> ![Možnosti aplikace Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Stisknutím klávesy **F5** ke spuštění řešení. **Přihlášení** stránka by se měla zobrazit v prohlížeči.

    > [!NOTE]
    > Při spuštění aplikace se aktivuje výchozí trasa MVC, která ve výchozím nastavení se mapuje na **Index** akce **HomeController** třídy. Protože **HomeController** omezen ověřeným uživatelům (mějte na paměti, že dekorované třídy s **Authorize** atribut v cvičení 1) a neexistuje žádný ověřený uživatel, ale aplikace původní požadavek přesměruje na přihlašovací stránce.

    ![Spuštění řešení](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "spuštění řešení")

    *Spuštění řešení*
2. Klikněte na tlačítko **zaregistrovat** k vytvoření nového uživatele.

    ![Registrace nového uživatele](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "registraci nového uživatele")

    *Registrace nového uživatele*
3. V **zaregistrovat** stránky, zadejte **uživatelské jméno** a **heslo**a potom klikněte na tlačítko **zaregistrovat**.

    ![Stránka pro registraci](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "stránku registrace")

    *Stránka pro registraci*
4. Aplikace registruje nový účet a uživatel je ověřený a přesměrován zpět na domovskou stránku.

    ![Ověření uživatele](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "uživatel byl ověřen")

    *Ověření uživatele*
5. V prohlížeči stiskněte **F12** otevřít **vývojářské nástroje** panelu. Stisknutím klávesy **CTRL + 4** nebo klikněte na tlačítko **sítě** ikony a pak klikněte na tlačítko začít zachytávání síťového provozu na zelenou šipku.

    ![Inicializace webového rozhraní API zachytávání sítě](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "zachytávání sítě inicializaci webového rozhraní API")

    *Spouštění Zachytávání síťových rozhraní Web API*
6. Připojit **rozhraní api/triviální prvek** na adresu URL v adresním řádku prohlížeče. Nyní zkontrolujete podrobnosti odpovědi **získat** metodu akce v **TriviaController**.

    ![Načítají se další otázku data prostřednictvím webového rozhraní API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "načítání další otázku dat prostřednictvím webového rozhraní API")

    *Načítají se další otázku data prostřednictvím webového rozhraní API*

    > [!NOTE]
    > Jakmile se stahování dokončí, vyzve k akci udělat pomocí staženého souboru. Ponechte dialogové okno otevřené, aby bylo možné sledovat obsah odpovědi prostřednictvím panel nástrojů pro vývojáře.
7. Nyní bude kontrolovat textu odpovědi. Chcete-li to provést, klikněte na tlačítko **podrobnosti** kartu a potom klikněte na tlačítko **tělo odpovědi**. Můžete zkontrolovat, že stažených dat je objekt s vlastnostmi **možnosti** (což je seznam **TriviaOption** objekty), **id** a **title** , které odpovídají **TriviaQuestion** třídy.

    ![Zobrazení textu odpovědi rozhraní API webové](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "tělo odpovědi rozhraní API webové zobrazení")

    *Text odpovědi rozhraní API webové zobrazení*
8. Vraťte se zpět do sady Visual Studio a stiskněte klávesu **SHIFT + F5** chcete zastavit ladění.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Cvičení 2: Vytvoření rozhraní SPA

V tomto cvičení nejprve vytvoříte webového front-endu část Informatik kvíz, zaměřuje se na interakci jednostránkové aplikace pomocí **AngularJS**. Potom vylepší uživatelské prostředí s CSS3 bohaté animace a poskytují vizuální efekt kontextu přepínání při přesunu z jedné otázky na další.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Úloha 1 – vytváření rozhraní jednostránková aplikace pomocí AngularJS

V této úloze budete používat **AngularJS** implementovat Informatik kvíz aplikace na straně klienta. **AngularJS** je open source platforma jazyka JavaScript, která rozšiřuje aplikace využívající prohlížeč *Model-View-Controller* (MVC) funkce, které usnadňují i vývoj a testování.

Začněte instalací AngularJS z konzoly Správce balíčků sady Visual Studio. Potom vytvoříte řadič poskytnout chování aplikace Informatik kvíz a zobrazení k vykreslení kvíz otázky a odpovědi pomocí šablony modulu AngularJS.

> [!NOTE]
> Další informace o AngularJS [ [ http://angularjs.org/ ](http://angularjs.org/) ](http://angularjs.org/).


1. Otevřete **Visual Studio Express 2013 for Web** a otevřete **GeekQuiz.sln** řešení nachází v **zdroj/Ex2-CreatingASPAInterface/Begin** složky. Alternativně můžete pokračovat v řešení, který jste získali v předchozím cvičení.
2. Otevřít **Konzola správce balíčků** z **nástroje** > **Správce balíčků NuGet**. Zadejte následující příkaz k instalaci **AngularJS.Core** balíček NuGet.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **skripty** složky **GeekQuiz** projektu a vyberte **přidat | Nová složka**. Název složky **aplikace** a stiskněte klávesu **Enter**.
4. Klikněte pravým tlačítkem myši **aplikace** složky, které jste právě vytvořili a vyberte **přidat | Soubor JavaScript**.

    ![Vytváří se nový soubor JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Vytváří se nový soubor JavaScript*
5. V **zadat název pro položku** dialogovém okně *řadič testu* v **název položky** textového pole a klikněte na tlačítko **OK**.

    ![Pojmenování nového souboru JavaScriptu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Pojmenování nového souboru JavaScriptu*
6. V **kvíz controller.js** přidejte následující kód, který deklarujete a inicializujete AngularJS **QuizCtrl** kontroleru.

    (Fragment - kódu *AspNetWebApiSpa - Ex2 - AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > Funkce konstruktoru **QuizCtrl** řadič očekává parametr injekčních s názvem **$scope**. Počáteční stav oboru musí být nastaveno v funkce konstruktoru připojením vlastnosti, které chcete **$scope** objektu. Všechny vlastnosti obsahují **model zobrazení**a budou mít přístup k šabloně při registraci kontroleru.
    > 
    > **QuizCtrl** kontroleru je definována uvnitř modulu s názvem **QuizApp**. Moduly jsou jednotky práce, které vám umožňují rozdělit na samostatné součásti vaší aplikace. Hlavní výhody používání modulů je, že kód je srozumitelnější a usnadňuje testování částí a opětovné použití, udržovatelnost.
7. Aby bylo možné reagovat na události aktivované ze zobrazení nyní přidejte chování oboru. Přidejte následující kód na konci **QuizCtrl** kontroleru k definování **nextQuestion** fungovat v **$scope** objektu.

    (Fragment - kódu *AspNetWebApiSpa - Ex2 - AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Tato funkce načte další dotaz z **triviální prvek** webového rozhraní API vytvořili v předchozím cvičení a připojí data otázku, která mají **$scope** objektu.
8. Vložte následující kód na konci **QuizCtrl** kontroleru k definování **sendAnswer** fungovat v **$scope** objektu.

    (Fragment - kódu *AspNetWebApiSpa - Ex2 - AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Tato funkce odesílá odpovědi vybrané uživatelem **triviální prvek** webového rozhraní API a ukládá výsledek – tj. Pokud je odpověď nebo není správná – **$scope** objektu.
    > 
    > **NextQuestion** a **sendAnswer** používají funkce výše AngularJS **$http** objektu abstraktní komunikaci s webovým rozhraním API prostřednictvím XMLHttpRequest Objekt jazyka JavaScript v prohlížeči. AngularJS podporuje jiné službě, která přináší vyšší úroveň abstrakce pro provádění operací CRUD, s prostředkem prostřednictvím rozhraní REST API. AngularJS **$resource** objekt má metody akce, které poskytují základní chování bez nutnosti pracovat **$http** objektu. Zvažte použití **$resource** objektu ve scénářích, která vyžaduje CRUD model (fore informace najdete v článku [$resource dokumentaci](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Dalším krokem je vytvoření šablony AngularJS, která definuje zobrazení kvíz. Chcete-li to provést, otevřete **Index.cshtml** uvnitř souboru **zobrazení | Domů** složky a nahraďte obsah následujícím kódem.

    (Fragment - kódu *AspNetWebApiSpa - Ex2 - GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > AngularJS šablony je deklarativní specifikace, která používá informace z modelu a řadičem pro transformaci statický kód do dynamického zobrazení, která uživateli se zobrazí v prohlížeči. Následují příklady AngularJS elementy a atributy prvků, které lze použít v šabloně:
    > 
    > - **Ng-app** – direktiva říká AngularJS prvek modelu DOM, který představuje kořenový prvek oddílu aplikace.
    > - **Ng-controller** – direktiva připojí jako řadič v modelu DOM v okamžiku, kdy je deklarována jako direktiva.
    > - Zápis složenou závorku **{{}}** označuje vazby na vlastnosti oboru definované v kontroleru.
    > - **Ng kliknutím** – direktiva je použito k volání funkce definované v oboru v reakci na kliknutí.
10. Otevřít **Site.css** soubor uvnitř **obsahu** složky a přidejte následující zvýrazněný styly na konci souboru, který má poskytnout vzhled a chování pro zobrazení kvízu s časovým limitem.

    (Fragment - kódu *AspNetWebApiSpa - Ex2 - GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Úloha 2 – spuštění řešení

Řešení pomocí nového uživatele v této úloze bude vykonán rozhraní jste vytvořili pomocí AngularJS, abyste získali odpovědi na otázky kvízu s časovým limitem.

1. Stisknutím klávesy **F5** ke spuštění řešení.
2. Zaregistrujte nový uživatelský účet. Chcete-li to provést, postupujte podle kroků registrace je popsáno v cvičení 1, úloze 3.

    > [!NOTE]
    > Pokud používáte řešení v předchozím cvičení, přihlaste se na uživatelský účet, který jste vytvořili před.
3. **Domů** by se měla zobrazit stránka zobrazující první otázku kvíz. Odpovězte na otázku, kliknutím na jednu z možností. Tím se aktivuje **sendAnswer** definovali dříve, funkce, která odešle vybraný možnost **triviální prvek** webového rozhraní API.

    ![Odpovídání na otázku](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "odpovídání na otázku")

    *Odpovídání na otázku*
4. Po kliknutí na jedno z tlačítek, by se zobrazit odpověď. Klikněte na tlačítko **další otázku** zobrazíte na následující otázku. Tím se aktivuje **nextQuestion** funkce definované v kontroleru.

    ![Požadování další otázku](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "požadování další otázku")

    *Požadování další otázku*
5. By se zobrazit další dotaz. Dál zodpovězení otázek tolikrát, kolikrát chcete. Po dokončení všechny otázky, měli byste vrátit na první otázku.

    ![Další otázkou](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "další otázku")

    *Další otázka*
6. Vraťte se zpět do sady Visual Studio a stiskněte klávesu **SHIFT + F5** chcete zastavit ladění.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Úloha 3 – vytvoření překlopit animace CSS3

V této úloze se pomocí vlastnosti CSS3 provádět bohaté animace přidáním překlopit efekt, když bude dotaz zodpovězen a když se načte další dotaz.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **obsahu** složky **GeekQuiz** projektu a vyberte **přidat | Existující položka...** .

    ![Přidání existující položky do složky obsahu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "přidání existující položky do složky obsahu")

    *Přidání existující položky do složky obsahu*
2. V **přidat existující položku** dialogové okno, přejděte na **zdroj/prostředky** a pak zvolte položku **Flip.css**. Klikněte na tlačítko **přidat**.

    ![Přidání souboru Flip.css z prostředků](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "přidávání souboru Flip.css z prostředků")

    *Přidání souboru Flip.css z prostředků*
3. Otevřít **Flip.css** soubor jste právě přidali a zkontrolujte jeho obsah.
4. Vyhledejte **překlopit transformace** komentář. Styly níže daný komentář pomocí šablon stylů CSS **perspektivy** a **rotateY** transformací pro generování &quot;karty vyfiltrují&quot; vliv.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Vyhledejte **Skrýt zadní podokně během vyfiltrují** komentář. Styl níže daný komentář skryje back-side tváří, když jsou směřující od prohlížeč tak, že nastavíte **odstranění odvrácených viditelnost** vlastnost CSS *skryté*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Otevřít **BundleConfig.cs** soubor uvnitř **aplikace\_Start** složky a přidejte odkaz na **Flip.css** ve **&quot;~/Content/css&quot;** sady prostředků stylu

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Stisknutím klávesy **F5** ke spouštění řešení a přihlaste se pomocí svých přihlašovacích údajů.
8. Odpověď na otázku kliknutím na jednu z možností. Všimněte si, že překlopit efekt při přesunu mezi zobrazeními.

    ![Odpovědi na dotaz s použitím překlopit efekt](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "odpovídání na otázku pomocí překlopit efekt")

    *Odpovědi na dotaz s použitím překlopit efekt*
9. Klikněte na tlačítko **další otázku** načíst na následující otázku. Překlopit efekt objevit znovu.

    ![Načítají se Převrátit vliv na následující otázku](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "načítání s překlopit vliv na následující otázku")

    *Načítají se na následující otázku s překlopit efekt*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Po dokončení tohoto praktických cvičení jste se naučili, jak:

- Vytvoření webového rozhraní API ASP.NET řadiče pomocí technologie ASP.NET generování uživatelského rozhraní
- Provedení akce webové rozhraní API Get pro načtení další otázky kvíz
- Provedení akce webové rozhraní API příspěvku k uložení odpovědi kvízu
- Nainstalujte AngularJS v konzole Správce balíčků Visual Studio
- Implementace AngularJS šablony a kontrolerů
- Použít k provádění efekty animace CSS3 přechody
