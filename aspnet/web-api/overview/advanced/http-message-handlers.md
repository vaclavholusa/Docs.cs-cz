---
uid: web-api/overview/advanced/http-message-handlers
title: Obslužné rutiny zpráv HTTP ve webovém rozhraní API technologie ASP.NET | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: daf5bac8203beca3be0036b798188e373b02212a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394518"
---
<a name="http-message-handlers-in-aspnet-web-api"></a>Obslužné rutiny zpráv HTTP v rozhraní ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

A *obslužná rutina zprávy* je třída, která přijme požadavek HTTP a vrátí odpověď HTTP. Obslužné rutiny zpráv jsou odvozeny od abstraktní **objekt HttpMessageHandler** třídy.

Řada obslužné rutiny zpráv jsou obvykle zřetězen dohromady. První obslužná rutina požadavku HTTP, provádí nějaké zpracování a poskytuje k další obslužná rutina požadavku. V určitém okamžiku odpovědi se vytvoří a vrátí řetězem. Tento model se nazývá *delegování* obslužné rutiny.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Obslužné rutiny zpráv na straně serveru

Na straně serveru používá kanál rozhraní Web API některé vestavěné zprávy obslužné rutiny:

- **HttpServer** získá požadavku od hostitele.
- **HttpRoutingDispatcher** odešle požadavek na základě trasy.
- **HttpControllerDispatcher** odešle požadavek do kontroleru webového rozhraní API.

Vlastní obslužné rutiny můžete přidat do kanálu. Obslužné rutiny zpráv jsou vhodné pro vyskytující aspekty, které pracují na úrovni protokolu HTTP zprávy (spíše než akce kontroleru). Například může být obslužné rutiny zpráv:

- Číst nebo upravovat hlavičky žádosti.
- Přidání hlavičky odpovědi do odpovědi.
- Ověření požadavků dřív, než dorazí kontroleru.

