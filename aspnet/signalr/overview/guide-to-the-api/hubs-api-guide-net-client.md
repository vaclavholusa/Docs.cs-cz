---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: "Funkce SignalR technologie ASP.NET centra API Průvodce – klient .NET (C#) | Microsoft Docs"
author: pfletcher
description: "Tento dokument obsahuje úvod do používání centra rozhraní API pro SignalR verze 2 v rozhraní .NET klientů, jako jsou Windows Store (WinRT), WPF, Silverlight a nevýhody..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: a03c8c42622a768d706acf5ac1f23b37a830d426
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>Funkce SignalR technologie ASP.NET centra API Průvodce – klient .NET (C#)
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní Dykstra](https://github.com/tdykstra)

> Tento dokument obsahuje úvod do používání centra rozhraní API pro SignalR verze 2 v rozhraní .NET klientů, jako jsou Windows Store (WinRT), WPF, Silverlight a konzolové aplikace.
> 
> Rozhraní API rozbočovače SignalR umožňuje vytvářet vzdálená volání procedur (RPC) ze serveru pro připojené klienty a z klientů k serveru. V serverovém kódu můžete definovat metody, které lze volat klienty a volání metody, které běží na klientovi. V kódu klienta můžete definovat metody, které lze volat ze serveru a volání metody, které běží na serveru. SignalR se stará o všechny záležitosti klient server pro vás.
> 
> Funkce SignalR také nabízí nižší úrovně rozhraní API volat trvalé připojení. Úvod do SignalR, rozbočovače a trvalé připojení nebo kurz, který ukazuje, jak sestavit kompletní aplikaci SignalR, najdete v části [SignalR – Začínáme](../getting-started/index.md).
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

- [Instalace klienta](#clientsetup)
- [Postup připojení](#establishconnection)

    - [Připojení mezi doménami z klienty prostředí Silverlight](#slcrossdomain)
- [Postup konfigurace připojení](#configureconnection)

    - [Jak nastavit maximální počet souběžných připojení v klientech WPF](#maxconnections)
    - [Určení parametrů řetězce dotazu](#querystring)
    - [Určení metodu přenosu](#transport)
    - [Určení hlavičky protokolu HTTP](#httpheaders)
    - [Postup určení klientských certifikátů](#clientcertificate)
- [Jak vytvořit proxy server rozbočovače](#proxy)
- [Jak definovat metody na straně klienta, které můžete volat serveru](#callclient)

    - [Metody bez parametrů](#clientmethodswithoutparms)
    - [Metody s parametry, zadání typy parametrů](#clientmethodswithparmtypes)
    - [Metody s parametry, zadání dynamických objektů pro parametry](#clientmethodswithdynamparms)
    - [Postup odebrání obslužná rutina](#removehandler)
- [Jak volat metody serveru z klienta](#callserver)
- [Postupy: zpracování události životnost připojení](#connectionlifetime)
- [Způsob zpracování chyb](#handleerrors)
- [Jak povolit protokolování na straně klienta](#logging)
- [WPF, Silverlight a ukázky kódu aplikace konzoly pro klienta metody, které můžete volat serveru](#wpfsl)

Ukázkové klienta projekty rozhraní .NET naleznete v následujících zdrojích:

- [gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) na webu GitHub.com (WinRT, Silverlight, konzole aplikace příklady).
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) na webu GitHub.com (třeba WPF).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) na webu GitHub.com (třeba aplikace konzoly).

Pro dokumentaci o tom, jak program server nebo klientů JavaScript, najdete v následujících materiálech:

- [Rozhraní API Průvodce pro rozbočovače SignalR – Server](hubs-api-guide-server.md)
- [Rozhraní API Průvodce pro rozbočovače SignalR – JavaScript klienta](hubs-api-guide-javascript-client.md)

Odkazy na témata referenční dokumentace rozhraní API jsou na rozhraní .NET 4.5 verzi rozhraní API. Pokud používáte rozhraní .NET 4, přečtěte si téma [verze .NET 4 témat rozhraní API](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Instalace klienta

Nainstalujte [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) balíček NuGet (ne [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) balíčku). Tento balíček podporuje WinRT, Silverlight, WPF, konzolové aplikace a klienti Windows Phone pro rozhraní .NET 4 i rozhraní .NET 4.5.

Pokud je odlišná od verze, ke které máte na serveru verze funkce signalr, zda máte na straně klienta, je často moci přizpůsobit rozdílu SignalR. Například server se systémem SignalR verze 2 bude podporovat klienty, kteří mají nainstalované 1.1.x, jakož i klientů, kteří mají verze 2 nainstalován. Pokud je příliš velký rozdíl mezi verze na serveru a verzí v klientovi, nebo pokud je klient novější než na serveru, vyvolá SignalR `InvalidOperationException` výjimky, když se klient pokusí navázat připojení. Chybová zpráva "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Postup připojení

Předtím, než může vytvořit připojení, je nutné vytvořit `HubConnection` objektu a vytvořit proxy. K vytvoření připojení, volejte `Start` metodu `HubConnection` objektu.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> U klientů JavaScript, budete muset registraci alespoň jeden obslužné rutiny události před voláním `Start` metodu pro vytvoření připojení. To není nutné pro klienty .NET. Pro klienty, JavaScript, kód vygenerovaný proxy server automaticky vytvoří proxy pro všechny rozbočovače, které existují na serveru a registrace obslužná rutina je, jak určíte, které centra vašeho klienta v úmyslu použít. Ale pro klienta rozhraní .NET vytvořit proxy rozbočovače ručně, takže SignalR předpokládá, že budete používat každý rozbočovač, kterou vytvoříte proxy pro.


Ukázkový kód používá výchozí "/ signalr" adresa URL k připojení k službě SignalR. Informace o tom, jak zadejte jinou adresu URL základní najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – adresa URL /signalr](hubs-api-guide-server.md#signalrurl).

`Start` Asynchronně provede metodu. Chcete-li mít jistotu, další řádek kódu není spouštění až po připojení, použijte `await` v asynchronní metodu technologie ASP.NET 4.5 nebo `.Wait()` v synchronní metoda. Nepoužívejte `.Wait()` v klientovi WinRT.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Připojení mezi doménami z klienty prostředí Silverlight

Informace o tom, jak povolit připojení mezi doménami z klienty prostředí Silverlight naleznete v tématu [provádění služby k dispozici za domény hranicemi](https://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Postup konfigurace připojení

Před navázáním připojení, můžete zadat jakýkoli z následujících možností:

- Limit souběžných připojení.
- Parametry řetězce dotazu.
- Metoda přenosu.
- Hlavičky HTTP.
- Klientské certifikáty.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Jak nastavit maximální počet souběžných připojení v klientech WPF

V klientech WPF možná muset zvýšit maximální počet souběžných připojení z jeho výchozí hodnotu 2. Doporučená hodnota je 10.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Další informace najdete v tématu [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Určení parametrů řetězce dotazu

Pokud chcete odesílat data na server, když se klient připojí, můžete přidat parametrů řetězce dotazu pro objekt připojení. Následující příklad ukazuje, jak nastavit parametr řetězce dotazu v kódu klienta.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

Následující příklad ukazuje, jak číst parametr řetězce dotazu v serverovém kódu.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Určení metodu přenosu

Jako součást procesu připojení klienta SignalR normálně vyjedná se serverem určit nejlepší přenos, který je podporován server i klienta. Pokud již víte, které přenosu, kterou chcete použít, můžete tento proces vyjednávání obejít. Chcete-li zadat metodu přenosu, předejte objekt transport metody Start. Následující příklad ukazuje, jak chcete zadat metodu přenosu do kódu klienta.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/en-us/library/jj918090(v=vs.111).aspx) obor názvů zahrnuje následující třídy, které můžete použít k určení přenosu.

- [LongPollingTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (dostupné jenom v případě, že server i klienta pomocí rozhraní .NET 4.5.)
- [AutoTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (automaticky vybere nejlepší přenosu, který podporuje klient a server. Toto je výchozí přenos. To k předání `Start` metoda má stejný účinek jako není předávání v nic.)

Přenos ForeverFrame není součástí tohoto seznamu, protože se používá pouze pomocí prohlížeče.

Informace o tom, jak zkontrolovat metodu přenosu v serverovém kódu najdete v tématu [ASP.NET SignalR centra API Průvodce - Server – jak získat informace o klientovi z vlastností kontextu](hubs-api-guide-server.md#contextproperty). Další informace o přenosy a případech přejít najdete v tématu [Úvod do SignalR – přenosy a případech Přejít](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Určení hlavičky protokolu HTTP

Pokud chcete nastavit hlavičky HTTP, použijte `Headers` vlastnost na objekt připojení. Následující příklad ukazuje, jak přidat hlavičku protokolu HTTP.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Postup určení klientských certifikátů

Chcete-li přidat klientské certifikáty, použijte `AddClientCertificate` metodu na objekt připojení.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Jak vytvořit proxy server rozbočovače

Aby bylo možné definovat metody na straně klienta, který rozbočovač můžete volat ze serveru a k vyvolání metody v rozbočovači na serveru, vytvořit proxy server rozbočovače voláním `CreateHubProxy` na objekt připojení. Řetězec předáním `CreateHubProxy` je název vaší třídy rozbočovače nebo název určený správcem `HubName` atribut v případě, že bylo použito na serveru. Shoda názvů nerozlišuje.

**Třídy rozbočovače na serveru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Vytvořit proxy server klienta pro třídy rozbočovače**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Pokud uspořádání vaší třídy rozbočovače s `HubName` atribut, použijte tento název.

**Třídy rozbočovače na serveru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Vytvořit proxy server klienta pro třídy rozbočovače**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

Když zavoláte `HubConnection.CreateHubProxy` víckrát se stejným `hubName`, stejné uložené v mezipaměti získáte `IHubProxy` objektu.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Jak definovat metody na straně klienta, které můžete volat serveru

Metoda, která můžete volat serveru, použijte k proxy serveru `On` metody pro registraci obslužné rutiny události.

Metoda shoda názvů nerozlišuje. Například `Clients.All.UpdateStockPrice` na serveru, budou spuštěny `updateStockPrice`, `updatestockprice`, nebo `UpdateStockPrice` na straně klienta.

Různé klientské platformy mají různé požadavky na jak psát kód, metoda aktualizace uživatelského rozhraní. Příklady uvedené jsou pro klienty WinRT (Windows Store .NET). WPF, Silverlight a příklady aplikace konzoly jsou uvedeny v [samostatné části dále v tomto tématu](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Metody bez parametrů

Pokud jste zpracování metody nemá parametrů, použijte přetížení neobecnou `On` metoda:

**Volání metody klienta bez parametrů kódu serveru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Kód WinRT klienta pro metodu volat ze serveru bez parametrů ([WPF a Silverlight příklady později v tomto tématu najdete v části](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metody s parametry, zadání typy parametrů

Pokud jste zpracování metoda obsahuje parametry, zadejte typy parametrů jako obecné typy `On` metoda. Existují přetížení obecné `On` způsob povolení k zadání parametrů až 8 (4 na Windows Phone 7). V následujícím příkladu se posílá jeden parametr `UpdateStockPrice` metoda.

**Volání metody s parametrem kódu serveru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**Třída Stock použitý pro parametr**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**Kód WinRT klienta pro metodu volat ze serveru s parametrem ([WPF a Silverlight příklady později v tomto tématu najdete v části](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metody s parametry, zadání dynamických objektů pro parametry

Jako alternativu k zadání parametrů jako obecné typy `On` metodu, můžete zadat parametry jako dynamické objekty:

**Volání metody s parametrem kódu serveru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**Třída Stock použitý pro parametr**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**Kód WinRT klienta pro metodu volat ze serveru s parametrem, pomocí parametru dynamických objektů ([WPF a Silverlight příklady později v tomto tématu najdete v části](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Postup odebrání obslužná rutina

Chcete-li odebrat obslužnou rutinu, volejte jeho `Dispose` metoda.

**Kód klienta pro metodu s názvem ze serveru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Kód klienta odebrat obslužná rutina**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Jak volat metody serveru z klienta

Chcete-li zavolat metodu na serveru, použijte `Invoke` metodu na proxy server rozbočovače.

Pokud metodu se serverovou nemá žádnou návratovou hodnotu, použijte přetížení neobecnou `Invoke` metoda.

**Serverový kód pro metodu, která nemá žádnou návratovou hodnotu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Volání metody, která nemá žádnou návratovou hodnotu kódu klienta**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Pokud metodu se serverovou návratovou hodnotu, zadat návratový typ jako obecný typ `Invoke` metoda.

**Serverový kód pro metodu, která má návratovou hodnotu a přebírá parametr komplexního typu.**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**Třída Stock použitý pro parametr a vrátit hodnotu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**Volání metody, která má návratovou hodnotu a použití technologie ASP.NET 4.5 asynchronní metody přebírá parametr komplexního typu, kód klienta**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Volání metody, která má návratovou hodnotu a synchronní metoda přebírá parametr komplexního typu, kód klienta**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

`Invoke` Metoda asynchronně provede a vrátí `Task` objektu. Pokud nezadáte `await` nebo `.Wait()`, dalším řádku kódu, budou spuštěny před metodu, která vyvolání byl dokončen.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Postupy: zpracování události životnost připojení

Funkce SignalR poskytuje následující připojení životnost události, které může zpracovat:

- `Received`: Vyvolá, když je obdržena žádná data k připojení. Poskytuje přijatá data.
- `ConnectionSlow`: Vyvolá, když klient zjistí pomalé nebo často vyřazení připojení.
- `Reconnecting`: Vyvolá, když základní přenos začne znovu obnovovat.
- `Reconnected`: Vyvolá, když opětovně připojil základní přenos.
- `StateChanged`: Vyvolána při změně stavu připojení. Poskytuje stav starý a nový stav. Informace o připojení najdete v části hodnot stavu [ConnectionState výčtu](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Vyvolá, když připojení byl odpojen.

Například pokud chcete zobrazit zprávy upozornění pro chyby, které nejsou závažné, ale způsobit problémy s nepřerušované připojení, například jako pomalost nebo častému vyřazení připojení, zpracování `ConnectionSlow` událostí.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Další informace najdete v tématu [pochopení a zpracování události životnost připojení v systému SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Způsob zpracování chyb

Pokud nepovolíte explicitně podrobné chybové zprávy na serveru, obsahuje objekt výjimky, která SignalR vrací po chybě minimální informace o této chybě. Například, pokud volání `newContosoChatMessage` selže, obsahuje chybovou zprávu v objektu chyba "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" podrobné chybové zprávy pro klienty v produkčním prostředí se nedoporučuje z bezpečnostních důvodů, ale pokud chcete povolit podrobné chybové zprávy pro odesílání na serveru pro účely odstraňování potíží, použijte následující kód.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Zpracování chyb, které vyvolá SignalR, můžete přidat obslužnou rutinu pro `Error` událostí na objekt připojení.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Zpracování chyb z volání metod, zalomení kód v bloku try-catch.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Jak povolit protokolování na straně klienta

Chcete-li povolit protokolování na straně klienta, nastavte `TraceLevel` a `TraceWriter` vlastnosti na objekt připojení.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF, Silverlight a ukázky kódu aplikace konzoly pro klienta metody, které můžete volat serveru

Ukázky kódu, které jsou uvedené výše pro definování klienta metody, které můžete volat serveru k dispozici pro klienty WinRT. Následující ukázky zobrazit kód ekvivalentní pro WPF, Silverlight a klienty aplikace konzoly.

### <a name="methods-without-parameters"></a>Metody bez parametrů

**WPF kód klienta pro metodu s názvem ze serveru bez parametrů**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Kód klienta Silverlight pro metodu s názvem ze serveru bez parametrů**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Konzole application kód klienta pro metodu s názvem ze serveru bez parametrů**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metody s parametry, zadání typy parametrů

**WPF kód klienta pro metodu s názvem ze serveru s parametrem**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Kód klienta Silverlight pro metodu s názvem ze serveru s parametrem**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Konzole application kód klienta pro metodu s názvem ze serveru s parametrem**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metody s parametry, zadání dynamických objektů pro parametry

**WPF kód klienta pro metodu s názvem ze serveru s parametrem, pomocí parametru dynamických objektů**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Kód klienta Silverlight pro metodu s názvem ze serveru s parametrem, pomocí parametru dynamických objektů**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Konzole aplikace kód klienta pro metodu s názvem ze serveru s parametrem, pomocí dynamického objektu pro parametr**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
