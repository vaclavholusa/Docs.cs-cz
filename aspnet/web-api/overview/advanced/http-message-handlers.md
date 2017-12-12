---
uid: web-api/overview/advanced/http-message-handlers
title: "Obslužné rutiny zpráv HTTP v rozhraní ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="http-message-handlers-in-aspnet-web-api"></a>Obslužné rutiny zpráv HTTP v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

A *obslužné rutiny zpráv* je třída, která přijímá požadavek HTTP a vrátí odpověď HTTP. Obslužné rutiny zpráv jsou odvozeny od abstraktní **HttpMessageHandler** třídy.

Řadu obslužné rutiny zpráv jsou obvykle zřetězen dohromady. První obslužná rutina obdrží požadavek HTTP, nemá nějaké zpracování a poskytuje požadavku na další obslužnou rutinu. Odpověď v určitém okamžiku se vytvoří a přejde zpět do řetězu. Tento vzor nazývá *delegování* obslužné rutiny.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Obslužné rutiny zpráv na straně serveru

Na straně serveru používá kanál rozhraní Web API obslužné rutiny některé integrované zpráv:

- **HttpServer** získá požadavek z hostitele.
- **HttpRoutingDispatcher** odešle zprávu požadavku na základě trasy.
- **HttpControllerDispatcher** odešle žádost do kontroleru webového rozhraní API.

Můžete přidat vlastní obslužné rutiny do kanálu. Obslužné rutiny zpráv jsou vhodné pro vyjímání mezi aspekty, které fungují na úrovni HTTP zprávy (nikoli akce kontroleru). Například může obslužné rutiny zpráv:

- Čtení nebo úpravy hlavičky žádosti.
- Přidání hlavičky odpovědi do odpovědi.
- Ověření požadavků před nedostanou kontroleru.

