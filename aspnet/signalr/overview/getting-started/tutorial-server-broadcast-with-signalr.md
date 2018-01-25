---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: "Kurz: Server všesměrového vysílání pomocí nástroje SignalR 2 | Microsoft Docs"
author: tdykstra
description: "Tento kurz ukazuje, jak vytvořit webovou aplikaci, která používá ASP.NET SignalR 2 k poskytnutí všesměrového vysílání funkce serveru. Všesměrové vysílání serveru znamená, že commun..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 98a7ce4991d58181177cf56976888e9fd1526987
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-server-broadcast-with-signalr-2"></a>Kurz: Server všesměrového vysílání pomocí nástroje SignalR 2
====================
podle [tní Dykstra](https://github.com/tdykstra), [tní FitzMacken](https://github.com/tfitzmac)

> Tento kurz ukazuje, jak vytvořit webovou aplikaci, která používá ASP.NET SignalR 2 k poskytnutí všesměrového vysílání funkce serveru. Všesměrové vysílání serveru znamená, že komunikace se odešle klientům zahájili serveru. Tento scénář vyžaduje jiný programovací přístup než peer-to-peer scénáře, jako jsou například aplikace chat, ve kterých se spouští komunikace odeslaných klientům jednu nebo více klientů.
> 
> Aplikace, která v tomto kurzu vytvoříte simuluje běžícími Typický scénář v nástroji všesměrového vysílání funkce serveru.
> 
> Toto téma byl původně zapsán pomocí Patrik Fletcher.
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

V tomto kurzu budete vytvoření běžícími aplikace, který je typický pro aplikace v reálném čase, ve kterých chcete pravidelně "push" nebo všesměrového vysílání, chcete-li všechny připojené klienty upozornění ze serveru. V první části tohoto kurzu vytvoříte zjednodušenou verzi aplikace od začátku. Ve zbývající části tohoto kurzu budete instalovat balíček NuGet, který obsahuje další funkce a zkontrolovat kód pro tyto funkce.

Aplikace, která budete sestavení v první části tohoto kurzu zobrazí mřížku uložených daty.

![Počáteční verze StockTicker](tutorial-server-broadcast-with-signalr/_static/image1.png)

Server pravidelně náhodně aktualizací uložených ceny a nabízených oznámení aktualizace pro všechny připojené klienty. V prohlížeči čísla a symboly v **změnit** a  **%**  sloupců dynamicky měnit v reakci na oznámení ze serveru. Pokud můžete otevřít další prohlížeče na stejnou adresu URL, budou všechny zobrazit stejná data a stejné změny dat současně.

Tento kurz obsahuje následující části:

- [Požadavky](#prerequisites)
- [Vytvoření projektu](#createproject)
- [Nastavení kódu serveru](#server)
- [Nastavit kód klienta](#client)
- [Testování aplikace](#test)
- [Povolení protokolování](#enablelogging)
- [Instalace a zkontrolovat v celé ukázce StockTicker](#fullsample)
- [Další kroky](#nextsteps)

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) balíček NuGet nainstaluje ukázkové simulated běžícími aplikace v projektu sady Visual Studio.

> [!NOTE]
> Pokud nechcete, aby postup provede kroky vytváření aplikace, můžete nainstalovat balíček SignalR.Sample v nový projekt prázdný webové aplikace ASP.NET. Je-li nainstalovat balíček NuGet bez kroky v tomto kurzu **musí podle pokynů v souboru readme.txt**. Spusťte balíček, je nutné přidat třídy pro spuštění OWIN, který volá metodu ConfigureSignalR ve s nainstalovaným balíčkem. Obdržíte chybu, pokud nepřidáte třídy pro spuštění OWIN.


<a id="prerequisites"></a>

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že máte Visual Studio 2013 v počítači nainstalována. Pokud nemáte Visual Studio, najdete v části [ASP.NET stáhne](https://www.asp.net/downloads) získat bezplatné sady Visual Studio 2013 Express.

<a id="createproject"></a>

## <a name="create-the-project"></a>Vytvoření projektu

1. Z **soubor** nabídky, klikněte na tlačítko **nový projekt**.
2. V **nový projekt** dialogové okno, rozbalte seznam **C#** pod **šablony** a vyberte **webové**.
3. Vyberte **prázdný webové aplikace ASP.NET** šablony, název projektu *SignalR.StockTicker*a klikněte na tlačítko **OK**.

    ![Dialogové okno Nový projekt](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. V **nové ASP.NET** okna projektu, ponechejte **prázdný** vybraný a klikněte na tlačítko **vytvořit projekt**.

    ![Dialogové okno Nový projekt ASP](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Nastavení kódu serveru

V této části můžete nastavit kód, který běží na serveru.

### <a name="create-the-stock-class"></a>Vytvořte třídu Stock

Začněte vytvořením Stock třídu modelu, který budete používat k ukládání a přenášet informace o populace.

1. Vytvořit nový soubor třídy ve složce projektu, pojmenujte ji *Stock.cs*a pak nahraďte kód šablony s následujícím kódem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Dvě vlastnosti, které budete nastavit při vytváření akcií jsou Symbol (například MSFT Microsoft) a cenu. Ostatní vlastnosti závisí na tom, jak a kdy nastavíte ceny. Při prvním nastavení ceny, je hodnota získá rozšířena do DayOpen. Následné časy, když jste nastavili ceníku, tato změna a hodnoty vlastností PercentChange jsou vypočítávány podle rozdíl mezi ceny a DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Vytvoření třídy StockTicker a StockTickerHub

Rozhraní API rozbočovače SignalR budete používat pro zpracování interakce klienta a serveru. StockTickerHub třída odvozená z třídy rozbočovače SignalR bude zpracovávat přijímá připojení a volání metod od klientů. Musíte také k zachování uložených dat a spustit časovač objekt pravidelně spouštět aktualizaci cen, nezávisle na připojení klientů. Tyto funkce nelze vložit do třídy rozbočovače, protože nejsou přechodné instance rozbočovače. Pro každou operaci na rozbočovači, jako je připojení a volání od klienta k serveru se vytvoří instance třídy rozbočovače. Takže mechanismus, který zajistí uložených dat, aktualizuje ceny a vysílá aktualizaci cen musí spustit v samostatné třídy, které budete název StockTicker.

![Všesměrové vysílání z StockTicker](tutorial-server-broadcast-with-signalr/_static/image5.png)

Chcete pouze jedna instance třídy StockTicker ke spuštění na serveru, takže budete muset nastavit odkaz z každá instance StockTickerHub k StockTicker instanci typu singleton. Třída StockTicker nemá být schopni všesměrové vysílání pro klienty, protože nemá na základě dat a aktivuje aktualizace, ale StockTicker není třídy rozbočovače. Proto je třídu StockTicker získat odkaz na objekt kontextu připojení rozbočovače SignalR. Objekt kontextu připojení SignalR pak může použít k vysílání klientům.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a klikněte na tlačítko **přidat | Třídy rozbočovače SignalR (v2)**.
2. Název nového centra *StockTickerHub.cs*a potom klikněte na **přidat**. Balíčky SignalR NuGet přidá do projektu.
3. Kód šablony nahraďte následujícím kódem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    [Rozbočovače](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) třída se používá k definování metody klienty můžete volat na serveru. Definování jednu metodu: `GetAllStocks()`. Když se klient původně připojí k serveru, se bude volat tuto metodu za účelem získání seznamu všech sledovaných akcií s jejich aktuální ceny. Můžete provést synchronně a vrátí metodu `IEnumerable<Stock>` vzhledem k tomu, že ji vrací data z paměti. Pokud metoda museli získat data pomocí tohoto postupu něco, co by zahrnovat čekání, jako je vyhledávání v databázi nebo volání webové služby, zadali byste `Task<IEnumerable<Stock>>` jako návratová hodnota Povolit asynchronní zpracování. Další informace najdete v tématu [ASP.NET SignalR centra API Průvodce - Server - při spuštění asynchronně](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

    HubName atribut určuje, jak se bude odkazovat rozbočovače v kódu jazyka JavaScript na straně klienta. Výchozí název na straně klienta, pokud nepoužijete tento atribut je ve formátu camelCase verzi název třídy, které by v tomto případě stockTickerHub.

    Jak uvidíte později při vytváření StockTicker třídy, se vytvoří instanci typu singleton této třídy v jeho statickou vlastnost Instance. Že instanci typu singleton daného StockTicker zůstane v paměti bez ohledu na to, kolik klientů připojení nebo odpojení a tato instance je metoda GetAllStocks používá k vrácení aktuální informací uložených.
4. Vytvořit nový soubor třídy ve složce projektu, pojmenujte ji *StockTicker.cs*a pak nahraďte kód šablony s následujícím kódem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    Vzhledem k tomu, že více vláken bude používat stejnou instanci StockTicker kódu, musí být threadsafe StockTicker třídy.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Instanci typu singleton ukládání do statických polí

    Kód inicializuje statických \_pole instance, která zálohuje vlastnost Instance s instancí třídy, a to je pouze instance třídy, která se dají vytvořit, protože konstruktoru je označen jako soukromé. [Opožděná inicializace](https://msdn.microsoft.com/library/dd997286.aspx) se používá pro \_pole instance, není z důvodů výkonu ale a zkontrolujte, zda je vytvoření instance threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    Pokaždé, když klient připojí k serveru, novou instanci třídy StockTickerHub spuštěna v samostatných podprocesu získá instanci typu singleton StockTicker z StockTicker.Instance statické vlastnosti, jako jste viděli dříve ve třídě StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Ukládání dat uložených v ConcurrentDictionary

    Konstruktor inicializuje \_akcií kolekce s některými ukázkových uložených dat a GetAllStocks vrátí populací. Jak už jste viděli dříve, tato kolekce akcií zase vrácený StockTickerHub.GetAllStocks, což je metoda serveru ve třídě rozbočovače, které klienty můžete volat.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    Kolekce akcií je definován jako [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) typ pro bezpečný přístup z více vláken. Jako alternativu, můžete použít [slovník](https://msdn.microsoft.com/library/xfhwa508.aspx) objektu a explicitně zamknout slovníku, když provedete změny.

    Tato ukázková aplikace je OK pro uložení dat aplikace v paměti a ke ztrátě dat při zrušení StockTicker instance. V reálné aplikaci byste pracovat s back-end data store například do databáze.

    ### <a name="periodically-updating-stock-prices"></a>Pravidelně aktualizuje uložených ceny

    Konstruktor spuštění časovače objekt, který pravidelně volá metody, které aktualizace uložených ceny náhodně.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    UpdateStockPrices je volána službou časovač, která předá hodnotu null v parametru state. Před aktualizací ceny, zámek pořízené \_updateStockPricesLock objektu. Kód zkontroluje, zda jiné vlákno je už aktualizace ceny, a pak zavolá TryUpdateStockPrice na každý stock v seznamu. Metoda TryUpdateStockPrice rozhodne, zda se změní uložených cena a kolik ho změnit. Pokud dojde ke změně cenu akcií, BroadcastStockPrice je volána pro všesměrové vysílání změna uložených ceny pro všechny připojené klienty.

    \_UpdatingStockPrices příznak je označena jako [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) zajistit, že je přístup k němu threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    V reálné aplikaci byste metodu TryUpdateStockPrice volání webové služby k vyhledání cenu; v tento kód používá generátor náhodných čísel náhodně provádět změny.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Získávání kontext SignalR tak, aby klientům můžete vysílání StockTicker – třída

    Protože změny cen pocházejí zde v objektu StockTicker, to je objekt, který potřebuje volat metodu updateStockPrice na všech připojených klientů. Ve třídě rozbočovače máte rozhraní API pro volání metody klienta, ale StockTicker není odvozena od třídy rozbočovače a nemá odkaz na objekt žádné rozbočovače. Proto aby bylo možné všesměrové vysílání pro připojené klienty, třída StockTicker musí získat instance kontextu SignalR pro StockTickerHub třídu a použijte ho k volání metody na klientských počítačích.

    Kód získá odkaz na kontext SignalR při vytváření instance třídy singleton, předává, které odkazují na do konstruktoru, a vloží ho konstruktoru vlastnost klientů.

    Existují dva důvody, proč chcete získat kontext pouze jednou: získávání kontextu je náročná operace a jednou pro spolupráci zajistíte zachování určený pořadí zprávy odeslané do klientů.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    Získávání klienti vlastností kontextu a umístit ho do vlastnosti StockTickerClient umožňuje psát kód pro volání metod klienta, který vypadá stejně, jako by v třídy rozbočovače. Například všesměrové vysílání pro všechny klienty můžete napsat Clients.All.updateStockPrice(stock).

    UpdateStockPrice metoda, která jsou volání v BroadcastStockPrice ještě; neexistuje přidáte ho později při psaní kódu, který běží na klientovi. Najdete zde updateStockPrice protože Clients.All je dynamický, což znamená, že se vyhodnotí výraz v době běhu. Při volání této metody se provede, bude SignalR posílat název metody a hodnota parametru do klienta a pokud má klient metodu s názvem updateStockPrice, volání této metody a hodnota parametru se předá ho.

    Clients.All znamená odeslat na všechny klienty. Funkce SignalR poskytuje dalších možností, které určují, které klienti nebo skupiny klientů k odeslání do. Další informace najdete v tématu [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Zaregistrovat trasy SignalR

Server musí znát adresu URL, která je zachytí a přesměrování na SignalR. Chcete-li provést, přidáte a třídy pro spuštění OWIN.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Třídy pro spuštění OWIN**. Název třídy **Startup.cs**.
2. Nahraďte kód v **Startup.cs** následujícím kódem.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Teď jste dokončili nastavení kódu serveru. V další části budete nastavení klienta.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Nastavit kód klienta

1. Vytvořte nový soubor HTML ve složce projektu s názvem *StockTicker.html*.
2. Kód šablony nahraďte následujícím kódem.

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    HTML vytvoří tabulku se sloupci 5, řádek záhlaví a řádek dat s jedinou buňku, která zahrnuje všechny sloupce 5. Řádku dat zobrazí "načítání..." a se zobrazí pouze na okamžik při spuštění aplikace. Kód jazyka JavaScript odebere tento řádek a přidat v jeho místní řádky s uložených data načtená ze serveru.

    Značky skriptu zadejte souboru skriptu jQuery, soubor skriptu SignalR core, soubor skriptu SignalR proxy servery a StockTicker soubor skriptu, který vytvoříte později. Soubor skriptu SignalR proxy, který určuje adresu URL, "/ signalr/hubs", se dynamicky vygeneruje a v takovém případě definuje proxy metody pro metody pro třídy rozbočovače pro StockTickerHub.GetAllStocks. Pokud dáváte přednost, tento soubor JavaScript generování ručně pomocí [nástroje SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) a zakázat dynamické souboru vytvoření při volání metody MapHubs.
3. > [!IMPORTANT]
 > Ujistěte se, zda soubor JavaScript odkazuje v *StockTicker.html* jsou správné. To znamená, ujistěte se, že verze jQuery ve vaší značky script (1.10.2 v příkladu) je stejná jako verze jQuery ve vašem projektu *skripty* složku a ujistěte se, že verze SignalR ve vaší značky script je stejný jako funkce SignalR verze ve vašem projektu *skripty* složky. V případě potřeby, změňte názvy souborů v značek skriptu.
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem na *StockTicker.html*a potom klikněte na **nastavit jako úvodní stránku**.
5. Vytvořte nový soubor JavaScript ve složce projektu s názvem *StockTicker.js*...
6. Kód šablony nahraďte následujícím kódem:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    $.connection odkazuje na proxy SignalR. Kód získá odkaz na proxy serveru pro třídu StockTickerHub a vloží ho burzovní proměnné. Název proxy serveru je název, který byl nastaven atribut [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    Jakmile jsou definovány všechny proměnné a funkce, poslední řádek kódu v souboru inicializuje připojení SignalR voláním funkce SignalR start. Funkce start asynchronně provede a vrátí [jQuery odložené objekt](http://api.jquery.com/category/deferred-object/), což znamená, že můžete volat funkci done k určení funkce k volání při dokončení asynchronní operace...

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    Init – funkce volá funkci getAllStocks na serveru a používá informace, které server vrátí uložených tabulku aktualizovat. Všimněte si, že ve výchozím nastavení, budete muset použít formátu camelCase velká a malá písmena v klientovi sice název metody, pascal použita na serveru. Pravidlo formátu camelCase-velká a malá písmena platí jenom pro metody, ne objekty. Například odkazujete stock. Symbol a stock. Ceny, není stock.symbol nebo stock.price.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    Pokud jste chtěli v klientovi použít pascal velká a malá písmena, nebo pokud jste chtěli použít název úplně jinou metodu, vám může uspořádání metody rozbočovače s atributem HubMethodName stejným způsobem jako dekorované se třídy rozbočovače s atributem HubName.

    V metodě init HTML pro řádek tabulky se vytvoří pro každý uložených objekt přijatých ze serveru volání formatStock formát vlastností uložených objektu, a potom pomocí volání mohla nahradit (která je definována v horní části *StockTicker.js*) pro nahrazení zástupných symbolů v proměnné rowTemplate hodnoty vlastností uložených objektu. Výsledný HTML je pak připojí k uložených tabulky.

    Init – lze volat v předáním jako funkce zpětného volání, která spustí po dokončení asynchronního spuštění funkce. Pokud jste volali metodu init – jako samostatný příkaz JavaScript po volání metody spuštění funkce by nezdaří, protože by spustit okamžitě bez čekání na spuštění funkce ukončíte navazování připojení. Init – funkce v takovém případě by se pokusil volat funkci getAllStocks předtím, než je vytvořeno připojení k serveru.

    Když se server změní cena stock, volání updateStockPrice na připojené klienty. Funkce se přidá do vlastnosti klienta stockTicker proxy serveru je k dispozici na volání ze serveru.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    Funkce updateStockPrice naformátuje objekt uložených přijatých ze serveru do řádku tabulky na stejném principu jako init – funkce. Připojit řádek do tabulky, je však vyhledá stock aktuální řádek v tabulce a nahradí tento řádek s novým.

<a id="test"></a>

## <a name="test-the-application"></a>Testování aplikace

1. Stisknutím klávesy F5 spusťte aplikaci v režimu ladění.

    V uložených původně tabulce "načítání..." řádku, pak po chvíli trvat, které se zobrazí počáteční uložených dat a spusťte ceny akcií změnit.

    ![Načítání](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![Počáteční uložených tabulky](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![Uložené tabulky přijímat změny ze serveru](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. Zkopírujte adresu URL z panelu Adresa prohlížeče a vložte ho do jednoho nebo více nových časových období prohlížeče.

    Počáteční uložených zobrazení je stejný jako první prohlížeče a změny nastat současně.
3. Zavřít všechny prohlížeče a otevřete nový prohlížeč a přejděte na stejnou adresu URL.

    Objekt typu singleton StockTicker stále běží na serveru, takže zobrazení uložené tabulka ukazuje, že populací dál zlepšovalo, chcete-li změnit. (Nevidíte počáteční tabulku s nula, změňte následující obrázky.)
4. Zavřete prohlížeč.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Povolení protokolování

SignalR obsahuje vestavěné protokolování funkci, která můžete povolit na klientovi, které pomáhají při řešení potíží s. V této části Povolit protokolování a příklady, které ukazují, jak protokoly oznámením, která z těchto metod přenosu používá SignalR:

- [Technologie WebSockets](http://en.wikipedia.org/wiki/WebSocket)podporovaný IIS 8 a aktuální prohlížeče.
- [Server odeslal události](http://en.wikipedia.org/wiki/Server-sent_events)podporovaný prohlížeče než Internet Explorer.
- [Navždy rámce](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), Internet Explorer nepodporuje.
- [AJAX dlouhé dotazování](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), nepodporuje všechny prohlížeče.

Funkce SignalR pro dané připojení, vybere nejlepší metody přenosu, který podporuje serveru a klienta.

1. Otevřete *StockTicker.js* a řádek kódu povolení protokolování bezprostředně před kód, který inicializuje připojení na konci souboru:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. Stisknutím klávesy F5 spusťte projekt.
3. Otevřete okno nástroje pro vývojáře v prohlížeči a vyberte v konzole pro najdete v souborech protokolů. Bude pravděpodobně nutné aktualizovat stránku a najdete v protokolech vyjednávání metodu přenosu pro nové připojení Signalr.

    Pokud používáte Internet Explorer 10 v systému Windows 8 (IIS 8), metodu přenosu je objekty WebSockets.

    ![Konzole IE 10 IIS 8](tutorial-server-broadcast-with-signalr/_static/image9.png)

    Pokud aplikace Internet Explorer 10 běží na systému Windows 7 (službu IIS 7.5), metodu přenosu je iframe.

    ![IE 10 konzoly, IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    Ve Firefoxu nainstalujte doplněk Firebug získat okna konzoly. Pokud používáte Firefox 19 v systému Windows 8 (IIS 8), metodu přenosu je objekty WebSockets.

    ![Firefox 19 IIS 8 objekty Websockets](tutorial-server-broadcast-with-signalr/_static/image11.png)

    Pokud Firefox 19 běží na systému Windows 7 (službu IIS 7.5), je způsob přepravy události odeslané serverem.

    ![Firefox 19 konzole služby IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Instalace a zkontrolovat v celé ukázce StockTicker

StockTicker aplikaci, která je nainstalována pomocí [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) balíček NuGet obsahuje více funkcí než zjednodušené verzi, kterou jste právě vytvořili od začátku. V této části kurzu nainstalujte balíček NuGet a zkontrolujte nové funkce a kód, který je implementuje. Je-li nainstalovat balíček bez předchozích kroků tohoto kurzu, je nutné přidat do projektu třídy pro spuštění OWIN. Tento krok je vysvětleno v souboru readme.txt pro balíček NuGet.

### <a name="install-the-signalrsample-nuget-package"></a>Nainstalujte balíček SignalR.Sample NuGet

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a klikněte na **spravovat balíčky NuGet**.
2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **Online**, zadejte *SignalR.Sample* v **Online hledání** pole a pak klikněte na tlačítko  **Nainstalujte** v **SignalR.Sample** balíčku.

    ![Instalovat balíček SignalR.Sample](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. V **Průzkumníku řešení**, rozbalte *SignalR.Sample* složku, která byla vytvořená při instalaci balíčku SignalR.Sample.
4. V *SignalR.Sample* složku, klikněte pravým tlačítkem na *StockTicker.html*a potom klikněte na **nastavit jako úvodní stránku**.

    > [!NOTE]
    > Instalace SignalR.Sample NuGet balíček mohou změnit verzi jQuery, který máte ve vaší *skripty* složky. Nové *StockTicker.html* soubor, který nainstaluje balíček *SignalR.Sample* složky budou synchronizována s jQuery verzi, která nainstaluje balíček, ale pokud chcete spustit původní *StockTicker.html* znovu, možná budete muset nejdřív aktualizovat odkaz na jQuery ve značce skriptu.

### <a name="run-the-application"></a>Spuštění aplikace

1. Stisknutím klávesy F5 spusťte aplikaci.

    Kromě mřížky, které jste předtím viděli zobrazuje aplikace úplné běžícími vodorovně posouvání okno, které zobrazí stejné uložených data. Při spuštění aplikace pro první "trhu" je "Uzavřeno" a zobrazí statické mřížky a burzovní okno, které není posouvání.

    ![StockTicker obrazovky start](tutorial-server-broadcast-with-signalr/_static/image14.png)

    Když kliknete na tlačítko **otevřete trhu**, **Live burzovní Stock** pole začne vodorovný posun a server se spustí k pravidelně vysílání uložených cena změny v náhodných intervalech. Pokaždé, když uložených cena změní, i **Live tabulky Stock** mřížky a **Live burzovní Stock** pole jsou aktualizovány. Pokud je změna ceny stock kladné, stav se zobrazí s zelenou pozadí, když je změna záporná, stav se zobrazí s červeným pozadím.

    ![Otevřete aplikaci StockTicker trhu](tutorial-server-broadcast-with-signalr/_static/image15.png)

    **Zavřít trhu** tlačítko přestane změny a posouvání burzovní, k dispozici a **resetovat** tlačítko obnoví všechny uložených dat do původního stavu před spuštěním cena změny. Pokud otevřete další okna prohlížeče a přejděte na stejnou adresu URL, zobrazí stejná data dynamicky aktualizuje ve stejnou dobu v každé prohlížeče. Když kliknete na jedno z tlačítek, všechny prohlížeče reagovat stejným způsobem jako ve stejnou dobu.

### <a name="live-stock-ticker-display"></a>Za provozu Stock burzovní zobrazení

**Live burzovní Stock** zobrazení je neuspořádaného seznamu v div elementu, který je naformátovaný do jednoho řádku pomocí stylů CSS. Burzovní je inicializován a aktualizovat stejným způsobem jako v tabulce: nahrazení zástupných symbolů v &lt;li&gt; řetězec šablony a dynamicky přidání &lt;li&gt; elementů &lt;ul&gt; element. Posouvání se provádí pomocí funkce postupné animaci jQuery k odlišení okraj zprava doleva neuspořádaný seznam v rámci div.

Burzovní lístek HTML:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

Burzovní lístek šablon stylů CSS:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

Posuňte se jQuery kód, který umožňuje:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Další metody na serveru, který klient může volat

Třída StockTickerHub definuje čtyři další metody, které klient může volat:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

V reakci na tlačítka v horní části stránky se nazývají OpenMarket, CloseMarket a resetování. Vysvětlují vzor jeden klient aktivován změny ve stavu, který je okamžitě rozšířen na všechny klienty. Každá z těchto metod volá metodu v třídě StockTicker této důsledky stavu trhu změnit a potom vysílá nový stav.

Ve třídě StockTicker stav na trhu je možný díky MarketState vlastnost, která vrátí hodnotu výčtu MarketState:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Každá z metod, které změní stav trhu učinit uvnitř blok zámku protože třída StockTicker musí být threadsafe:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Pro zajištění threadsafe, tento kód \_marketState pole, která zálohuje vlastnost MarketState je označena jako volatile,

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Metody BroadcastMarketStateChange a BroadcastMarketReset jsou podobné BroadcastStockPrice metoda, kterou jste už viděli, s výjimkou volání různé metody, které jsou definované na straně klienta:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Další funkce v klientovi, který server můžete volat.

Funkce updateStockPrice teď zpracovává mřížky a zobrazení burzovní a používá jQuery.Color na flash červené a zelené barvy.

Nové funkce v *SignalR.StockTicker.js* povolení a zákaz tlačítek na základě stavu na trhu, a jejich zastavení nebo spuštění burzovní okno vodorovného posouvání. Vzhledem k tomu, že víc funkcí přidávané do ticker.client, [jQuery rozšířit funkce](http://api.jquery.com/jQuery.extend/) se používá k přidání je.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Instalace dalších klientských po navázání připojení

Po navázání připojení klienta, má některé další práci udělat: zjistíte, pokud je na trhu otevřené nebo uzavřené, aby bylo možné volat příslušné marketOpened nebo marketClosed funkce a volání metody server tlačítka připojit.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Metody serveru nejsou svázanou tlačítka až po navázání připojení tak, aby kód nelze pokusit o volání metody server předtím, než jsou k dispozici.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste zjistili, jak programovat aplikace SignalR, která odešle zprávy ze serveru pro všechny připojené klienty v pravidelných intervalech a v reakci na oznámení z jakéhokoli klienta. Vzor pomocí instance Vícevláknová singleton pro uchování stavu serveru také lze také v online herní scénáře více hráčů. Příklad, naleznete v části [ShootR hra, který je založen na SignalR](https://github.com/NTaylorMullen/ShootR).

Podrobné pokyny, které se zobrazí scénáře komunikace peer-to-peer, najdete v části [Začínáme s SignalR](introduction-to-signalr.md) a [aktualizace v reálném čase s SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Informace o pokročilejší SignalR vývoj koncepty, najdete na následujících stránkách pro SignalR zdrojového kódu a prostředky:

- [Funkce SignalR technologie ASP.NET](../../index.md)
- [Projekt SignalR](http://signalr.net/)
- [SignalR Githubu a ukázky](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

Podrobný postup nasazení aplikace SignalR do Azure, najdete v části [pomocí SignalR s webovými aplikacemi ve službě Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Podrobné informace o tom, jak nasadit webový projekt sady Visual Studio na webu systému Windows Azure najdete v tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
