---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: "Funkce SignalR technologie ASP.NET centra API Průvodce – Server (C#) | Microsoft Docs"
author: pfletcher
description: "Tento dokument obsahuje úvod do programování na straně serveru rozhraní API ASP.NET SignalR centra pro SignalR verze 2, s ukázky kódu demonstraci..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c2567d4d39a494daf77a23db5dff83c8fae4925d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a>Funkce SignalR technologie ASP.NET centra API Průvodce – Server (C#)
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní Dykstra](https://github.com/tdykstra)

> Tento dokument obsahuje úvod do programování na straně serveru rozhraní API ASP.NET SignalR centra pro SignalR verze 2, s ukázky kódu demonstraci běžné možnosti.
> 
> Rozhraní API rozbočovače SignalR umožňuje vytvářet vzdálená volání procedur (RPC) ze serveru pro připojené klienty a z klientů k serveru. V serverovém kódu můžete definovat metody, které lze volat klienty a volání metody, které běží na klientovi. V kódu klienta můžete definovat metody, které lze volat ze serveru a volání metody, které běží na serveru. SignalR se stará o všechny záležitosti klient server pro vás.
> 
> Funkce SignalR také nabízí nižší úrovně rozhraní API volat trvalé připojení. Úvod do SignalR, rozbočovače a trvalé připojení, najdete v části [Úvod do SignalR 2](../getting-started/introduction-to-signalr.md).
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
> ## <a name="topic-versions"></a>Téma verze
> 
> Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Přehled

Tento dokument obsahuje následující části:

- [Postup registrace middlewaru SignalR.](#route)

    - [Adresa URL /signalr](#signalrurl)
    - [Konfigurace možností SignalR](#options)
- [Postup vytvoření a používání třídy rozbočovače](#hubclass)

    - [Doba života objektu rozbočovače](#transience)
    - [Formátu camelCase-velká a malá písmena rozbočovače názvů klientů JavaScript](#hubnames)
    - [Více rozbočovače](#multiplehubs)
    - [Silného typu rozbočovače](#stronglytypedhubs)
- [Jak definovat metody ve třídě rozbočovače, která můžete volat klientů](#hubmethods)

    - [Formátu camelCase-velká a malá písmena metoda názvů klientů JavaScript](#methodnames)
    - [Při spuštění asynchronně](#asyncmethods)
    - [Definování přetížení](#overloads)
    - [Vytvoření sestavy průběhu z volání metod rozbočovače](#progress)
- [Jak volat metody klienta z třídy rozbočovače](#callfromhub)

    - [Výběr klienty, kteří obdrží vzdálené volání Procedur](#selectingclients)
    - [Žádné ověření kompilace pro názvy – metoda](#dynamicmethodnames)
    - [S odpovídajícím názvem velká a malá písmena – metoda](#caseinsensitive)
    - [Asynchronního spuštění](#asyncclient)
- [Správa členství ve skupině z třídy rozbočovače](#groupsfromhub)

    - [Asynchronní provádění metody přidat a odebrat](#asyncgroupmethods)
    - [Trvalost členství ve skupině](#grouppersistence)
    - [Skupiny jednoho uživatele](#singleusergroups)
- [Postupy: zpracování události životnost připojení v třídy rozbočovače](#connectionlifetime)

    - [Kdy jsou volány onconnected rozbočovače, ondisconnected rozbočovače a onreconnected rozbočovače](#onreconnected)
    - [Stav volajícího není naplněno.](#nocallerstate)
- [Jak získat informace o klientovi z vlastností kontextu](#contextproperty)
- [Jak předat stavu mezi klienty a třídy rozbočovače](#passstate)
- [Postupy: zpracování chyb v třídy rozbočovače](#handleErrors)
- [Tom, jak volat metody klienta a Správa skupin z mimo třídy rozbočovače](#callfromoutsidehub)

    - [Volání metody klienta](#callingclientsoutsidehub)
    - [Správa členství ve skupině](#managinggroupsoutsidehub)
- [Postup povolení trasování](#tracing)
- [Postup přizpůsobení kanálu rozbočovače](#hubpipeline)

Dokumentaci k programu klientů najdete v následujících materiálech:

- [Rozhraní API Průvodce pro rozbočovače SignalR – JavaScript klienta](hubs-api-guide-javascript-client.md)
- [Rozhraní API Průvodce pro rozbočovače SignalR – klient .NET](hubs-api-guide-net-client.md)

Součásti serveru pro funkci SignalR 2 jsou dostupné jenom v rozhraní .NET 4.5. Servery se systémem .NET 4.0, musíte použít SignalR v1.x.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Postup registrace middlewaru SignalR.

Chcete-li definovat trasy, která budou klienti používat pro připojení do vašeho centra, volejte `MapSignalR` metoda při spuštění aplikace. `MapSignalR`je [metoda rozšíření](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) pro `OwinExtensions` třídy. Následující příklad ukazuje, jak definovat trasu rozbočovače SignalR pomocí třídy pro spuštění OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Pokud přidáváte funkce SignalR pro aplikaci ASP.NET MVC, ujistěte se, zda SignalR trasy, která je přidána před dalším postupům. Další informace najdete v tématu [kurz: Začínáme s SignalR 2 a MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>Adresa URL /signalr

Ve výchozím nastavení, je adresa URL trasy, který budou klienti používat pro připojení do vašeho centra "/ signalr". (Nepleťte si tuto adresu URL s adresou URL "/ signalr/hubs", což je pro automaticky generované soubor JavaScript. Další informace o vygenerovaný proxy server, najdete v části [průvodce rozhraní API rozbočovače SignalR - JavaScript klienta - vygenerovaný proxy server a jakým způsobem vám](hubs-api-guide-javascript-client.md#genproxy).)

Může být výjimečných případech, které bylo není použít tuto základní adresu URL pro SignalR; například máte složku v projektu s názvem *signalr* a nechcete, aby změna názvu. V takovém případě můžete změnit základní adresu URL, jak je znázorněno v následujících příkladech (nahraďte "/ signalr" v ukázkovém kódu s požadovanou adresu URL).

**Kód serveru, který určuje adresu URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Kód jazyka JavaScript klienta, který určuje adresu URL (s vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Kód jazyka JavaScript klienta, který určuje adresu URL (bez vygenerovaný proxy server)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Kód klienta rozhraní .NET, který určuje adresu URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Konfigurace možností SignalR

Přetížení `MapSignalR` metoda umožňují zadejte vlastní adresu URL, překladač závislostí vlastní a následující možnosti:

- Povolte volání mezi doménami pomocí CORS nebo JSONP z prohlížečů klientů.

    Obvykle Pokud prohlížeč načte stránky z `http://contoso.com`, připojení SignalR je ve stejné doméně, v `http://contoso.com/signalr`. Pokud stránku z `http://contoso.com` vytvoří připojení k `http://fabrikam.com/signalr`, který je připojení mezi doménami. Z bezpečnostních důvodů jsou ve výchozím nastavení zakázány připojení mezi doménami. Další informace najdete v tématu [ASP.NET SignalR centra API Průvodce - JavaScript klienta - postup připojení mezi doménami](hubs-api-guide-javascript-client.md#crossdomain).
- Povolte podrobné chybové zprávy.

    Pokud dojde k chybám, je výchozí chování SignalR klientům odeslat zprávu oznámení bez podrobnosti o co se stalo. Odesílání podrobné informace o chybě klientům se nedoporučuje v produkčním prostředí, protože uživatelé se zlými úmysly pravděpodobně moci použijte informace v útoky na vaší aplikace. Řešení potíží, můžete použít tuto možnost dočasně povolit informativnější zasílání zpráv o chybách.
- Zakažte automaticky generované soubory proxy server JavaScript.

    Ve výchozím nastavení je soubor s proxy JavaScript pro třídy rozbočovače vygenerované v reakci na adresu "/ signalr/hubs". Pokud nechcete použít třídy proxy JavaScript, nebo pokud chcete generovat ručně tento soubor a odkazovat na fyzický soubor v vašim klientům, můžete tuto možnost zakažte generování proxy serveru. Další informace najdete v tématu [průvodce rozhraní API rozbočovače SignalR - JavaScript klienta - vytvoření fyzického souboru pro funkci SignalR generované proxy](hubs-api-guide-javascript-client.md#manualproxy).

Následující příklad ukazuje, jak určit adresu URL připojení SignalR a tyto možnosti ve volání `MapSignalR` metoda. Chcete-li zadat vlastní adresu URL, nahraďte "/ signalr" v příkladu s adresou URL, kterou chcete použít.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Postup vytvoření a používání třídy rozbočovače

K vytvoření rozbočovač, vytvořte třídu, která je odvozena z [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). Následující příklad ukazuje jednoduchý třídy rozbočovače pro chatovací aplikace.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

V tomto příkladu můžete volat připojený klient `NewContosoChatMessage` metoda, a pokud ano, je přijatá data vysílali pro všechny připojené klienty.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Doba života objektu rozbočovače

Nepřidávejte vytvořit instanci třídy rozbočovače nebo volat jeho metody z vlastního kódu na serveru. všechny možnosti, které je potřeba pro vás kanálu rozbočovače SignalR. SignalR vytvoří novou instanci třídy vašeho centra pokaždé, když je potřeba zpracovat operaci rozbočovače, například když je klient připojí, odpojí nebo provede metodu volání na server.

Protože jsou přechodné instancí třídy rozbočovače, nelze je použít pro uchování stavu z volání jedné metody na další. Pokaždé, když server obdrží volání metody z klienta, novou instanci třídy procesů rozbočovače zprávu. Pro uchování stavu pomocí více připojení a volání metod, pomocí jiné metody, jako je například databáze, nebo proměnnou statické třídy rozbočovače nebo jinou třídu, která není odvozena od `Hub`. Pokud jste uchovávat data v paměti, pomocí metody například statické proměnné na třídy rozbočovače, data se ztratí recyklování domény aplikace.

Pokud chcete odesílat zprávy pro klienty z vlastní kód, který je spuštěn mimo třídy rozbočovače, nelze provést po vytvoření instance instance třídy rozbočovače, ale můžete provést získáním odkaz na objekt context SignalR pro vlastní třídy rozbočovače. Další informace najdete v tématu [jak volat metody klienta a spravovat skupiny z mimo třídy rozbočovače](#callfromoutsidehub) dál v tomto tématu.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Formátu camelCase-velká a malá písmena rozbočovače názvů klientů JavaScript

Ve výchozím nastavení najdete klientů JavaScript do centra pomocí názvu třídy verze ve formátu camelCase. SignalR tuto změnu automaticky provede tak, aby JavaScript konvence může odpovídat kódu jazyka JavaScript. V předchozím příkladu by se označuje jako `contosoChatHub` v kódu jazyka JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**JavaScript klienta pomocí vygenerovaný proxy server**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Pokud chcete zadat jiný název pro klienty použít, přidejte `HubName` atribut. Při použití `HubName` atributu, se nezměnila název na formát camelCase na klientech JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**JavaScript klienta pomocí vygenerovaný proxy server**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Více rozbočovače

V aplikaci můžete definovat více třídy rozbočovače. Pokud můžete udělat, připojení se sdílí, ale jsou samostatné skupiny:

- Budou všichni klienti používat stejnou adresu URL k navázání připojení SignalR s služby ("/ signalr" nebo vaše vlastní adresu URL, pokud byl zadán), a zda připojení se používá pro všechny rozbočovače definované službu.

    Neexistuje žádné rozdíly ve výkonnosti pro více rozbočovače ve srovnání s definování všechny funkce rozbočovače do jedné třídy.
- Všechny rozbočovače získat stejné informace o požadavku HTTP.

    Vzhledem k tomu, že všechny rozbočovače sdílet stejné připojení, je pouze informace žádosti HTTP, který server bude, co se dodává v původní požadavek HTTP, který vytvoří připojení SignalR. Pokud používáte k předávání informací z klienta na server tak, že zadáte řetězec dotazu požadavku na připojení, nemůžete zadat odlišné řetězce dotazu do různých rozbočovače. Stejné informace se zobrazí všechny rozbočovače.
- Vygenerovaný soubor proxy JavaScript bude obsahovat proxy pro všechny rozbočovače v jednom souboru.

    Informace o třídy proxy JavaScript najdete v tématu [průvodce rozhraní API rozbočovače SignalR - JavaScript klienta - vygenerovaný proxy server a jakým způsobem vám](hubs-api-guide-javascript-client.md#genproxy).
- Skupiny jsou definovány v rámci centra.

    V systému SignalR, které můžete definovat název skupiny pro všesměrové vysílání pro podmnožiny připojených klientů. Skupiny jsou udržovány odděleně pro každý rozbočovač. Například skupina s názvem "Administrators" by mělo zahrnovat jednu sadu klientů pro vaše `ContosoChatHub` třídy a stejný název skupiny by se označují jinou sadu klientů pro vaše `StockTickerHub` třídy.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Silného typu rozbočovače

Chcete-li definovat rozhraní pro vaše Centrum metody, které může váš klient odkaz (a povolení technologie Intellisense pro vaše Centrum metody), nemají centra z `Hub<T>` (dostupné ve verzi SignalR 2.1) místo `Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Jak definovat metody ve třídě rozbočovače, která můžete volat klientů

Ke zveřejnění metodu v rozbočovači, kterou chcete být možné volat z klienta, deklarujte veřejnou metodu, jak je znázorněno v následujících příkladech.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Můžete zadat návratový typ a parametry, včetně komplexní typy a pole, jako byste v libovolné metody C#. Žádná data, která přijímat parametry nebo vrací volajícímu informace se předávají mezi klientem a serverem pomocí JSON a SignalR automaticky zpracovává vazba na komplexní objekty a pole objektů.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Formátu camelCase-velká a malá písmena metoda názvů klientů JavaScript

Ve výchozím nastavení klienti JavaScript odkazovat na metod rozbočovače pomocí ve formátu camelCase verzi název metody. SignalR tuto změnu automaticky provede tak, aby JavaScript konvence může odpovídat kódu jazyka JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**JavaScript klienta pomocí vygenerovaný proxy server**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Pokud chcete zadat jiný název pro klienty použít, přidejte `HubMethodName` atribut.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**JavaScript klienta pomocí vygenerovaný proxy server**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Při spuštění asynchronně

Pokud metoda bude se dlouho běžící nebo pracovat, by zahrnovat čekání, jako je vyhledávání v databázi nebo volání webové služby, zkontrolujte asynchronní metody rozbočovače vrácením [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (místě `void` vrátit) nebo [ Úloha&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) objektu (místě `T` návratový typ). Když se vrátíte `Task` objekt z metody SignalR čeká `Task` k dokončení, a pak pošle rozbalenou výsledek zpět do klienta, takže není žádný rozdíl v tom, jak kód volání metody, které v klientovi.

Provedení metody rozbočovače asynchronní zabraňuje blokování připojení, když používá přenos protokolu WebSocket. Když metoda rozbočovače spouští synchronně a přenosu je WebSocket, následných volání metod rozbočovače ze stejného klienta jsou zablokovány, dokud se nedokončí metody rozbočovače.

Následující příklad ukazuje stejnou metodu zakódované spouští synchronně nebo asynchronně, za nímž následuje kód JavaScript klienta, který se dá použít pro volání buď verze.

**Synchronní**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Asynchronous**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**JavaScript klienta pomocí vygenerovaný proxy server**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Další informace o tom, jak použít asynchronní metody v technologii ASP.NET 4.5 najdete v tématu [použití asynchronních metod v architektuře ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definování přetížení

Pokud chcete definovat přetížení pro metodu, musí být jiný počet parametrů v každé přetížení. Pokud jste právě zadáním různé typy parametrů rozlišit přetížení, bude kompilovat vaší třídy rozbočovače ale služby SignalR vyvolá výjimku za běhu, když se klienti pokusí volání, jedno z přetížení.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Vytvoření sestavy průběhu z volání metod rozbočovače

SignalR 2.1 přidává podporu pro [průběh reporting vzor](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) byla zavedená v rozhraní .NET 4.5. K implementaci průběh vytváření sestav, definování `IProgress<T>` parametru pro metodu rozbočovače, kterým může přistupovat vašeho klienta:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Při zápisu metodu dlouho běžící serveru, je důležité použít asynchronní programování vzor jako asynchronní / Await místo blokování vláken rozbočovače.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Jak volat metody klienta z třídy rozbočovače

Chcete-li volat metody klienta ze serveru, použijte `Clients` vlastnost metodu v třídě rozbočovače. Následující příklad ukazuje kód serveru, který volá `addNewMessageToPage` na všechny připojené klienty a klientský kód, který definuje metodu v klientovi JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

**JavaScript klienta pomocí vygenerovaný proxy server**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

Nelze získat návratovou hodnotu z klienta metody; syntaxe jako `int x = Clients.All.add(1,1)` nefunguje.

Můžete zadat komplexní typy a pole parametrů. Následující příklad předá do klienta v parametru metody komplexního typu.

**Kód serveru, který volá metodu klienta pomocí komplexní objekt**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Kód serveru, který definuje komplexní objekt**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**JavaScript klienta pomocí vygenerovaný proxy server**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Výběr klienty, kteří obdrží vzdálené volání Procedur

Vrátí vlastnost klientů [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) objekt, který poskytuje několik možností pro zadání klienty, kteří obdrží vzdálené volání Procedur:

- Všichni připojení klienti.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Pouze volajícího klienta.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Všichni klienti s výjimkou volajícího klienta.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Konkrétním klientovi identifikovaný ID připojení.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    Tento příklad volá `addContosoChatMessageToPage` na volajícího klienta a má stejný účinek jako použití `Clients.Caller`.
- Všichni připojení klienti s výjimkou zadaných klientů, identifikovaný ID připojení.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Všechny připojené klienty v zadané skupině.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Všechny připojené klienty v zadané skupině s výjimkou zadaných klientů, identifikovaný ID připojení.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Všechny připojené klienty v zadané skupině s výjimkou volajícího klienta.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Konkrétní uživatele identifikovaného parametrem ID uživatele.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    Ve výchozím nastavení je to `IPrincipal.Identity.Name`, ale to se dá změnit pomocí [registrace implementace IUserIdProvider s globální hostitelem](mapping-users-to-connections.md#IUserIdProvider).
- Všichni klienti a skupiny v seznamu ID připojení.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Seznam skupin.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Uživatel podle názvu.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Seznam názvů uživatele (dostupné ve verzi SignalR 2.1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Žádné ověření kompilace pro názvy – metoda

Název metody, který zadáte interpretována jako dynamický objekt, což znamená, že není žádná IntelliSense nebo kompilace přijme se. Vyhodnotí výraz v době běhu. Při volání metody, které spouští, se k němu předávají SignalR odešle klientovi název metody a hodnoty parametru, a pokud má klient metodu, která odpovídá názvu, že metoda je volána a hodnoty parametru. Pokud žádná odpovídající metoda nenajde na straně klienta, je vyvolána žádná chyba. Informace o formátu dat, která SignalR odesílá klientovi na pozadí při volání metody klienta najdete v tématu [Úvod do SignalR](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>S odpovídajícím názvem velká a malá písmena – metoda

Metoda shoda názvů nerozlišuje. Například `Clients.All.addContosoChatMessageToPage` na serveru, budou spuštěny `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, nebo `addContosoChatMessageToPage` na straně klienta.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Asynchronního spuštění

Asynchronně provede metodu, kterou je možné volat. Kód, který se dodává po volání metody pro klienta, budou spuštěny okamžitě bez čekání na SignalR k dokončení přenosu dat do klientů, pokud je určit, že další řádek kódu by měla čekání na dokončení metody. Následující příklad kódu ukazuje, jak provést dvě metody klienta postupně.

**Pomocí operátoru Await (.NET 4.5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Pokud používáte `await` počkat, dokud nebude dokončeno metodu klientské před dalším řádku kódu, který nemusí znamenat, že klienti budou ve skutečnosti zobrazí se zpráva před dalším řádku kódu. "Dokončení" volání metody klienta pouze znamená, že SignalR provedla všechno potřebné k odeslání zprávy. Pokud potřebujete, aby klienti zobrazila zpráva ověření, budete muset tento mechanismus programu sami. Například může kód `MessageReceived` metodu v rozbočovači a v `addContosoChatMessageToPage` metody na straně klienta může volat `MessageReceived` potom činnost, kterou je potřeba udělat na straně klienta. V `MessageReceived` v centru můžete provést, ať pracovní závisí na příjem skutečné klienta a zpracování původní volání metody.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Postup řetězec proměnnou použít jako název metody

Pokud chcete vyvolat metodu klienta pomocí proměnné řetězec jako název metody, přetypovat `Clients.All` (nebo `Clients.Others`, `Clients.Caller`atd) k `IClientProxy` a pak zavolají [Invoke (methodName, argumentů...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Správa členství ve skupině z třídy rozbočovače

Skupiny v systému SignalR poskytují metodu pro zprávy všesměrové vysílání pro zadaný podmnožiny připojených klientů. Skupina může mít libovolný počet klientů a klienta může být členem skupiny libovolný počet skupin.

Chcete-li spravovat členství ve skupinách, použijte [přidat](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) a [odebrat](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metody poskytované `Groups` vlastnost třídy rozbočovače. Následující příklad ukazuje `Groups.Add` a `Groups.Remove` metody používané v metodách rozbočovače, které se nazývají kódem na straně klienta, za nímž následuje kód JavaScript klienta, který volá je.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**JavaScript klienta pomocí vygenerovaný proxy server**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

Nemáte nemusel vytvářet skupiny. Platí skupina se automaticky vytvoří při prvním zadejte její název ve volání `Groups.Add`, a bude odstraněn, když odeberete poslední připojení z členství v ní.

Neexistuje žádné rozhraní API pro získání seznamu členství ve skupinách nebo seznam skupin. SignalR odešle zprávy do klientů a skupin na základě [pub nebo sub modelu](http://en.wikipedia.org/wiki/Publish/subscribe), a server neumožňuje spravovat seznam skupin nebo členství ve skupinách. To pomáhá maximalizovat škálovatelnost, protože při každém přidání uzlu do webové farmy, nějaký stav, který udržuje SignalR musí být rozšířena do nového uzlu.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Asynchronní provádění metody přidat a odebrat

`Groups.Add` a `Groups.Remove` metody provedení asynchronně. Pokud chcete okamžitě odeslání zprávy do klienta pomocí skupiny a přidání klienta do skupiny, budete muset Ujistěte se, že `Groups.Add` metoda nejprve dokončí. Následující příklad kódu ukazuje, jak to provést.

**Přidání klienta do skupiny a pak zasílání zpráv tohoto klienta**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Trvalost členství ve skupině

SignalR sleduje připojení, nikoli uživatelům, takže když chcete, aby uživatel být ve stejné skupině pokaždé, když uživatel naváže připojení, budete muset volat `Groups.Add` pokaždé, když uživatel vytvoří nové připojení.

Po k dočasné ztrátě připojení někdy SignalR můžete obnovit připojení automaticky. V takovém případě SignalR obnovuje stejné připojení, není vytvořeno nové připojení, a proto je členství ve skupině klienta automaticky obnoví. To lze provést i v případě, že dočasné přerušení je výsledek restartování serveru nebo selhání, protože stav připojení u jednotlivých klientů, včetně členství ve skupinách, je zpětné odbavení klientovi. Pokud se server přestane fungovat a je nahrazena nový server, než vyprší časový limit připojení, můžete klienta automaticky opětovně připojit k novému serveru a znovu zaregistrovat ve skupinách, které je členem skupiny.

Při připojení nelze obnovit automaticky po ztrátě připojení, nebo pokud vyprší časový limit připojení, nebo pokud se klient neodpojí (například pokud prohlížeč přejde na novou stránku), členství ve skupinách budou ztraceny. Při příštím připojení uživatele bude nové připojení. Chcete-li udržovat členství ve skupinách, pokud stejný uživatel vytvoří nové připojení, aplikace má sledovat přidružení mezi uživatelů a skupin a členství ve skupinách obnovení pokaždé, když uživatel vytvoří nové připojení.

Další informace o připojení a opětovná připojení najdete v tématu [zpracování událostí životnost připojení v třídy rozbočovače](#connectionlifetime) dál v tomto tématu.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Skupiny jednoho uživatele

Aplikace, které používají SignalR obvykle mají ke sledování asociace mezi uživateli a připojení k vědět, který uživatel odeslal zprávu a které jiní uživatelé měli přijímání zprávy. Skupiny se používají v jednom ze dvou běžně používané vzorků pro učinit.

- Skupiny jednoho uživatele.

    Můžete zadat uživatelské jméno jako je název skupiny a přidejte aktuální ID připojení ke skupině pokaždé, když se uživatel připojí nebo znovu připojí. K odeslání zprávy uživateli odeslat do skupiny. Nevýhody této metody je, že skupiny není poskytují způsob, jak zjistit, zda uživatel je online nebo offline.
- Sledování přidružení mezi uživatelská jména a ID připojení.

    Můžete ukládat přidružení mezi každé uživatelské jméno a jeden nebo více připojení ID slovník nebo databáze a aktualizovat uložená data pokaždé, když uživatel připojí nebo odpojí. Pokud chcete odesílat zprávy uživateli zadáte ID připojení. Nevýhody této metody je, že trvá více paměti.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Postupy: zpracování události životnost připojení v třídy rozbočovače

Typické důvody pro zpracování události životnost připojení jsou ke sledování zda je uživatel připojen nebo Ne a ke sledování přidružení mezi uživatelská jména a ID připojení. Chcete-li spustit vlastní kód, když klienti připojení nebo odpojení, přepište `OnConnected`, `OnDisconnected`, a `OnReconnected` třídy virtuální metody rozbočovače, jak je znázorněno v následujícím příkladu.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Kdy jsou volány onconnected rozbočovače, ondisconnected rozbočovače a onreconnected rozbočovače

Pokaždé, když prohlížeč přejde na novou stránku, nové připojení musí být zavedena, což znamená, že budou spuštěny SignalR `OnDisconnected` metoda následuje `OnConnected` metoda. SignalR vytvoří nové ID připojení vždy, když je vytvořeno nové připojení.

`OnReconnected` Metoda je volána, když bude zjištěna dočasné přerušení připojení SignalR automaticky obnovení v případě, například když kabelu dočasně odpojení a opětovném připojení, než vyprší časový limit připojení. `OnDisconnected` Metoda je volána, když je klient odpojen a SignalR nelze znovu připojit automaticky, například pokud prohlížeč přejde na novou stránku. Proto je možné pořadí událostí pro daného klienta `OnConnected`, `OnReconnected`, `OnDisconnected`; nebo `OnConnected`, `OnDisconnected`. Neuvidíte sekvenci `OnConnected`, `OnDisconnected`, `OnReconnected` pro dané připojení.

`OnDisconnected` Metoda nebude zavolána v některých případech, například když server ocitne mimo provoz nebo doména aplikace získá recykluje. Pokud jiný server obsahuje na řádku nebo doména aplikace dokončení jeho recyklace, může být někteří klienti moci znovu připojit a spusťte `OnReconnected` událostí.

Další informace najdete v tématu [pochopení a zpracování události životnost připojení v systému SignalR](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Stav volajícího není naplněno.

Metody obslužné rutiny události životnost připojení se nazývají ze serveru, což znamená, že žádný stav, který jste zadali v `state` objektu na straně klienta se nenaplní v `Caller` vlastnost na serveru. Informace o `state` objektu a `Caller` vlastnost, najdete v části [jak předat stavu mezi klienty a třídy rozbočovače](#passstate) dál v tomto tématu.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Jak získat informace o klientovi z vlastností kontextu

Chcete-li získat informace o klientovi, použijte `Context` vlastnost třídy rozbočovače. `Context` Vlastnost vrátí [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) objekt, který poskytuje přístup k následujícím informacím:

- ID připojení volajícího klienta.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    ID připojení je identifikátor GUID, který je přiřazen nástrojem SignalR (nelze zadat hodnotu v kódu). Neexistuje jeden ID připojení pro každé připojení a stejné připojení, které ID je využíváno všechny rozbočovače, pokud máte více Hubs v aplikaci.
- Data hlavičky protokolu HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Můžete také získat hlavičky protokolu HTTP z `Context.Headers`. Z důvodu pro několik odkazů na stejnou věc je, že `Context.Headers` byla vytvořena jako první, `Context.Request` vlastnost byla přidána novější, a `Context.Headers` se zachovává kvůli zpětné kompatibilitě.
- Data řetězce dotazu.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    Můžete také získat data řetězce dotazu z `Context.QueryString`.

    Řetězec dotazu, kterou můžete získat v této vlastnosti je ten, který byl použit s požadavkem HTTP, který vytvořili připojení SignalR. Konfigurace připojení, což je pohodlný způsob, jak předat data o klientovi z klienta na server, můžete přidat parametrů řetězce dotazu v klientovi. Následující příklad ukazuje jeden způsob, jak přidat řetězec dotazu v klientovi JavaScript, při použití vygenerovaný proxy server.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Další informace o nastavení parametrů řetězce dotazu, najdete v části příručky rozhraní API pro [JavaScript](hubs-api-guide-javascript-client.md) a [.NET](hubs-api-guide-net-client.md) klientů.

    Můžete najít metodu přenosu používá pro připojení v data řetězce dotazu, společně s některé hodnoty, které používá interně SignalR:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    Hodnota `transportMethod` bude "Websocket", "serverSentEvents", "foreverFrame" nebo "longPolling". Všimněte si, že pokud změnami tuto hodnotu `OnConnected` obslužná rutina události, v některých scénářích může nejdřív získat hodnotu přenosu, která není metodu konečné vyjednané přenosu pro připojení. V takovém případě metoda vyvolá výjimku a bude volána později, po vytvoření metodu konečné přenosu.
- Soubory cookie.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    Můžete získat také soubory cookie z `Context.RequestCookies`.
- Informace o uživateli.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- Položka HttpContext objektu žádosti:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Tuto metodu použijte místo získávání `HttpContext.Current` získat `HttpContext` objekt pro připojení SignalR.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Jak předat stavu mezi klienty a třídy rozbočovače

Poskytuje proxy serveru klienta `state` objektu, ve kterém můžete ukládat data, která chcete k přenosu na server s každou volání metody. Na serveru můžete přístup k těmto datům v `Clients.Caller` vlastnost v metodách rozbočovače, které se nazývají klienty. `Clients.Caller` Vlastnost není vyplněný pro metody obslužné rutiny události životnost připojení `OnConnected`, `OnDisconnected`, a `OnReconnected`.

Vytváření nebo aktualizaci dat v `state` objektu a `Clients.Caller` vlastnost funguje v obou směrech. Můžete aktualizovat hodnoty na serveru a je předán zpět do klienta.

Následující příklad ukazuje kód JavaScript klienta, která ukládá stav pro přenos na server s každé volání metody.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

Následující příklad ukazuje kód ekvivalent v rozhraní .NET klientovi.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

Ve třídě rozbočovače, můžete přístup k těmto datům v `Clients.Caller` vlastnost. Následující příklad ukazuje kód, který načte stav uvedené v předchozím příkladu.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Tento mechanismus pro zachování stavu není určena pro velké objemy dat, protože všechno, co jste zadali v `state` nebo `Clients.Caller` vlastnost je zpětné odbavení s každé volání metody. Je vhodné pro menší položek, jako jsou uživatelská jména nebo čítače.


V VB.NET nebo v rozbočovači silného typu, volající objekt stavu nelze přistupovat prostřednictvím `Clients.Caller`; místo toho použijte `Clients.CallerState` (dostupné ve verzi SignalR 2.1):

**Pomocí CallerState v jazyce C#**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Pomocí CallerState v jazyce Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Postupy: zpracování chyb v třídy rozbočovače

Zpracování chyb, které nastat ve vaší metody třídy rozbočovače, použijte jednu nebo více z následujících metod:

- Zalomení metoda kódu v try-catch – bloky a protokolu objekt výjimky. Pro účely ladění můžete odeslat výjimka klienta, ale pro zabezpečení se nedoporučuje důvodů odesílání podrobné informace pro klienty v produkčním prostředí.
- Vytvoření modulu kanálu rozbočovače, který zpracovává [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) metoda. Následující příklad ukazuje, kanálů modul, který protokoluje chyby, za nímž následuje kód v souboru Startup.cs, který se vloží do kanálu rozbočovače modul.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Použití `HubException` – třída (počínaje SignalR 2). Tato chyba může být vyvolána z jakékoli volání rozbočovače. `HubError` Konstruktor přebírá zprávu řetězec a objektu pro ukládání dodatečné údaje o chybě. SignalR se automaticky serializovat výjimka a odeslat ho do klienta, kde se bude používat odmítnout nebo selže volání metody rozbočovače.

    Následující ukázky kódu ukazují, jak má být vyvolána `HubException` během volání rozbočovače a postupy: zpracování výjimky na klienty JavaScript a rozhraní .NET.

    **Ukázka třídy HubException kódu serveru**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Kód jazyka JavaScript klienta demonstraci odpovědi k vyvolání HubException v rozbočovači**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Ukázka odpovědi k vyvolání HubException v rozbočovači kód klienta rozhraní .NET**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Další informace o modulech kanálu rozbočovače najdete v tématu [postup přizpůsobení kanálu rozbočovače](#hubpipeline) dál v tomto tématu.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Postup povolení trasování

Povolení trasování na straně serveru, přidejte system.diagnostics element do souboru Web.config, jak je znázorněno v tomto příkladu:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Když spustíte aplikaci v sadě Visual Studio, můžete zobrazit protokoly v **výstup** okno.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Tom, jak volat metody klienta a Správa skupin z mimo třídy rozbočovače

Volání metod klienta z jiné třídy než vaší třídy rozbočovače, získat odkaz na objekt context SignalR pro rozbočovač a použijte ho k volání metody na straně klienta nebo spravovat skupiny.

Následující ukázka `StockTicker` – třída získá objekt kontextu, uloží je v instanci třídy, ukládá instance třídy pomocí statické vlastnosti a používá kontext z instance třídy singleton k volání `updateStockPrice` na klienty, kteří jsou – metoda připojení k rozbočovači s názvem `StockTickerHub`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Pokud potřebujete použít více časy kontextu v dlohotrvající objektu, získat odkaz na jednou a uložit ji spíš než načítání opakovat. Získávání kontextu jednou zajistí, že SignalR odesílá zprávy klientům ve stejném pořadí, ve kterém zkontrolujte vaše metody rozbočovače klienta volání metod. Kurz, který ukazuje způsob použití kontext SignalR pro rozbočovač, najdete v části [Server všesměrového vysílání pomocí funkce SignalR technologie ASP.NET](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Volání metody klienta

Můžete určit, kteří klienti dostanou vzdálené volání Procedur, ale máte méně možností než při volání z třídy rozbočovače. Důvodem je, že kontextu není spojen s konkrétní volání od klienta, proto všechny metody, které vyžadují znalosti systému aktuální ID připojení, jako `Clients.Others`, nebo `Clients.Caller`, nebo `Clients.OthersInGroup`, nejsou k dispozici. K dispozici jsou následující možnosti:

- Všichni připojení klienti.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Konkrétním klientovi identifikovaný ID připojení.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Všichni připojení klienti s výjimkou zadaných klientů, identifikovaný ID připojení.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Všechny připojené klienty v zadané skupině.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Všechny připojené klienty v zadané skupině s výjimkou zadaných klientů, identifikovaný ID připojení.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Při volání do vaší třídy jiný Hub z metod ve třídě rozbočovače, můžete předat aktuální ID připojení a použít s `Clients.Client`, `Clients.AllExcept`, nebo `Clients.Group` k simulaci `Clients.Caller`, `Clients.Others`, nebo `Clients.OthersInGroup`. V následujícím příkladu `MoveShapeHub` třída předá ID připojení, které `Broadcaster` – třída tak, aby `Broadcaster` třídy můžete simulovat `Clients.Others`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Správa členství ve skupině

Pro správu skupin máte stejné možnosti jako v třídy rozbočovače.

- Přidání klienta do skupiny

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Odebrání klienta ze skupiny

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Postup přizpůsobení kanálu rozbočovače

SignalR umožňuje vložit do kanálu rozbočovače modul vlastní kód. Následující příklad ukazuje vlastní modul kanálu rozbočovače, který protokoluje každý příchozí volání metody dostali od klienta a odchozí volání metody vyvolání na straně klienta:

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

Následující kód na *Startup.cs* souboru zaregistruje modul pro spuštění v kanálu rozbočovače modul:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Existuje mnoho různých způsobů, které můžete přepsat. Úplný seznam najdete v tématu [HubPipelineModule metody](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
