---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: "Funkce SignalR technologie ASP.NET centra API Průvodce – klienta JavaScript | Microsoft Docs"
author: pfletcher
description: "Tento dokument obsahuje úvod do používání centra rozhraní API pro SignalR verze 2 v klientech JavaScript, například s prohlížeči a applicat Windows Store (WinJS)..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 65e369a393a8c5d2d1bba11b5c71347df8f9c69d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>Funkce SignalR technologie ASP.NET centra API Průvodce – JavaScript klienta
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní Dykstra](https://github.com/tdykstra)

> Tento dokument obsahuje úvod do používání centra rozhraní API pro SignalR verze 2 v klientech JavaScript, například s prohlížeči a aplikace pro Windows Store (WinJS).
> 
> Rozhraní API rozbočovače SignalR umožňuje vytvářet vzdálená volání procedur (RPC) ze serveru pro připojené klienty a z klientů k serveru. V serverovém kódu můžete definovat metody, které lze volat klienty a volání metody, které běží na klientovi. V kódu klienta můžete definovat metody, které lze volat ze serveru a volání metody, které běží na serveru. SignalR se stará o všechny záležitosti klient server pro vás.
> 
> Funkce SignalR také nabízí nižší úrovně rozhraní API volat trvalé připojení. Úvod do SignalR, rozbočovače a trvalé připojení, najdete v části [Úvod do SignalR](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Verze softwaru použitým v tomto tématu
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR verze 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Předchozí verze tohoto tématu
> 
> Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Přehled

Tento dokument obsahuje následující části:

- [Vygenerovaný proxy server a jakým způsobem vám](#genproxy)

    - [Kdy použít vygenerovaný proxy server](#cantusegenproxy)
- [Instalace klienta](#clientsetup)

    - [Jak chcete-li dynamicky vygenerovaný proxy server](#dynamicproxy)
    - [Postup vytvoření fyzický soubor pro funkci SignalR generované proxy](#manualproxy)
- [Postup připojení](#establishconnection)

    - [$. connection.hub je stejný objekt vytvoří tento $.hubConnection()](#connequivalence)
    - [Asynchronní provádění metody start](#asyncstart)
- [Postup připojení mezi doménami](#crossdomain)
- [Postup konfigurace připojení](#configureconnection)

    - [Určení parametrů řetězce dotazu](#querystring)
    - [Určení metodu přenosu](#transport)
- [Jak získat proxy server pro třídy rozbočovače](#getproxy)
- [Jak definovat metody na straně klienta, které můžete volat serveru](#callclient)
- [Jak volat metody serveru z klienta](#callserver)
- [Postupy: zpracování události životnost připojení](#connectionlifetime)
- [Způsob zpracování chyb](#handleerrors)
- [Jak povolit protokolování na straně klienta](#logging)

Pro dokumentaci o tom, jak program server nebo klientů .NET, naleznete v následujících zdrojích:

- [Rozhraní API Průvodce pro rozbočovače SignalR – Server](hubs-api-guide-server.md)
- [Rozhraní API Průvodce pro rozbočovače SignalR – klient .NET](hubs-api-guide-net-client.md)

Součást serveru SignalR 2 je dostupná pouze na rozhraní .NET 4.5 (i když je klient .NET pro SignalR 2 na rozhraní .NET 4.0).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Vygenerovaný proxy server a jakým způsobem vám

Můžete naprogramovat JavaScript klienta pro komunikaci se službou SignalR s nebo bez proxy server, který generuje SignalR pro vás. Proxy vykonává vám je zjednodušení syntaxe kódu můžete použít pro připojení, metody zápisu, které server volá, a volání metody na serveru.

Při psaní kódu pro volání metody server vygenerovaný proxy server umožňuje použijte syntaxi, která vypadá jako kdyby byly provádění místní funkce: můžete napsat `serverMethod(arg1, arg2)` místo `invoke('serverMethod', arg1, arg2)`. Syntaxe vygenerovaný proxy server také umožňuje okamžitou a srozumitelné chybě na straně klienta, pokud překlep metoda název serveru. A pokud ručně vytvoříte soubor, který definuje proxy serverů, můžete také získat podporu technologie IntelliSense pro psaní kódu, který volá metody server.

Předpokládejme například, že máte následující třídy rozbočovače na serveru:

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Následující příklady kódu ukazují, co JavaScript kódu vypadá jako pro vyvolání `NewContosoChatMessage` metoda na server a přijímá volání `addContosoChatMessageToPage` metoda ze serveru.

**S vygenerovaný proxy server**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Bez vygenerovaný proxy server**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Kdy použít vygenerovaný proxy server

Pokud chcete zaregistrovat více obslužných rutin událostí pro metodu klienta, který volá serveru, nemůžete použít vygenerovaný proxy server. Jinak můžete zvolit vygenerovaný proxy server nebo není založen na zúčastníte kódování. Pokud si zvolíte možnost nepoužít, nemusíte odkazovat na adresu "rozbočovače/signalr" v `script` elementu v kódu klienta.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Instalace klienta

Klient JavaScript vyžaduje odkazy na jQuery a soubor JavaScript SignalR core. JQuery verze musí být 1.6.4 nebo hlavní novější verze, například 1.7.2, 1.8.2 nebo 1.9.1. Pokud se rozhodnete použít vygenerovaný proxy server, musíte také odkaz k proxy serveru SignalR vygeneruje soubor JavaScript. Následující příklad ukazuje, jak může odkazy vypadat na stránce HTML, který používá vygenerovaný proxy server.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Tyto odkazy musí být zahrnuty v tomto pořadí: jQuery první, poslední SignalR core, po který a proxy SignalR.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Jak chcete-li dynamicky vygenerovaný proxy server

V předchozím příkladu je odkaz na proxy SignalR generované dynamicky generovaný kód JavaScript, není pro fyzický soubor. SignalR vytvoří kód jazyka JavaScript pro proxy server za chodu a slouží ke klientovi v odpovědi na adresu "/ signalr/hubs". Pokud zadáte jiný základní adresu URL pro připojení SignalR na serveru v vaší `MapSignalR` metoda, adresa URL souboru dynamicky vygenerovaný proxy server je vaše vlastní adresu URL s "/ hubs" k němu připojen.

> [!NOTE]
> U klientů Windows 8 (pro Windows Store) JavaScript použijte místo dynamicky vygenerovaný soubor fyzické proxy. Další informace najdete v tématu [vytvoření fyzického souboru pro funkci SignalR generované proxy](#manualproxy) dál v tomto tématu.


V ASP.NET MVC 4 nebo 5 zobrazení syntaxe Razor použijte k odkazování na kořenový adresář aplikace ve vaší odkaz na soubor proxy tilda:

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

Další informace o používání SignalR v MVC 5 v tématu [Začínáme s SignalR a MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

V zobrazení syntaxe Razor rozhraní ASP.NET MVC 3, použijte `Url.Content` pro vaši informaci, soubor proxy serveru:

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

V aplikaci webových formulářů ASP.NET, použijte `ResolveClientUrl` pro vaše servery proxy odkaz na soubor nebo registrovat prostřednictvím ScriptManager pomocí relativní cesty aplikace kořenové (začíná tildou):

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

Obecně platí použijte stejnou metodu pro určení adresy URL "/ signalr/hubs", který používáte pro soubory šablon stylů CSS a JavaScript. Pokud zadáte adresu URL bez použití tildou, v některých scénářích vaše aplikace bude fungovat správně při testování v sadě Visual Studio pomocí služby IIS Express, ale selže s chybou 404 při nasazování do úplnou službu IIS. Další informace najdete v tématu **řešení odkazy na kořenové úrovni prostředky** v [webové servery v sadě Visual Studio pro webové projekty ASP.NET](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx) na webu MSDN.

Při spuštění webového projektu v sadě Visual Studio 2013 v režimu ladění, a pokud používáte Internet Explorer jako prohlížeč, zobrazí se soubor proxy serveru v **Průzkumníku řešení** pod **dokumentů skriptu**, jak je znázorněno Následující obrázek.

![Soubor vygenerovaný proxy server JavaScript v Průzkumníku řešení](hubs-api-guide-javascript-client/_static/image1.png)

Chcete-li zobrazit obsah souboru, dvakrát klikněte na **hubs**. Pokud nepoužíváte Visual Studio 2012 nebo 2013 a prohlížeče Internet Explorer, nebo pokud nejsou v režimu ladění, můžete také získat obsah souboru tak, že přejde na adresu "/ signalR/hubs". Například, pokud vaše lokalita běží v `http://localhost:56699`, přejděte na `http://localhost:56699/SignalR/hubs` v prohlížeči.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Postup vytvoření fyzický soubor pro funkci SignalR generované proxy

Jako alternativu k dynamicky vygenerovaný proxy server můžete vytvořit fyzický soubor, který má kód proxy a tento soubor. Můžete to provést kontrolu nad ukládání do mezipaměti nebo sdružování chování nebo získat IntelliSense při programování volání metody server.

Pokud chcete vytvořit soubor proxy serveru, proveďte následující kroky:

1. Nainstalujte [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) balíček NuGet.
2. Otevřete příkazový řádek a přejděte do *nástroje* složku, která obsahuje soubor SignalR.exe. Složky nástroje je v následujícím umístění:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. Zadejte následující příkaz:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Cesta k vaší *.dll* je obvykle *bin* ve složce projektu.

    Tento příkaz vytvoří soubor s názvem *server.js* ve stejné složce jako *signalr.exe*.
4. Umístit *server.js* souborů v příslušné složky ve vašem projektu, přejmenujte jej podle potřeby pro vaši aplikaci a přidejte odkaz na jeho místo odkaz na "rozbočovače/signalr".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Postup připojení

Předtím, než může vytvořit připojení, budete muset vytvořit objekt připojení, vytvořit proxy a registraci obslužných rutin událostí pro metody, které lze volat ze serveru. Pokud jsou nastavení proxy serveru a události obslužných rutin, navázání připojení pomocí volání `start` metoda.

Pokud používáte vygenerovaný proxy server, nemáte vytvořit objekt připojení v kódu, protože kód vygenerovaný proxy server provede za vás.

<a id="nogenconnection"></a>

**Připojení (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Připojení (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Ukázkový kód používá výchozí "/ signalr" adresa URL k připojení k službě SignalR. Informace o tom, jak zadejte jinou adresu URL základní najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – adresa URL /signalr](hubs-api-guide-server.md#signalrurl).

Ve výchozím umístění rozbočovače, je aktuální server; Pokud se připojujete k jinému serveru, zadejte adresu URL před voláním `start` metoda, jak je znázorněno v následujícím příkladu:

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> Za normálních okolností registraci obslužné rutiny událostí před voláním `start` metodu pro vytvoření připojení. Pokud chcete zaregistrovat některé obslužné rutiny událostí po navázání připojení, můžete to udělat, ale je nutné zaregistrovat alespoň jeden z vaší událostí handler(s) před voláním `start` metoda. Jeden důvodem je to, že může být mnoho Hubs v aplikaci, ale nebude chcete aktivovat `OnConnected` událostí na všechny rozbočovače, chcete-li pouze pomocí jedné z nich. Při připojení, je přítomnost metodu klienta na proxy server rozbočovače pro co informuje SignalR pro aktivaci `OnConnected` událostí. Pokud si nezaregistrujete všechny obslužné rutiny před voláním `start` metoda, bude možné volat metody v rozbočovači, ale rozbočovače na `OnConnected` nebude volána metoda a žádné metody klienta, který bude vyvolán ze serveru.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub je stejný objekt vytvoří tento $.hubConnection()

Jak je vidět v příkladech, při použití vygenerovaný proxy server, `$.connection.hub` odkazuje na objekt připojení. Toto je stejný objekt, který získáte při volání `$.hubConnection()` když nepoužíváte vygenerovaný proxy server. Kód vygenerovaný proxy server vytvoří připojení můžete spuštěním následujícího příkazu:

![Vytvoření připojení v souboru vygenerovaný proxy server](hubs-api-guide-javascript-client/_static/image3.png)

Pokud používáte vygenerovaný proxy server, můžete provést cokoli, co se `$.connection.hub` , které můžete provést s objektem připojení po nepoužíváte vygenerovaný proxy server.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Asynchronní provádění metody start

`start` Asynchronně provede metodu. Vrátí [jQuery odložené objekt](http://api.jquery.com/category/deferred-object/), což znamená, že můžete přidat funkce zpětného volání voláním metod `pipe`, `done`, a `fail`. Pokud máte kód, který chcete spustit po připojení, například volání metody server, vložte tento kód funkce zpětného volání nebo volání z funkce zpětného volání. `.done` Metoda zpětného volání je proveden po navázání připojení, a po žádný kód, který budete mít ve vaší `OnConnected` metoda obslužné rutiny událostí na serveru dokončí provádění.

Když vložíte příkaz "Teď připojený" z předchozího příkladu jako dalším řádku kódu po `start` volání metody (není v `.done` zpětného volání), `console.log` řádku, budou spuštěny před připojení, jak je znázorněno v následujícím Příklad:

![Nesprávný způsob, jak napsat kód, který spouští po navázání připojení](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Postup připojení mezi doménami

Obvykle Pokud prohlížeč načte stránky z `http://contoso.com`, připojení SignalR je ve stejné doméně, v `http://contoso.com/signalr`. Pokud stránku z `http://contoso.com` vytvoří připojení k `http://fabrikam.com/signalr`, který je připojení mezi doménami. Z bezpečnostních důvodů jsou ve výchozím nastavení zakázány připojení mezi doménami.

V systému SignalR 1.x napříč požadavky domény kontrolovala jeden příznak EnableCrossDomain. Tento příznak řídí požadavky JSONP a CORS. Pro větší flexibilitu, podpora všechny CORS byla odebrána z komponenty serveru SignalR (klientů JavaScript i nadále používat CORS normálně když se zjistí, zda prohlížeč podporuje), a nové middlewaru OWIN, který má je k dispozici pro tyto scénáře podporovat.

Pokud JSONP je vyžadována na straně klienta (pro podporu požadavků napříč doménami v prohlížečích starší), ji budou muset být explicitně povoleno nastavením `EnableJSONP` na `HubConfiguration` do objektu `true`, jak je uvedeno níže. JSONP je standardně zakázána, protože je to méně bezpečné než CORS.

**Přidání Microsoft.Owin.Cors do projektu:** k instalaci této knihovny, spusťte následující příkaz v konzole Správce balíčků:

`Install-Package Microsoft.Owin.Cors`

Tento příkaz přidá 2.1.0 verze balíčku do projektu.

### <a name="calling-usecors"></a>Volání metody UseCors

 Následující fragment kódu ukazuje, jak implementovat připojení mezi doménami v SignalR 2. 

**Implementace žádosti napříč doménami v SignalR 2**

Následující kód ukazuje, jak povolit CORS a JSONP v projektu SignalR 2. Tento příklad používá `Map` a `RunSignalR` místo `MapSignalR`, aby se spouštěla CORS middleware jenom pro SignalR požadavky, které vyžadují podporu CORS (nikoli pro všechny přenosy v zadané cestě v `MapSignalR`.) Mapa mohou sloužit také pro veškerý middleware, který je potřeba spustit pro konkrétní předponu adresy URL, nikoli pro celou aplikaci.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - Není nastavený `jQuery.support.cors` na hodnotu true v kódu.
> 
>     ![JQuery.support.cors není nastavený na hodnotu true](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR zpracovává použití CORS. Nastavení `jQuery.support.cors` na hodnotu true zakáže JSONP, protože způsobuje, že SignalR předpokládat, že je prohlížeč podporuje CORS.
> - Když se připojujete k adresu URL místního hostitele, Internet Explorer 10 nebude považuje za připojení mezi doménami, takže aplikace se pracovat místně aplikace Internet Explorer 10 i v případě, že jste nepovolili napříč doménami připojení na serveru.
> - Informace o používání připojení mezi doménami s aplikace Internet Explorer 9 najdete v tématu [tohoto podprocesu StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Informace o používání připojení mezi doménami s Chrome najdete v tématu [tohoto podprocesu StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Ukázkový kód používá výchozí "/ signalr" adresa URL k připojení k službě SignalR. Informace o tom, jak zadejte jinou adresu URL základní najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – adresa URL /signalr](hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Postup konfigurace připojení

Před navázáním připojení, můžete určit parametrů řetězce dotazu nebo zadejte metodu přenosu.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Určení parametrů řetězce dotazu

Pokud chcete odesílat data na server, když se klient připojí, můžete přidat parametrů řetězce dotazu pro objekt připojení. Následující příklady ukazují, jak nastavit parametr řetězce dotazu v kódu klienta.

**Nastavte hodnotu řetězce dotazu před voláním metody start (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Nastavte hodnotu řetězce dotazu před voláním metody start (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

Následující příklad ukazuje, jak číst parametr řetězce dotazu v serverovém kódu.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Určení metodu přenosu

Jako součást procesu připojení klienta SignalR normálně vyjedná se serverem určit nejlepší přenos, který je podporován server i klienta. Pokud již víte, které přenosu, kterou chcete použít, můžete tento proces vyjednávání obejít tak, že určíte metodu přenosu při volání `start` metoda.

**Kód klienta, který určuje způsob přepravy (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**Kód klienta, který určuje způsob přepravy (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

Jako alternativu můžete zadat několik metod přenosu v pořadí, ve kterém chcete SignalR pokusit je:

**Kód klienta, který určuje záložní schéma vlastní přenosu (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Kód klienta, který určuje záložní schématu vlastní přenosu (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Pro zadání metodu přenosu můžete použít následující hodnoty:

- "Websocket"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Následující příklady ukazují, jak zjistit, jakou metodu přenosu používá připojení.

**Kód klienta, který zobrazí metodu přenosu používá připojení (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**Kód klienta, který zobrazí metodu přenosu používá připojení (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Informace o tom, jak zkontrolovat metodu přenosu v serverovém kódu najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – jak získat informace o klientovi z vlastností kontextu](hubs-api-guide-server.md#contextproperty). Další informace o přenosy a případech přejít najdete v tématu [Úvod do SignalR – přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Jak získat proxy server pro třídy rozbočovače

Každý objekt připojení, který vytvoříte zapouzdří informace o připojení pro službu SignalR, která obsahuje jednu nebo více tříd rozbočovače. Ke komunikaci s třídy rozbočovače, pomocí objektu proxy, která můžete vytvořit sami (Pokud nepoužíváte vygenerovaný proxy server) nebo který je pro vás vygeneroval.

Název proxy serveru na straně klienta je ve formátu camelCase verzi název třídy rozbočovače. SignalR tuto změnu automaticky provede tak, aby JavaScript konvence může odpovídat kódu jazyka JavaScript.

**Třídy rozbočovače na serveru**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Získat odkaz na proxy serveru generovaného klienta pro rozbočovač**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**Vytvořit proxy server klienta pro třídy rozbočovače (bez vygenerovaný proxy server)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Pokud uspořádání vaší třídy rozbočovače s `HubName` atribut, použít přesný název bez Změna velikosti písmen.

**Třídy rozbočovače na serveru s atributem HubName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Získat odkaz na proxy serveru generovaného klienta pro rozbočovač**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**Vytvořit proxy server klienta pro třídy rozbočovače (bez vygenerovaný proxy server)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Jak definovat metody na straně klienta, které můžete volat serveru

Zadat metodu, která serveru můžete volat z rozbočovače, přidejte obslužnou rutinu události pro proxy server rozbočovače pomocí `client` vlastnost vygenerovaný proxy server nebo volání `on` metoda, pokud nepoužíváte vygenerovaný proxy server. Parametry lze komplexních objektů.

Přidání obslužné rutiny události před voláním `start` metodu pro vytvoření připojení. (Pokud chcete přidat obslužné rutiny událostí po volání `start` metodu, najdete v poznámce v [postup připojení](#establishconnection) dříve v tomto dokumentu a použijte syntaxi pro definování metoda bez použití vygenerovaný proxy server.)

Metoda shoda názvů nerozlišuje. Například `Clients.All.addContosoChatMessageToPage` na serveru, budou spuštěny `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, nebo `addcontosochatmessagetopage` na straně klienta.

**Zadejte metodu na klientovi (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**Alternativní způsob, jak definovat metoda na klientovi (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**Definovat metoda na klienta (bez vygenerovaný proxy server, nebo při přidávání po volání metody spuštění)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**Kód serveru, který volá metodu klienta**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Následující příklady zahrnují komplexní objekt jako parametr metody.

**Zadejte metodu na klienta, který přebírá objekt komplexní (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Zadejte metodu na klienta, který přebírá objekt komplexní (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Kód serveru, který definuje komplexní objekt**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Kód serveru, který volá metodu klienta pomocí komplexní objekt**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Jak volat metody serveru z klienta

Chcete-li zavolat metodu serveru z klienta, použijte `server` vlastnost vygenerovaný proxy server nebo `invoke` metodu na proxy server rozbočovače, pokud nepoužíváte vygenerovaný proxy server. Návratová hodnota nebo parametry můžou být komplexních objektů.

Předat ve stylu camelCase verzi název metody rozbočovače. SignalR tuto změnu automaticky provede tak, aby JavaScript konvence může odpovídat kódu jazyka JavaScript.

Následující příklady ukazují, jak volat metodu serveru, který nemá návratovou hodnotu a jak volat metodu serveru, která nemá návratovou hodnotu.

**Metoda serveru s žádný atribut HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Kód serveru, který definuje komplexní objekt předaný parametr**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**Kód klienta, která volá metodu serveru (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**Kód klienta, která volá metodu serveru (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Pokud dekorované metody rozbočovače s `HubMethodName` atribut, použijte tento název bez Změna velikosti písmen.

**Metoda serveru** s atributem HubMethodName

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**Kód klienta, která volá metodu serveru (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**Kód klienta, která volá metodu serveru (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Předchozí příklady ukazují, jak volat metodu serveru, který nemá žádnou návratovou hodnotu. Následující příklady ukazují, jak volat metodu serveru, který má návratovou hodnotu.

**Serverový kód pro metodu, která má návratovou hodnotu**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**Použitá pro třída Stock** návratová hodnota

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**Kód klienta, která volá metodu serveru (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**Kód klienta, která volá metodu serveru (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Postupy: zpracování události životnost připojení

Funkce SignalR poskytuje následující připojení životnost události, které může zpracovat:

- `starting`: Aktivována před odesláním libovolných dat přes připojení.
- `received`: Vyvolá, když je obdržena žádná data k připojení. Poskytuje přijatá data.
- `connectionSlow`: Vyvolá, když klient zjistí pomalé nebo často vyřazení připojení.
- `reconnecting`: Vyvolá, když základní přenos začne znovu obnovovat.
- `reconnected`: Vyvolá, když opětovně připojil základní přenos.
- `stateChanged`: Vyvolána při změně stavu připojení. Poskytuje stav starý a nový stav (připojení, připojeno, Reconnecting nebo odpojeno).
- `disconnected`: Vyvolá, když připojení byl odpojen.

Například, pokud chcete zobrazit upozornění, pokud nejsou potíže s připojením, které by mohly způsobit významnému zpoždění, zpracování `connectionSlow` událostí.

**Zpracovat událost connectionSlow (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**Zpracovat událost connectionSlow (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Další informace najdete v tématu [pochopení a zpracování události životnost připojení v systému SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Způsob zpracování chyb

Poskytuje SignalR JavaScript klienta `error` události, které můžete přidat obslužnou rutinu pro. Můžete také použít metodu služeb při selhání pro přidání obslužné rutiny pro chyby, které jsou výsledkem volání metody serveru.

Pokud nepovolíte explicitně podrobné chybové zprávy na serveru, obsahuje objekt výjimky, která SignalR vrací po chybě minimální informace o této chybě. Například, pokud volání `newContosoChatMessage` selže, obsahuje chybovou zprávu v objektu chyba "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" podrobné chybové zprávy pro klienty v produkčním prostředí se nedoporučuje z bezpečnostních důvodů, ale pokud chcete povolit podrobné chybové zprávy pro odesílání na serveru pro účely odstraňování potíží, použijte následující kód.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

Následující příklad ukazuje, jak přidat obslužnou rutinu události chyby.

**Přidejte obslužnou rutinu chyby (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Přidejte obslužnou rutinu chyby (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

Následující příklad ukazuje, jak zpracovat chybu z volání metody.

**Zpracování chyby z volání metody (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Zpracování chyby z volání metody (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

V případě selhání volání metody `error` také událost se vyvolá, takže kódu v `error` metoda obslužné rutiny a v `.fail` by provést metoda zpětného volání.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Jak povolit protokolování na straně klienta

Chcete-li povolit protokolování na straně klienta na připojení, nastavte `logging` vlastnost u objektu připojení před voláním `start` metodu pro vytvoření připojení.

**Povolit protokolování (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**Povolit protokolování (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Pokud chcete zobrazit protokoly, otevřete v prohlížeči na nástroje pro vývojáře a přejděte na kartu konzoly. Kurz, který zobrazuje podrobné pokyny a obrazovky snímky, které ukazují, jak to udělat, najdete v části [Server všesměrového vysílání pomocí funkce Signalr technologie ASP.NET - povolit protokolování](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).
