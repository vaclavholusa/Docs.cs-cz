---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: Vysoká frekvence v reálném čase s SignalR 1.x | Microsoft Docs
author: pfletcher
description: Tento kurz ukazuje, jak vytvořit webovou aplikaci, která používá funkce SignalR technologie ASP.NET poskytuje funkce zasílání zpráv vysoká frekvence. Vysoká frekvence zasílání zpráv v...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/16/2013
ms.topic: article
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0c680a7d8b911b2734647948b683d5ff6e47aec4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="high-frequency-realtime-with-signalr-1x"></a>Vysoká frekvence v reálném čase s SignalR 1.x
====================
podle [Patrik Fletcher](https://github.com/pfletcher)

> Tento kurz ukazuje, jak vytvořit webovou aplikaci, která používá funkce SignalR technologie ASP.NET poskytuje funkce zasílání zpráv vysoká frekvence. Vysoká frekvence zasílání zpráv v tomto případě znamená aktualizace, které se odesílají s pevnou sazbou; v případě této aplikace zpráv až 10 sekund.
> 
> Aplikace, kterou budete v tomto kurzu vytvoříte zobrazí tvar, který můžete přetáhnout uživatele. Pozice tvaru ve všech ostatních připojených prohlížečích aktualizovány tak, aby odpovídaly místo taženou tvaru pomocí aktualizací vypršel.
> 
> Koncepty představené v tomto kurzu jste aplikací v reálném čase herní nebo jiných aplikací simulace.
> 
> Komentáře v tomto kurzu jsou úvodní. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak vytvořit aplikaci, který sdílí stav objektu jiných prohlížečů v reálném čase. Aplikace, kterou vytvoříme nazývá MoveShape. Na stránce MoveShape se zobrazí element Div jazyka HTML, který uživatel můžete přetáhnout; Když uživatel nastavuje tažením Div, nové místo odešle na server, který bude potom informovat, všechny ostatní připojené klienty aktualizovat umístění tak, aby odpovídaly.

![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Aplikace vytvořené v tomto kurzu je založena na ukázku Damianu Edwards. Video obsahující tuto ukázku si můžete prohlédnout [zde](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Kurz se spustí ve který ukazuje, jak k odeslání zprávy SignalR z každé události, které se zahájí, jako je přetáhnout tvaru. Každý připojený klient pak bude aktualizovat umístění v místní verzi tvaru pokaždé, když je přijata zpráva.

Když aplikace bude fungovat touto metodou, to není doporučené programovací model, vzhledem k tomu, že by existovat žádné horní limit pro počet zpráv, získávání odeslat, aby klienti a server může získat zahlcen zprávy a by snížení výkonu . Zobrazené animace v klientovi by také odděleném pohybu tvar, který by být okamžitě každý metodou spíše než přesunutí plynule do každého nového umístění. V dalších částech tohoto kurzu bude ukazují, jak vytvořit časovač funkci, která omezuje maximální rychlost, jakou jsou odesílány zprávy klienta nebo serveru a postup přesunutí tvaru plynule mezi umístěními. Finální verzi aplikace vytvořené v tomto kurzu lze stáhnout z [galerie kódů](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Tento kurz obsahuje následující části:

- [Požadavky](#prerequisites)
- [Vytvoření projektu](#createtheproject)
- [Přidání balíčků funkce SignalR technologie ASP.NET a JQuery.UI NuGet](#nugetpackages)
- [Vytvoření základní aplikace](#baseapp)
- [Přidat klienta smyčky](#clientloop)
- [Přidat server smyčky](#serverloop)
- [Přidat smooth animace na straně klienta](#animation)
- [Další kroky](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Požadavky

Tento kurz vyžaduje Visual Studio 2012 nebo Visual Studio 2010. Pokud se používá Visual Studio 2010, projekt bude použijte rozhraní .NET Framework 4, nikoli rozhraní .NET Framework 4.5.

Pokud používáte Visual Studio 2012, doporučujeme nainstalovat [ASP.NET a webové nástroje 2012.2 aktualizace](https://go.microsoft.com/fwlink/?LinkId=282650). Tato aktualizace obsahuje nové funkce, jako je například vylepšení publikování, nové funkce a nové šablony.

Pokud máte Visual Studio 2010, ujistěte se, že [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) je nainstalovaná.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Vytvoření projektu

V této části vytvoříme projektu v sadě Visual Studio.

1. Z **soubor** nabídce klikněte na tlačítko **nový projekt**.
2. V **nový projekt** dialogové okno, rozbalte seznam **C#** pod **šablony** a vyberte **webové**.
3. Vyberte **prázdný webové aplikace ASP.NET** šablony, název projektu *MoveShapeDemo*a klikněte na tlačítko **OK**.

    ![Vytvoření nového projektu](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Přidat SignalR a balíčky JQuery.UI NuGet

Funkce SignalR můžete přidat do projektu pomocí instalace balíčku NuGet. V tomto kurzu bude balíček JQuery.UI použít také pro povolení tvar, který má být přetažen a animovaný.

1. Klikněte na tlačítko **nástroje | Správce balíčků knihoven | Konzola správce balíčků**.
2. Zadejte následující příkaz v Správce balíčků.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    SignalR balíček nainstaluje několik dalších balíčcích NuGet jako závislosti. Po dokončení instalace budete mít vše serveru a klienta součásti požadované pro použití v aplikaci ASP.NET SignalR.
3. Zadejte následující příkaz do konzoly Správce balíčků k instalaci balíčků JQuery a JQuery.UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Vytvoření základní aplikace

V této části vytvoříme prohlížeče aplikace, která odesílá umístění tvaru na server během každou událost pohybu myší. Přijetí, všesměrově server pak tyto informace do všech ostatních připojených klientů. Jsme budete rozšířit do této aplikace v pozdějších částech.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat**, **třídy...** . Název třídy **MoveShapeHub** a klikněte na tlačítko **přidat**.
2. Nahraďte kód v novém **MoveShapeHub** třídy následujícím kódem.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    `MoveShapeHub` Výše třída je implementace rozbočovače SignalR. Jako v [Začínáme s SignalR](index.md) kurzu rozbočovače má metodu, která klienti bude volat přímo. V tomto případě klient odešle objekt, který obsahuje nový tvar, který má na server, který pak získá vysílali pro všechny ostatní připojené klienty souřadnice X a Y. SignalR bude automaticky serializovat tento objekt, který používá JSON.

    Objekt, který odešle klientovi (`ShapeModel`) obsahuje členy k uložení pozice tvaru. Verze objektu na serveru také obsahuje členem, abyste mohli sledovat data klientů, které ukládají, tak, aby daného klienta nebude odeslána svá vlastní data. Tento člen používá `JsonIgnore` atribut zabránit jeho se odešle do klienta a serializovat.
3. V dalším kroku nastavíme rozbočovače při spuštění aplikace. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Třída globální aplikace**. Přijměte výchozí název *globální* a klikněte na tlačítko **OK**.

    ![Přidání třídy globální aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Přidejte následující `using` příkaz po poskytnutého **pomocí** příkazy ve třídě Global.asax.cs.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Přidejte následující řádek kódu `Application_Start` metoda globální třídy k registraci výchozí trasu pro SignalR.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Váš soubor global.asax by měl vypadat asi takto:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Potom přidáme klienta. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Nová položka**. V **přidat novou položku** dialogovém okně, vyberte **stránku Html**. Stránky zadejte vhodný název (jako je **Default.html**) a klikněte na tlačítko **přidat**.
7. V **Průzkumníku řešení**, klikněte pravým tlačítkem na stránku, kterou jste právě vytvořili a klikněte na **nastavit jako úvodní stránku**.
8. Nahraďte kód výchozí stránku HTML následující fragment kódu.

    > [!NOTE]
    > Ověřte, zda skript odkazuje níže shodu balíčky přidat do projektu ve složce skripty. V sadě Visual Studio 2010 nemusí odpovídat verzi JQuery a SignalR k projektu nepřidají níže číslo verze.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Ve výše uvedeném kódu HTML a JavaScript vytvoří red Div názvem tvaru, umožňuje tvaru přetahování chování pomocí knihovny jQuery a používá tvaru `drag` událost, která má odeslat umístění na server.
9. Spusťte aplikaci stisknutím klávesy F5. Zkopírujte adresu URL stránky a vložte jej do druhé okno prohlížeče. Přetáhněte obrazec v jednom z okna prohlížeče; Přesuňte tvaru v jiných okna prohlížeče.

    ![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Přidat klienta smyčky

Vzhledem k tomu, že odesílání umístění obrazce na každou událost pohybu myší vytvoří nepotřebných množství síťový provoz, je potřeba omezeny zprávy z klienta. Použijeme javascript `setInterval` funkce nastavit smyčku, která odesílá informace o novém umístění na server s pevnou sazbou. Tato smyčka je velmi základní reprezentace "herní smyčka", opakovaně zavolat funkci, která jednotky všechny funkce hry nebo jiné simulace.

1. Aktualizujte kód klienta na stránce HTML tak, aby odpovídala následující fragment kódu.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    Výše uvedené aktualizace přidá `updateServerModel` funkce, který je volán na pevnou frekvencí. Tato funkce odesílá umístění dat na server vždy, když `moved` příznak znamená, že existuje nové umístění dat k odeslání.
2. Spusťte aplikaci stisknutím klávesy F5. Zkopírujte adresu URL stránky a vložte jej do druhé okno prohlížeče. Přetáhněte obrazec v jednom z okna prohlížeče; Přesuňte tvaru v jiných okna prohlížeče. Vzhledem k tomu, že počet zpráv, které získat odeslány na server budou omezeny, nezobrazí se jako plynulé jako v předchozí části animace.

    ![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Přidat server smyčky

V aktuální aplikaci zprávy odeslané ze serveru klientovi přejděte často příjmu. To představuje podobný problém, jak je vidět na straně klienta; častěji, než jsou potřeba, a připojení může být přenášeny proto nelze odesílat zprávy. Tato část popisuje postup aktualizace serveru pro implementaci časovač, který omezí generovaný počet odchozích zpráv.

1. Nahraďte obsah `MoveShapeHub.cs` následujícím fragmentem kódu.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Výše uvedený kód rozšiřuje klienta přidat `Broadcaster` třídy, která omezí generovaný odchozích zpráv pomocí `Timer` třídy z rozhraní .NET framework.

    Vzhledem k tomu, že je přechodné rozbočovače, samotné (vytváří se pokaždé, když je to potřeba), `Broadcaster` bude vytvořen jako typ singleton. Opožděná inicializace (dostupné v rozhraní .NET 4) se používá k odložení svého vytvoření, dokud nebude potřeba, zajistíte, aby první instanci rozbočovače úplně vytvořený před zahájením časovač.

    Volání klientů `UpdateShape` funkce je pak přesunout mimo rozbočovače na `UpdateModel` metody, které se nazývá už okamžitě vždy, když jsou přijaty příchozí zprávy. Místo toho budou odeslány zprávy, které mají klienty s rychlostí 25 volání za sekundu, spravuje `_broadcastLoop` časovače uvnitř `Broadcaster` třídy.

    Nakonec namísto volání metody klienta od rozbočovače přímo, `Broadcaster` třída musí získat odkaz na aktuálně operačního centra (`_hubContext`) pomocí `GlobalHost`.
2. Spusťte aplikaci stisknutím klávesy F5. Zkopírujte adresu URL stránky a vložte jej do druhé okno prohlížeče. Přetáhněte obrazec v jednom z okna prohlížeče; Přesuňte tvaru v jiných okna prohlížeče. Nebudou viditelné rozdíl v prohlížeči z předchozí části, ale počet zpráv, které získat odeslat klientovi, budou omezeny.

    ![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Přidat smooth animace na straně klienta

Aplikace je téměř dokončena, ale může uděláme jeden další vylepšení v pohybu tvaru v klientovi pohybu v odpovědi na zprávy serveru. Místo nastavení polohy tvaru do nového umístění zadané serverem, použijeme knihovna uživatelského rozhraní JQuery `animate` funkce přesunutí obrazce plynule mezi aktuálních a nových pozici.

1. Aktualizace klienta `updateShape` metodu vypadat jako následující zvýrazněný kód:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Ve výše uvedeném kódu přesune tvaru z původního umístění do nového zadaný server v průběhu intervalu animace (v tomto případě 100 milisekund). Všechny předchozí animace spuštěna na tvar, který není zaškrtnuto, před zahájením nové animace.
2. Spusťte aplikaci stisknutím klávesy F5. Zkopírujte adresu URL stránky a vložte jej do druhé okno prohlížeče. Přetáhněte obrazec v jednom z okna prohlížeče; Přesuňte tvaru v jiných okna prohlížeče. Přesun tvaru v dalším okně by se zobrazit méně trhané, jako je jeho přesunu interpolované přes čas a nikoli Probíhá nastavení jednou za příchozí zprávy.

    ![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Další kroky

V tomto kurzu když jste se naučili programování aplikace SignalR, která odesílá zprávy vysoká frekvence mezi klienty a servery. Tato komunikace zlepší je užitečné pro vývoj online her a dalších simulace, například [herní ShootR vytvořili pomocí nástroje SignalR](http://shootr.signalr.net).

Kompletní aplikace vytvořené v tomto kurzu lze stáhnout z [galerie kódů](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Další informace o konceptech vývoj SignalR, najdete na následujících stránkách pro SignalR zdrojového kódu a prostředky:

- [Projekt SignalR](http://signalr.net)
- [SignalR Githubu a ukázky](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
