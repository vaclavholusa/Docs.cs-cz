---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: "Kurz: Vysoká frekvence v reálném čase s SignalR 2 | Microsoft Docs"
author: pfletcher
description: "Tento kurz ukazuje, jak vytvořit webovou aplikaci, která používá funkce SignalR technologie ASP.NET poskytuje funkce zasílání zpráv vysoká frekvence. Vysoká frekvence zasílání zpráv v..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ab051b2ab85d1aac1e7179f342f22f470b1d1cc7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>Kurz: Vysoká frekvence v reálném čase s SignalR 2
====================
podle [Patrik Fletcher](https://github.com/pfletcher)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> Tento kurz ukazuje, jak vytvořit webovou aplikaci, která používá ASP.NET SignalR 2 vysoká frekvence funkce zasílání zpráv. Vysoká frekvence zasílání zpráv v tomto případě znamená aktualizace, které se odesílají s pevnou sazbou; v případě této aplikace zpráv až 10 sekund.
> 
> Aplikace, kterou budete v tomto kurzu vytvoříte zobrazí tvar, který můžete přetáhnout uživatele. Pozice tvaru ve všech ostatních připojených prohlížečích aktualizovány tak, aby odpovídaly místo taženou tvaru pomocí aktualizací vypršel.
> 
> Koncepty představené v tomto kurzu jste aplikací v reálném čase herní nebo jiných aplikací simulace.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR verze 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Tento kurz pomocí sady Visual Studio 2012
> 
> 
> Pokud chcete používat Visual Studio 2012 v tomto kurzu, postupujte takto:
> 
> - Aktualizace vašeho [Správce balíčků](http://docs.nuget.org/docs/start-here/installing-nuget) na nejnovější verzi.
> - Nainstalujte [webové platformy](https://www.microsoft.com/web/downloads/platform.aspx).
> - Ve webové platformy, vyhledání a instalace **ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012**. Šablony sady Visual Studio pro třídy SignalR dojde k instalaci, jako **rozbočovače**.
> - Některé šablony (jako například **třídy pro spuštění OWIN**) nebudou k dispozici; pro tyto třídy soubor použít místo.
> 
> 
> ## <a name="tutorial-versions"></a>Kurz verze
> 
> Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak vytvořit aplikaci, který sdílí stav objektu jiných prohlížečů v reálném čase. Aplikace, kterou vytvoříme nazývá MoveShape. Na stránce MoveShape se zobrazí element Div jazyka HTML, který uživatel můžete přetáhnout; Když uživatel nastavuje tažením Div, nové místo odešle na server, který bude potom informovat, všechny ostatní připojené klienty aktualizovat umístění tak, aby odpovídaly.

![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Aplikace vytvořené v tomto kurzu je založena na ukázku Damianu Edwards. Video obsahující tuto ukázku si můžete prohlédnout [zde](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Kurz se spustí ve který ukazuje, jak k odeslání zprávy SignalR z každé události, které se zahájí, jako je přetáhnout tvaru. Každý připojený klient pak bude aktualizovat umístění v místní verzi tvaru pokaždé, když je přijata zpráva.

Když aplikace bude fungovat touto metodou, to není doporučené programovací model, vzhledem k tomu, že by existovat žádné horní limit pro počet zpráv, získávání odeslat, aby klienti a server může získat zahlcen zprávy a by snížení výkonu . Zobrazené animace v klientovi by také odděleném pohybu tvar, který by být okamžitě každý metodou spíše než přesunutí plynule do každého nového umístění. V dalších částech tohoto kurzu bude ukazují, jak vytvořit časovač funkci, která omezuje maximální rychlost, jakou jsou odesílány zprávy klienta nebo serveru a postup přesunutí tvaru plynule mezi umístěními. Finální verzi aplikace vytvořené v tomto kurzu lze stáhnout z [galerie kódů](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Tento kurz obsahuje následující části:

- [Požadavky](#prerequisites)
- [Vytvořte projekt a přidejte balíček SignalR a JQuery.UI NuGet](#createtheproject2013)
- [Vytvoření základní aplikace](#baseapp)
- [Spouštění rozbočovače při spuštění aplikace](#startup2013)
- [Přidat klienta smyčky](#clientloop)
- [Přidat server smyčky](#serverloop)
- [Přidat smooth animace na straně klienta](#animation)
- [Další kroky](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Požadavky

Tento kurz vyžaduje Visual Studio 2013.

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>Vytvořte projekt a přidejte balíček SignalR a JQuery.UI NuGet

V této části vytvoříme projektu v sadě Visual Studio 2013.

Následující kroky použijte k vytvoření prázdné webové aplikace ASP.NET a přidání knihoven SignalR a jQuery.UI Visual Studio 2013:

1. V sadě Visual Studio vytvořte webovou aplikaci ASP.NET.

    ![Vytvořit web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. V **nový projekt ASP.NET** okno, ponechejte **prázdný** vybraný a klikněte na tlačítko **vytvořit projekt**.

    ![Vytvořit prázdnou webovou](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt, vyberte **přidat | Třídy rozbočovače SignalR (v2)**. Název třídy **MoveShapeHub.cs** a přidejte ji do projektu. Tento krok vytvoří **MoveShapeHub** třídy a přidá do projektu sadu souborů skriptů a odkazy na sestavení, které podporují funkce SignalR.

    > [!NOTE]
    > SignalR můžete také přidat do projektu kliknutím **nástroje | Správce balíčků knihoven | Konzola správce balíčků** a spuštění příkazu:

    `install-package Microsoft.AspNet.SignalR`. 

    Používáte-li přidat SignalR konzole, můžete vytvořte třídy rozbočovače SignalR jako samostatný krok po přidání SignalR.
4. Klikněte na tlačítko **nástroje | Správce balíčků knihoven | Konzola správce balíčků**. V okně Správce balíčku spusťte následující příkaz:

    `Install-Package jQuery.UI.Combined`

    To nainstaluje knihovny uživatelského rozhraní jQuery, který budete používat pro animaci tvaru.
5. V **Průzkumníku řešení** rozbalte uzel skripty. Skript knihovny jQuery, jQueryUI a SignalR jsou viditelné v projektu.

    ![Reference knihovny skriptu](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Vytvoření základní aplikace

V této části vytvoříme prohlížeče aplikace, která odesílá umístění tvaru na server během každou událost pohybu myší. Přijetí, všesměrově server pak tyto informace do všech ostatních připojených klientů. Jsme budete rozšířit do této aplikace v pozdějších částech.

1. Pokud jste ještě nevytvořili třídě MoveShapeHub.cs v **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat**, **třídy...** . Název třídy **MoveShapeHub** a klikněte na tlačítko **přidat**.
2. Nahraďte kód v novém **MoveShapeHub** třídy následujícím kódem.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    `MoveShapeHub` Výše třída je implementace rozbočovače SignalR. Jako v [Začínáme s SignalR](tutorial-getting-started-with-signalr.md) kurzu rozbočovače má metodu, která klienti bude volat přímo. V tomto případě klient odešle objekt, který obsahuje nový tvar, který má na server, který pak získá vysílali pro všechny ostatní připojené klienty souřadnice X a Y. SignalR bude automaticky serializovat tento objekt, který používá JSON.

    Objekt, který odešle klientovi (`ShapeModel`) obsahuje členy k uložení pozice tvaru. Verze objektu na serveru také obsahuje členem, abyste mohli sledovat data klientů, které ukládají, tak, aby daného klienta nebude odeslána svá vlastní data. Tento člen používá `JsonIgnore` atribut zabránit jeho se odešle do klienta a serializovat.

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>Spouštění rozbočovače při spuštění aplikace

1. V dalším kroku nastavíme mapování k rozbočovači při spuštění aplikace. V systému SignalR 2, to se provádí přidáním třídy pro spuštění OWIN, který bude volat `MapSignalR` při – spuštění třída `Configuration` metoda spuštěna při spuštění OWIN. Třída je přidán do na OWIN při spuštění zpracování pomocí `OwinStartup` atributu sestavení.

    V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Třídy pro spuštění OWIN**. Název třídy *spuštění* a klikněte na tlačítko **OK**.
2. Obsah souboru Startup.cs změňte na následující:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>Přidání klienta

1. Potom přidáme klienta. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Nová položka**. V **přidat novou položku** dialogovém okně, vyberte **stránku Html**. Název stránky **Default.html** a klikněte na tlačítko **přidat**.
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem na stránku, kterou jste právě vytvořili a klikněte na **nastavit jako úvodní stránku**.
3. Nahraďte kód výchozí stránku HTML následující fragment kódu.

    > [!NOTE]
    > Ověřte, zda skript odkazuje níže shodu balíčky přidat do projektu ve složce skripty.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    Ve výše uvedeném kódu HTML a JavaScript vytvoří red Div názvem tvaru, umožňuje tvaru přetahování chování pomocí knihovny jQuery a používá tvaru `drag` událost, která má odeslat umístění na server.
4. Spusťte aplikaci stisknutím klávesy F5. Zkopírujte adresu URL stránky a vložte jej do druhé okno prohlížeče. Přetáhněte obrazec v jednom z okna prohlížeče; Přesuňte tvaru v jiných okna prohlížeče.

    ![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Přidat klienta smyčky

Vzhledem k tomu, že odesílání umístění obrazce na každou událost pohybu myší vytvoří nepotřebných množství síťový provoz, je potřeba omezeny zprávy z klienta. Použijeme javascript `setInterval` funkce nastavit smyčku, která odesílá informace o novém umístění na server s pevnou sazbou. Tato smyčka je velmi základní reprezentace "herní smyčka", opakovaně zavolat funkci, která jednotky všechny funkce hry nebo jiné simulace.

1. Aktualizujte kód klienta na stránce HTML tak, aby odpovídala následující fragment kódu.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    Výše uvedené aktualizace přidá `updateServerModel` funkce, který je volán na pevnou frekvencí. Tato funkce odesílá umístění dat na server vždy, když `moved` příznak znamená, že existuje nové umístění dat k odeslání.
2. Spusťte aplikaci stisknutím klávesy F5. Zkopírujte adresu URL stránky a vložte jej do druhé okno prohlížeče. Přetáhněte obrazec v jednom z okna prohlížeče; Přesuňte tvaru v jiných okna prohlížeče. Vzhledem k tomu, že počet zpráv, které získat odeslány na server budou omezeny, nezobrazí se jako plynulé jako v předchozí části animace.

    ![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Přidat server smyčky

V aktuální aplikaci zprávy odeslané ze serveru klientovi přejděte často příjmu. To představuje podobný problém, jak je vidět na straně klienta; častěji, než jsou potřeba, a připojení může být přenášeny proto nelze odesílat zprávy. Tato část popisuje postup aktualizace serveru pro implementaci časovač, který omezí generovaný počet odchozích zpráv.

1. Nahraďte obsah `MoveShapeHub.cs` následujícím fragmentem kódu.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Výše uvedený kód rozšiřuje klienta přidat `Broadcaster` třídy, která omezí generovaný odchozích zpráv pomocí `Timer` třídy z rozhraní .NET framework.

    Vzhledem k tomu, že je přechodné rozbočovače, samotné (vytváří se pokaždé, když je to potřeba), `Broadcaster` bude vytvořen jako typ singleton. Opožděná inicializace (dostupné v rozhraní .NET 4) se používá k odložení svého vytvoření, dokud nebude potřeba, zajistíte, aby první instanci rozbočovače úplně vytvořený před zahájením časovač.

    Volání klientů `UpdateShape` funkce je pak přesunout mimo rozbočovače na `UpdateModel` metody, které se nazývá už okamžitě vždy, když jsou přijaty příchozí zprávy. Místo toho budou odeslány zprávy, které mají klienty s rychlostí 25 volání za sekundu, spravuje `_broadcastLoop` časovače uvnitř `Broadcaster` třídy.

    Nakonec namísto volání metody klienta od rozbočovače přímo, `Broadcaster` třída musí získat odkaz na aktuálně operačního centra (`_hubContext`) pomocí `GlobalHost`.
2. Spusťte aplikaci stisknutím klávesy F5. Zkopírujte adresu URL stránky a vložte jej do druhé okno prohlížeče. Přetáhněte obrazec v jednom z okna prohlížeče; Přesuňte tvaru v jiných okna prohlížeče. Nebudou viditelné rozdíl v prohlížeči z předchozí části, ale počet zpráv, které získat odeslat klientovi, budou omezeny.

    ![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Přidat smooth animace na straně klienta

Aplikace je téměř dokončena, ale může uděláme jeden další vylepšení v pohybu tvaru v klientovi pohybu v odpovědi na zprávy serveru. Místo nastavení polohy tvaru do nového umístění zadané serverem, použijeme knihovna uživatelského rozhraní JQuery `animate` funkce přesunutí obrazce plynule mezi aktuálních a nových pozici.

1. Aktualizace klienta `updateShape` metodu vypadat jako následující zvýrazněný kód:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    Ve výše uvedeném kódu přesune tvaru z původního umístění do nového zadaný server v průběhu intervalu animace (v tomto případě 100 milisekund). Všechny předchozí animace spuštěna na tvar, který není zaškrtnuto, před zahájením nové animace.
2. Spusťte aplikaci stisknutím klávesy F5. Zkopírujte adresu URL stránky a vložte jej do druhé okno prohlížeče. Přetáhněte obrazec v jednom z okna prohlížeče; Přesuňte tvaru v jiných okna prohlížeče. Přesun tvaru v dalším okně by se zobrazit méně trhané, jako je jeho přesunu interpolované přes čas a nikoli Probíhá nastavení jednou za příchozí zprávy.

    ![Okno aplikace](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Další kroky

V tomto kurzu když jste se naučili programování aplikace SignalR, která odesílá zprávy vysoká frekvence mezi klienty a servery. Tato komunikace zlepší je užitečné pro vývoj online her a dalších simulace, například [herní ShootR vytvořili pomocí nástroje SignalR](http://shootr.signalr.net).

Kompletní aplikace vytvořené v tomto kurzu lze stáhnout z [galerie kódů](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Další informace o konceptech vývoj SignalR, najdete na následujících stránkách pro SignalR zdrojového kódu a prostředky:

- [Projekt SignalR](http://signalr.net)
- [SignalR Githubu a ukázky](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

Podrobný postup nasazení aplikace SignalR do Azure, najdete v části [pomocí SignalR s webovými aplikacemi ve službě Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Podrobné informace o tom, jak nasadit webový projekt sady Visual Studio na webu systému Windows Azure najdete v tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