Tento diagram znázorňuje dvě vlastní obslužné rutiny, které jsou vloženy do kanálu:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> Na straně klienta používá HttpClient také obslužné rutiny zpráv. Další informace najdete v tématu [obslužné rutiny zpráv HttpClient](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Obslužné rutiny vlastních zpráv

Zápis obslužné rutiny vlastních zpráv, jsou odvozeny z **System.Net.Http.DelegatingHandler** a přepsat **SendAsync** metody. Tato metoda má následující podpis:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

Tato metoda přebírá **HttpRequestMessage** jako vstup a asynchronně vrátí **objekt HttpResponseMessage**. Obvyklá implementace provede následující akce:

1. Zpracování zprávy s požadavkem.
2. Volání `base.SendAsync` odešlete žádost na vnitřní obslužnou rutinu.
3. Vnitřní obslužná rutina vrátí zprávu odpovědi. (Tento krok je asynchronní.)
4. Zpracování odpovědi a vrátí řízení volajícímu.

Zde je příklad jednoduchého dotazu:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> Volání `base.SendAsync` je asynchronní. Pokud obslužná rutina nemá žádnou práci po tomto volání, použijte **await** – klíčové slovo, jak je znázorněno.


Delegující obslužné rutiny můžete také přeskočit vnitřní obslužná rutina a vytvořit odpověď přímo:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Pokud delegování obslužná rutina vytvoří odpověď bez volání `base.SendAsync`, přeskočí požadavku rest z kanálu. To může být užitečné pro obslužnou rutinu, která ověří žádost (vytvoření chybová odpověď).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Přidání obslužné rutiny do kanálu

Pro přidání obslužné rutiny zpráv na straně serveru, přidejte obslužné rutiny **HttpConfiguration.MessageHandlers** kolekce. Pokud jste použili k vytvoření projektu šablony "Aplikace pro Web ASP.NET MVC 4", můžete k tomu tento uvnitř **WebApiConfig** třídy:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Jsou volány obslužné rutiny zpráv ve stejném pořadí, ve kterém se zobrazují v **MessageHandlers** kolekce. Vzhledem k tomu, že jsou vnořené, v opačném směru přenáší zprávy s odpovědí. To znamená že poslední obslužnou rutinou je první získá zprávu odpovědi.

Všimněte si, že není nutné nastavit vnitřní obslužných rutin; rozhraní Web API se automaticky připojí obslužné rutiny zpráv.

Pokud jste [s vlastním hostováním](../older-versions/self-host-a-web-api.md), vytvořte instanci **HttpSelfHostConfiguration** třídu a přidejte obslužné rutiny pro **MessageHandlers** kolekce.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Nyní Pojďme se podívat na několik příkladů obslužné rutiny vlastních zpráv.

## <a name="example-x-http-method-override"></a>Příklad: X-HTTP-Method-Override

X-HTTP-Method-Override je nestandardní hlavičku HTTP. Je určená pro klienty, kteří nemůže odesílat určité typy požadavků HTTP, jako je například PUT nebo DELETE. Místo toho klient odešle požadavek POST a nastaví hlavičku X-HTTP-Method-Override na požadovanou metodu. Příklad:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Tady je popisovač zpráv, který přidává podporu pro X-HTTP-Method-Override:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

V **SendAsync** metody obslužné rutiny kontroluje, zda je požadavek POST zprávy s požadavkem, a určuje, zda obsahuje hlavičku X-HTTP-Method-Override. Pokud ano, ověří hodnotu hlavičky a potom upraví metoda žádosti. A konečně, obslužná rutina zavolá `base.SendAsync` předat další obslužné rutiny zprávy.

Když požadavek dosáhne **HttpControllerDispatcher** třídy **HttpControllerDispatcher** bude směrovat žádosti založené na metodě aktualizace žádosti.

## <a name="example-adding-a-custom-response-header"></a>Příklad: Přidáte vlastní hlavičky odpovědi

Tady je popisovač zpráv, který přidá vlastní hlavičku každou zprávu s odpovědí:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Nejprve, obslužná rutina zavolá `base.SendAsync` k předání požadavku na vnitřní popisovač zprávy. Vnitřní obslužná rutina vrátí zprávu odpovědi, ale nepracuje tak asynchronně pomocí **úloh&lt;T&gt;**  objektu. Zprávy s odpovědí není k dispozici, dokud `base.SendAsync` se dokončí asynchronně.

V tomto příkladu **await** – klíčové slovo a provádí práci asynchronně po `SendAsync` dokončí. Pokud se zaměřujete na rozhraní .NET Framework 4.0, použijte **úloh**&lt;T&gt;**. ContinueWith** metody:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Příklad: Vyhledání klíče rozhraní API

Některé webové služby, aby klienti obsahovat klíč rozhraní API v žádosti. Následující příklad ukazuje, jak zkontrolovat požadavky na platný klíč rozhraní API obslužné rutiny zpráv:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Tato obslužná rutina hledá klíče rozhraní API v řetězci dotazu identifikátoru URI. (V tomto příkladu předpokládáme, že klíč je statický řetězec. Skutečná implementace by pravděpodobně používat složitější ověřování.) Pokud řetězec dotazu obsahuje klíč, obslužná rutina předá žádost vnitřní obslužná rutina.

Pokud požadavek nemá platný klíč, obslužná rutina vytvoří zprávu odpovědi se stavem 403, zakázáno. V tomto případě obslužná rutina nevolá `base.SendAsync`, takže vnitřní obslužná rutina nikdy obdrží požadavek ani neprovádí kontroleru. Proto kontroleru můžete předpokládá, že všechny žádosti přicházející platný klíč rozhraní API.

> [!NOTE]
> Klíč rozhraní API se vztahuje pouze na určité akce kontroleru, vezměte v úvahu místo obslužné rutiny zpráv filtru akce. Filtry akcí se spustí po URI směrování se provádí.


## <a name="per-route-message-handlers"></a>Obslužné rutiny zpráv za trasy

Obslužné rutiny ve **HttpConfiguration.MessageHandlers** kolekce platí globálně.

Alternativně můžete přidat obslužné rutiny zpráv na konkrétní postup při definování trasy:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

V tomto příkladu, pokud identifikátor URI požadavku odpovídá "Route2" požadavek je odeslána `MessageHandler2`. Následující diagram znázorňuje kanál pro tyto dvě trasy:

![](http-message-handlers/_static/image4.png)

Všimněte si, že `MessageHandler2` nahradí výchozí **HttpControllerDispatcher**. V tomto příkladu `MessageHandler2` vytvoří odpovědi a požadavky, které odpovídají "Route2" nikdy přejít na kontroler. Tímto způsobem můžete nahradit celý mechanismus kontroleru webového rozhraní API s vlastní koncový bod.

Alternativně můžete delegovat obslužné rutiny zpráv-route **HttpControllerDispatcher**, která pak odešle zprávu do kontroleru.

![](http-message-handlers/_static/image5.png)

Následující kód ukazuje postup při konfiguraci této trasy:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
