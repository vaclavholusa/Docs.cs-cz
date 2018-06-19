---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: Příručka rozhraní API SignalR 1.x centra – klienta JavaScript | Microsoft Docs
author: pfletcher
description: Tento dokument obsahuje úvod do používání centra rozhraní API pro SignalR verze 1.1 v klientech JavaScript, například s prohlížeči a Windows Store (WinJS) applic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: f92470b2022f343cfd6d822abb255dc19947b4d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036918"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a>Příručka rozhraní API SignalR 1.x centra – JavaScript klienta
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní Dykstra](https://github.com/tdykstra)

> Tento dokument obsahuje úvod do používání centra rozhraní API pro SignalR v klientech JavaScript, například s prohlížeči a aplikace pro Windows Store (WinJS) verze 1.1.
> 
> Rozhraní API rozbočovače SignalR umožňuje vytvářet vzdálená volání procedur (RPC) ze serveru pro připojené klienty a z klientů k serveru. V serverovém kódu můžete definovat metody, které lze volat klienty a volání metody, které běží na klientovi. V kódu klienta můžete definovat metody, které lze volat ze serveru a volání metody, které běží na serveru. SignalR se stará o všechny záležitosti klient server pro vás.
> 
> Funkce SignalR také nabízí nižší úrovně rozhraní API volat trvalé připojení. Úvod do SignalR, rozbočovače a trvalé připojení nebo kurz, který ukazuje, jak sestavit kompletní aplikaci SignalR, najdete v části [SignalR – Začínáme](../getting-started/index.md).


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

- [Rozhraní API Průvodce pro rozbočovače SignalR – Server](../guide-to-the-api/hubs-api-guide-server.md)
- [Rozhraní API Průvodce pro rozbočovače SignalR – klient .NET](../guide-to-the-api/hubs-api-guide-net-client.md)

Odkazy na témata referenční dokumentace rozhraní API jsou na rozhraní .NET 4.5 verzi rozhraní API. Pokud používáte rozhraní .NET 4, přečtěte si téma [verze .NET 4 témat rozhraní API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Vygenerovaný proxy server a jakým způsobem vám

Můžete naprogramovat JavaScript klienta pro komunikaci se službou SignalR s nebo bez proxy server, který generuje SignalR pro vás. Proxy vykonává vám je zjednodušení syntaxe kódu můžete použít pro připojení, metody zápisu, které server volá, a volání metody na serveru.

Při psaní kódu pro volání metody server vygenerovaný proxy server umožňuje použijte syntaxi, která vypadá jako kdyby byly provádění místní funkce: můžete napsat `serverMethod(arg1, arg2)` místo `invoke('serverMethod', arg1, arg2)`. Syntaxe vygenerovaný proxy server také umožňuje okamžitou a srozumitelné chybě na straně klienta, pokud překlep metoda název serveru. A pokud ručně vytvoříte soubor, který definuje proxy serverů, můžete také získat podporu technologie IntelliSense pro psaní kódu, který volá metody server.

Předpokládejme například, že máte následující třídy rozbočovače na serveru:

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Následující příklady kódu ukazují, co JavaScript kódu vypadá jako pro vyvolání `NewContosoChatMessage` metoda na server a přijímá volání `addContosoChatMessageToPage` metoda ze serveru.

**S vygenerovaný proxy server**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Bez vygenerovaný proxy server**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Kdy použít vygenerovaný proxy server

Pokud chcete zaregistrovat více obslužných rutin událostí pro metodu klienta, který volá serveru, nemůžete použít vygenerovaný proxy server. Jinak můžete zvolit vygenerovaný proxy server nebo není založen na zúčastníte kódování. Pokud si zvolíte možnost nepoužít, nemusíte odkazovat na adresu "rozbočovače/signalr" v `script` elementu v kódu klienta.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Instalace klienta

Klient JavaScript vyžaduje odkazy na jQuery a soubor JavaScript SignalR core. JQuery verze musí být 1.6.4 nebo hlavní novější verze, například 1.7.2, 1.8.2 nebo 1.9.1. Pokud se rozhodnete použít vygenerovaný proxy server, musíte také odkaz k proxy serveru SignalR vygeneruje soubor JavaScript. Následující příklad ukazuje, jak může odkazy vypadat na stránce HTML, který používá vygenerovaný proxy server.

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

Tyto odkazy musí být zahrnuty v tomto pořadí: jQuery první, poslední SignalR core, po který a proxy SignalR.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Jak chcete-li dynamicky vygenerovaný proxy server

V předchozím příkladu je odkaz na proxy SignalR generované dynamicky generovaný kód JavaScript, není pro fyzický soubor. SignalR vytvoří kód jazyka JavaScript pro proxy server za chodu a slouží ke klientovi v odpovědi na adresu "/ signalr/hubs". Pokud zadáte jiný základní adresu URL pro připojení SignalR na serveru v vaší `MapHubs` metoda, adresa URL souboru dynamicky vygenerovaný proxy server je vaše vlastní adresu URL s "/ hubs" k němu připojen.

> [!NOTE]
> U klientů Windows 8 (pro Windows Store) JavaScript použijte místo dynamicky vygenerovaný soubor fyzické proxy. Další informace najdete v tématu [vytvoření fyzického souboru pro funkci SignalR generované proxy](#manualproxy) dál v tomto tématu.


V zobrazení syntaxe Razor rozhraní ASP.NET MVC 4 použijte k odkazování na kořenový adresář aplikace ve vaší odkaz na soubor proxy tilda:

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

Další informace o používání SignalR v MVC 4 najdete v tématu [Začínáme s SignalR a MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

V zobrazení syntaxe Razor rozhraní ASP.NET MVC 3, použijte `Url.Content` pro vaši informaci, soubor proxy serveru:

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

V aplikaci webových formulářů ASP.NET, použijte `ResolveClientUrl` pro vaše servery proxy odkaz na soubor nebo registrovat prostřednictvím ScriptManager pomocí relativní cesty aplikace kořenové (začíná tildou):

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

Obecně platí použijte stejnou metodu pro určení adresy URL "/ signalr/hubs", který používáte pro soubory šablon stylů CSS a JavaScript. Pokud zadáte adresu URL bez použití tildou, v některých scénářích vaše aplikace bude fungovat správně při testování v sadě Visual Studio pomocí služby IIS Express, ale selže s chybou 404 při nasazování do úplnou službu IIS. Další informace najdete v tématu **řešení odkazy na kořenové úrovni prostředky** v [webové servery v sadě Visual Studio pro webové projekty ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) na webu MSDN.

Při spuštění webového projektu v sadě Visual Studio 2012 v režimu ladění, a pokud používáte Internet Explorer jako prohlížeč, zobrazí se soubor proxy serveru v **Průzkumníku řešení** pod **dokumentů skriptu**, jak je znázorněno Následující obrázek.

![Soubor vygenerovaný proxy server JavaScript v Průzkumníku řešení](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

Chcete-li zobrazit obsah souboru, dvakrát klikněte na **hubs**. Pokud nepoužíváte sadu Visual Studio 2012 a prohlížeče Internet Explorer, nebo pokud nejsou v režimu ladění, můžete také získat obsah souboru tak, že přejde na adresu "/ signalR/hubs". Například, pokud vaše lokalita běží v `http://localhost:56699`, přejděte na `http://localhost:56699/SignalR/hubs` v prohlížeči.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Postup vytvoření fyzický soubor pro funkci SignalR generované proxy

Jako alternativu k dynamicky vygenerovaný proxy server můžete vytvořit fyzický soubor, který má kód proxy a tento soubor. Můžete to provést kontrolu nad ukládání do mezipaměti nebo sdružování chování nebo získat IntelliSense při programování volání metody server.

Pokud chcete vytvořit soubor proxy serveru, proveďte následující kroky:

1. Nainstalujte [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) balíček NuGet.
2. Otevřete příkazový řádek a přejděte do *nástroje* složku, která obsahuje soubor SignalR.exe. Složky nástroje je v následujícím umístění:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
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

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Připojení (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Ukázkový kód používá výchozí "/ signalr" adresa URL k připojení k službě SignalR. Informace o tom, jak zadejte jinou adresu URL základní najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – adresa URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

> [!NOTE]
> Za normálních okolností registraci obslužné rutiny událostí před voláním `start` metodu pro vytvoření připojení. Pokud chcete zaregistrovat některé obslužné rutiny událostí po navázání připojení, můžete to udělat, ale je nutné zaregistrovat alespoň jeden z vaší událostí handler(s) před voláním `start` metoda. Jeden důvodem je to, že může být mnoho Hubs v aplikaci, ale nebude chcete aktivovat `OnConnected` událostí na všechny rozbočovače, chcete-li pouze pomocí jedné z nich. Při připojení, je přítomnost metodu klienta na proxy server rozbočovače pro co informuje SignalR pro aktivaci `OnConnected` událostí. Pokud si nezaregistrujete všechny obslužné rutiny před voláním `start` metoda, bude možné volat metody v rozbočovači, ale rozbočovače na `OnConnected` nebude volána metoda a žádné metody klienta, který bude vyvolán ze serveru.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub je stejný objekt vytvoří tento $.hubConnection()

Jak je vidět v příkladech, při použití vygenerovaný proxy server, `$.connection.hub` odkazuje na objekt připojení. Toto je stejný objekt, který získáte při volání `$.hubConnection()` když nepoužíváte vygenerovaný proxy server. Kód vygenerovaný proxy server vytvoří připojení můžete spuštěním následujícího příkazu:

![Vytvoření připojení v souboru vygenerovaný proxy server](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

Pokud používáte vygenerovaný proxy server, můžete provést cokoli, co se `$.connection.hub` , které můžete provést s objektem připojení po nepoužíváte vygenerovaný proxy server.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Asynchronní provádění metody start

`start` Asynchronně provede metodu. Vrátí [jQuery odložené objekt](http://api.jquery.com/category/deferred-object/), což znamená, že můžete přidat funkce zpětného volání voláním metod `pipe`, `done`, a `fail`. Pokud máte kód, který chcete spustit po připojení, například volání metody server, vložte tento kód funkce zpětného volání nebo volání z funkce zpětného volání. `.done` Metoda zpětného volání je proveden po navázání připojení, a po žádný kód, který budete mít ve vaší `OnConnected` metoda obslužné rutiny událostí na serveru dokončí provádění.

Když vložíte příkaz "Teď připojený" z předchozího příkladu jako dalším řádku kódu po `start` volání metody (není v `.done` zpětného volání), `console.log` řádku, budou spuštěny před připojení, jak je znázorněno v následujícím Příklad:

![Nesprávný způsob, jak napsat kód, který spouští po navázání připojení](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Postup připojení mezi doménami

Obvykle Pokud prohlížeč načte stránky z `http://contoso.com`, připojení SignalR je ve stejné doméně, v `http://contoso.com/signalr`. Pokud stránku z `http://contoso.com` vytvoří připojení k `http://fabrikam.com/signalr`, který je připojení mezi doménami. Z bezpečnostních důvodů jsou ve výchozím nastavení zakázány připojení mezi doménami. Navázat připojení mezi doménami, ujistěte se, že jsou na serveru povoleno připojení mezi doménami a zadejte adresu URL připojení, když vytvoříte objekt připojení. SignalR bude odpovídající technologii použít pro připojení mezi doménami, například [JSONP](http://en.wikipedia.org/wiki/JSONP) nebo [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

Na serveru povolit připojení mezi doménami tak, že vyberete tuto možnost, při volání `MapHubs` metoda.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

Na klientovi zadejte adresu URL, když vytvoříte objekt připojení (bez vygenerovaný proxy server), nebo před voláním metody start (s vygenerovaný proxy server).

**Kód klienta, který určuje připojení mezi doménami (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**Kód klienta, který určuje připojení mezi doménami (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

Při použití `$.hubConnection` konstruktoru, nemusíte zahrnovat `signalr` v adrese URL je automaticky přidán (Pokud nezadáte `useDefaultUrl` jako `false`).

Můžete vytvořit více připojení k různým koncovým bodům.

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - Není nastavený `jQuery.support.cors` na hodnotu true v kódu.
> 
>     ![JQuery.support.cors není nastavený na hodnotu true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR zpracovává používání JSONP nebo CORS. Nastavení `jQuery.support.cors` na hodnotu true zakáže JSONP, protože způsobuje, že SignalR předpokládat, že je prohlížeč podporuje CORS.
> - Když se připojujete k adresu URL místního hostitele, Internet Explorer 10 nebude považuje za připojení mezi doménami, takže aplikace se pracovat místně aplikace Internet Explorer 10 i v případě, že jste nepovolili napříč doménami připojení na serveru.
> - Informace o používání připojení mezi doménami s aplikace Internet Explorer 9 najdete v tématu [tohoto podprocesu StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Informace o používání připojení mezi doménami s Chrome najdete v tématu [tohoto podprocesu StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Ukázkový kód používá výchozí "/ signalr" adresa URL k připojení k službě SignalR. Informace o tom, jak zadejte jinou adresu URL základní najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – adresa URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Postup konfigurace připojení

Před navázáním připojení, můžete určit parametrů řetězce dotazu nebo zadejte metodu přenosu.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Určení parametrů řetězce dotazu

Pokud chcete odesílat data na server, když se klient připojí, můžete přidat parametrů řetězce dotazu pro objekt připojení. Následující příklady ukazují, jak nastavit parametr řetězce dotazu v kódu klienta.

**Nastavte hodnotu řetězce dotazu před voláním metody start (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**Nastavte hodnotu řetězce dotazu před voláním metody start (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

Následující příklad ukazuje, jak číst parametr řetězce dotazu v serverovém kódu.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Určení metodu přenosu

Jako součást procesu připojení klienta SignalR normálně vyjedná se serverem určit nejlepší přenos, který je podporován server i klienta. Pokud již víte, které přenosu, kterou chcete použít, můžete tento proces vyjednávání obejít tak, že určíte metodu přenosu při volání `start` metoda.

**Kód klienta, který určuje způsob přepravy (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Kód klienta, který určuje způsob přepravy (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Jako alternativu můžete zadat několik metod přenosu v pořadí, ve kterém chcete SignalR pokusit je:

**Kód klienta, který určuje záložní schéma vlastní přenosu (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**Kód klienta, který určuje záložní schématu vlastní přenosu (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

Pro zadání metodu přenosu můžete použít následující hodnoty:

- "Websocket"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Následující příklady ukazují, jak zjistit, jakou metodu přenosu používá připojení.

**Kód klienta, který zobrazí metodu přenosu používá připojení (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**Kód klienta, který zobrazí metodu přenosu používá připojení (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

Informace o tom, jak zkontrolovat metodu přenosu v serverovém kódu najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – jak získat informace o klientovi z vlastností kontextu](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Další informace o přenosy a případech přejít najdete v tématu [Úvod do SignalR – přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Jak získat proxy server pro třídy rozbočovače

Každý objekt připojení, který vytvoříte zapouzdří informace o připojení pro službu SignalR, která obsahuje jednu nebo více tříd rozbočovače. Ke komunikaci s třídy rozbočovače, pomocí objektu proxy, která můžete vytvořit sami (Pokud nepoužíváte vygenerovaný proxy server) nebo který je pro vás vygeneroval.

Název proxy serveru na straně klienta je ve formátu camelCase verzi název třídy rozbočovače. SignalR tuto změnu automaticky provede tak, aby JavaScript konvence může odpovídat kódu jazyka JavaScript.

**Třídy rozbočovače na serveru**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**Získat odkaz na proxy serveru generovaného klienta pro rozbočovač**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**Vytvořit proxy server klienta pro třídy rozbočovače (bez vygenerovaný proxy server)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

Pokud uspořádání vaší třídy rozbočovače s `HubName` atribut, použít přesný název bez Změna velikosti písmen.

**Třídy rozbočovače na serveru s atributem HubName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**Získat odkaz na proxy serveru generovaného klienta pro rozbočovač**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**Vytvořit proxy server klienta pro třídy rozbočovače (bez vygenerovaný proxy server)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Jak definovat metody na straně klienta, které můžete volat serveru

Zadat metodu, která serveru můžete volat z rozbočovače, přidejte obslužnou rutinu události pro proxy server rozbočovače pomocí `client` vlastnost vygenerovaný proxy server nebo volání `on` metoda, pokud nepoužíváte vygenerovaný proxy server. Parametry lze komplexních objektů.

Přidání obslužné rutiny události před voláním `start` metodu pro vytvoření připojení. (Pokud chcete přidat obslužné rutiny událostí po volání `start` metodu, najdete v poznámce v [postup připojení](#establishconnection) dříve v tomto dokumentu a použijte syntaxi pro definování metoda bez použití vygenerovaný proxy server.)

Metoda shoda názvů nerozlišuje. Například `Clients.All.addContosoChatMessageToPage` na serveru, budou spuštěny `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, nebo `addcontosochatmessagetopage` na straně klienta.

**Zadejte metodu na klientovi (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**Alternativní způsob, jak definovat metoda na klientovi (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**Definovat metoda na klienta (bez vygenerovaný proxy server, nebo při přidávání po volání metody spuštění)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**Kód serveru, který volá metodu klienta**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

Následující příklady zahrnují komplexní objekt jako parametr metody.

**Zadejte metodu na klienta, který přebírá objekt komplexní (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**Zadejte metodu na klienta, který přebírá objekt komplexní (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**Kód serveru, který definuje komplexní objekt**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**Kód serveru, který volá metodu klienta pomocí komplexní objekt**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Jak volat metody serveru z klienta

Chcete-li zavolat metodu serveru z klienta, použijte `server` vlastnost vygenerovaný proxy server nebo `invoke` metodu na proxy server rozbočovače, pokud nepoužíváte vygenerovaný proxy server. Návratová hodnota nebo parametry můžou být komplexních objektů.

Předat ve stylu camelCase verzi název metody rozbočovače. SignalR tuto změnu automaticky provede tak, aby JavaScript konvence může odpovídat kódu jazyka JavaScript.

Následující příklady ukazují, jak volat metodu serveru, který nemá návratovou hodnotu a jak volat metodu serveru, která nemá návratovou hodnotu.

**Metoda serveru s žádný atribut HubMethodName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**Kód serveru, který definuje komplexní objekt předaný parametr**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**Kód klienta, která volá metodu serveru (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**Kód klienta, která volá metodu serveru (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

Pokud dekorované metody rozbočovače s `HubMethodName` atribut, použijte tento název bez Změna velikosti písmen.

**Metoda serveru** s atributem HubMethodName

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**Kód klienta, která volá metodu serveru (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**Kód klienta, která volá metodu serveru (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

Předchozí příklady ukazují, jak volat metodu serveru, který nemá žádnou návratovou hodnotu. Následující příklady ukazují, jak volat metodu serveru, který má návratovou hodnotu.

**Serverový kód pro metodu, která má návratovou hodnotu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**Použitá pro třída Stock** návratová hodnota

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**Kód klienta, která volá metodu serveru (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**Kód klienta, která volá metodu serveru (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

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

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**Zpracovat událost connectionSlow (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

Další informace najdete v tématu [pochopení a zpracování události životnost připojení v systému SignalR](index.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Způsob zpracování chyb

Poskytuje SignalR JavaScript klienta `error` události, které můžete přidat obslužnou rutinu pro. Můžete také použít metodu služeb při selhání pro přidání obslužné rutiny pro chyby, které jsou výsledkem volání metody serveru.

Pokud nepovolíte explicitně podrobné chybové zprávy na serveru, obsahuje objekt výjimky, která SignalR vrací po chybě minimální informace o této chybě. Například, pokud volání `newContosoChatMessage` selže, obsahuje chybovou zprávu v objektu chyba "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" podrobné chybové zprávy pro klienty v produkčním prostředí se nedoporučuje z bezpečnostních důvodů, ale pokud chcete povolit podrobné chybové zprávy pro odesílání na serveru pro účely odstraňování potíží, použijte následující kód.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

Následující příklad ukazuje, jak přidat obslužnou rutinu události chyby.

**Přidejte obslužnou rutinu chyby (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**Přidejte obslužnou rutinu chyby (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Následující příklad ukazuje, jak zpracovat chybu z volání metody.

**Zpracování chyby z volání metody (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**Zpracování chyby z volání metody (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

V případě selhání volání metody `error` také událost se vyvolá, takže kódu v `error` metoda obslužné rutiny a v `.fail` by provést metoda zpětného volání.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Jak povolit protokolování na straně klienta

Chcete-li povolit protokolování na straně klienta na připojení, nastavte `logging` vlastnost u objektu připojení před voláním `start` metodu pro vytvoření připojení.

**Povolit protokolování (s vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**Povolit protokolování (bez vygenerovaný proxy server)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

Pokud chcete zobrazit protokoly, otevřete v prohlížeči na nástroje pro vývojáře a přejděte na kartu konzoly. Kurz, který zobrazuje podrobné pokyny a obrazovky snímky, které ukazují, jak to udělat, najdete v části [Server všesměrového vysílání pomocí funkce Signalr technologie ASP.NET - povolit protokolování](index.md).