Tento diagram zobrazuje dvě vlastní obslužné rutiny, které jsou vloženy do kanálu:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> Na straně klienta používá HttpClient také obslužné rutiny zpráv. Další informace najdete v tématu [obslužné rutiny zpráv HttpClient](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Obslužné rutiny vlastní zpráv

Zápis obslužné rutiny zpráv vlastní, jsou odvozeny od **System.Net.Http.DelegatingHandler** a přepsat **SendAsync** metoda. Tato metoda má následující podpis:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

Tato metoda přebírá **HttpRequestMessage** jako vstup a asynchronně vrátí **objekt HttpResponseMessage**. Typická implementace provede následující akce:

1. Zpracovat zprávu požadavku.
2. Volání `base.SendAsync` odeslat požadavek na vnitřní obslužnou rutinu.
3. Vnitřní obslužná rutina vrátí zprávu odpovědi. (Tento krok je asynchronní.)
4. Zpracování odpovědi a vrátí volajícímu.

Zde je jednoduchý příklad:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> Volání `base.SendAsync` je asynchronní. Pokud obslužná rutina nemá žádné pracovní po toto volání, použijte **await** – klíčové slovo, jak je vidět.


Delegování obslužnou rutinu můžete také přeskočit vnitřní obslužnou rutinu a vytvořit odpověď přímo:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Pokud delegování obslužná rutina vytvoří odpověď bez volání `base.SendAsync`, žádost přeskočí zbytek kanálu. To může být užitečné pro obslužnou rutinu, která ověří žádost (vytvoření chybnou odpověď).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Přidání obslužné rutiny do kanálu

Přidání obslužné rutiny zpráv na straně serveru, přidejte obslužnou rutinu do **HttpConfiguration.MessageHandlers** kolekce. Pokud jste použili šablony "ASP.NET MVC 4 webové aplikace" a vytvořte tak projekt, můžete to udělat tento uvnitř **WebApiConfig** třídy:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Obslužné rutiny zpráv se nazývají ve stejném pořadí, ve kterém se zobrazují v **MessageHandlers** kolekce. Protože se nejedná o vnořené, zprávu odpovědi přenáší opačným směrem. To znamená že poslední obslužná rutina je první, zobrazí se zpráva odpovědi.

Všimněte si, že nemusíte nastavit vnitřní obslužné rutiny; rozhraní Web API se automaticky připojí obslužné rutiny zpráv.

Pokud jste [vlastní hostování](../older-versions/self-host-a-web-api.md), vytvořte instanci **HttpSelfHostConfiguration** třídu a přidejte obslužné rutiny na **MessageHandlers** kolekce.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Nyní Podíváme se na některé příklady obslužné rutiny zpráv vlastní.

## <a name="example-x-http-method-override"></a>Příklad: X-HTTP-Method-Override

X-HTTP-Method-Override je nestandardní hlavičku HTTP. Je určený pro klienty, kteří nelze odeslat určité typy požadavků HTTP, například PUT nebo odstranit. Místo toho klient odešle požadavek POST a nastaví hlavičku X-HTTP-Method-Override na požadovanou metodu. Příklad:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Tady je popisovač zpráv, který přidává podporu pro X-HTTP-Method-Override:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

V **SendAsync** metody obslužná rutina kontroluje, zda zpráva požadavku je požadavek POST a zda obsahuje hlavičku X-HTTP-Method-Override. Pokud ano, ověří hodnotu hlavičky a pak upravením metodu požadavku. Nakonec volá obslužnou rutinu `base.SendAsync` předat zprávu další obslužné rutině.

Když požadavek dosáhne **HttpControllerDispatcher** třídy, **HttpControllerDispatcher** bude směrovat žádosti založené na metodě aktualizované požadavku.

## <a name="example-adding-a-custom-response-header"></a>Příklad: Přidání vlastní hlavičky odpovědi

Tady je popisovač zpráv, který přidá vlastní hlavičku každou zprávu s odpovědí:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Nejprve obslužná rutina volá `base.SendAsync` k předání požadavku do vnitřní popisovač zprávy. Vnitřní obslužná rutina vrátí zprávu odpovědi, ale nepracuje tak asynchronně pomocí **úloh&lt;T&gt;**  objektu. Zpráva odpovědi není dostupná, dokud nebude `base.SendAsync` dokončení asynchronně.

Tento příklad používá **await** – klíčové slovo pro práci asynchronně po `SendAsync` dokončení. Pokud cílíte na rozhraní .NET Framework 4.0, použijte **úloh**&lt;T&gt;**. ContinueWith** metoda:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Příklad: Kontrola klíč rozhraní API

Některé webové služby, aby klienti obsahovat klíč rozhraní API v jejich požadavku. Následující příklad ukazuje, jak obslužné rutiny zpráv můžete zkontrolovat požadavky na platný klíč rozhraní API:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Tato rutina vyhledá klíč rozhraní API v řetězci dotazu identifikátoru URI. (V tomto příkladu předpokládáme, že klíč je statický řetězec. Skutečné implementace by pravděpodobně používají složitější ověřování.) Pokud řetězec dotazu obsahuje klíč, obslužná rutina předá požadavek na vnitřní obslužnou rutinu.

Pokud požadavek nemá platný klíč, obslužná rutina vytvoří zprávu odpovědi, která se stavem 403, zakázáno. V takovém případě obslužná rutina nevyvolá `base.SendAsync`, takže vnitřní obslužnou rutinu nikdy obdrží požadavek ani neprovádí kontroleru. Proto řadičem můžete předpokládá, že všechny příchozí požadavky platný klíč rozhraní API.

> [!NOTE]
> Pokud klíč rozhraní API se vztahuje pouze na určité akce kontroleru, zvažte použití filtru akce místo obslužné rutiny zpráv. Filtry akcí spustit po URI směrování se provádí.


## <a name="per-route-message-handlers"></a>Obslužné rutiny zpráv za trasy

Obslužné rutiny v **HttpConfiguration.MessageHandlers** kolekce platí globálně.

Alternativně můžete přidat obslužné rutiny zpráv na určený postup při definování trasy:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

V tomto příkladu, pokud odpovídá identifikátoru URI požadavku "Route2", žádost je odeslaných `MessageHandler2`. Následující diagram znázorňuje kanálu pro tyto dvě cesty:

![](http-message-handlers/_static/image4.png)

Všimněte si, že `MessageHandler2` nahradí výchozí **HttpControllerDispatcher**. V tomto příkladu `MessageHandler2` vytvoří odpovědi a požadavky, které odpovídají "Route2" nikdy přejděte do řadiče. Díky tomu můžete nahradit vlastní koncový bod celého mechanismus kontroleru webového rozhraní API.

Alternativně můžete delegovat obslužné rutiny zpráv za trasy na **HttpControllerDispatcher**, který pak odešle zprávu do řadiče.

![](http-message-handlers/_static/image5.png)

Následující kód ukazuje, jak nakonfigurovat tento postup:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
