---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Obslužné rutiny zpráv HttpClient v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566341"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Obslužné rutiny zpráv HttpClient v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

A *obslužné rutiny zpráv* je třída, která přijímá požadavek HTTP a vrátí odpověď HTTP.

Řadu obslužné rutiny zpráv jsou obvykle zřetězen dohromady. První obslužná rutina obdrží požadavek HTTP, nemá nějaké zpracování a poskytuje požadavku na další obslužnou rutinu. Odpověď v určitém okamžiku se vytvoří a přejde zpět do řetězu. Tento vzor nazývá *delegování* obslužné rutiny.

![](httpclient-message-handlers/_static/image1.png)

Na straně klienta **HttpClient** třída používá obslužné rutiny zpráv pro zpracování požadavků. Výchozí obslužnou rutinu je **HttpClientHandler**, který odešle žádost přes síť a získá odpověď ze serveru. Obslužné rutiny zpráv vlastní můžete vložit do kanálu klienta:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> Rozhraní ASP.NET Web API používá také obslužné rutiny zpráv na straně serveru. Další informace najdete v tématu [obslužné rutiny zpráv HTTP](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Obslužné rutiny vlastní zpráv

Zápis obslužné rutiny zpráv vlastní, jsou odvozeny od **System.Net.Http.DelegatingHandler** a přepsat **SendAsync** metoda. Tady je podpis metody:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Tato metoda přebírá **HttpRequestMessage** jako vstup a asynchronně vrátí **objekt HttpResponseMessage**. Typická implementace provede následující akce:

1. Zpracovat zprávu požadavku.
2. Volání `base.SendAsync` odeslat požadavek na vnitřní obslužnou rutinu.
3. Vnitřní obslužná rutina vrátí zprávu odpovědi. (Tento krok je asynchronní.)
4. Zpracování odpovědi a vrátí volajícímu.

Následující příklad ukazuje popisovač zpráv, který přidá vlastní hlavičku odchozí žádost:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

Volání `base.SendAsync` je asynchronní. Pokud obslužná rutina nemá žádné pracovní po toto volání, použijte **await** – klíčové slovo Chcete-li pokračovat v provádění po dokončení metody. Následující příklad ukazuje obslužnou rutinu, která protokoly kódy chyb. Protokolování samotné není velmi zajímavé, ale tento příklad ukazuje, jak získat v odpovědi uvnitř obslužné rutiny.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Přidání obslužných rutin zpráv do kanálu klienta

Chcete-li přidat vlastní obslužné rutiny na **HttpClient**, použijte **HttpClientFactory.Create** metoda:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Obslužné rutiny zpráv se nazývají v pořadí, ve kterém můžete předat je do **vytvořit** metoda. Protože jsou vnořené obslužné rutiny, zprávu odpovědi přenáší opačným směrem. To znamená že poslední obslužná rutina je první, zobrazí se zpráva odpovědi.
