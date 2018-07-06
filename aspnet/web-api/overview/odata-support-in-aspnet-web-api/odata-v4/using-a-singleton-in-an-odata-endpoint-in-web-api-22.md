---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Vytvoření jednoho prvku v OData v4 použití rozhraní Web API 2.2 | Dokumentace Microsoftu
author: rick-anderson
description: Toto téma ukazuje, jak definovat typ singleton v koncový bod OData v Web API 2.2.
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: baccfed79949ae4efd45395bed3ad0058549cb4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830513"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Vytvoření jednoho prvku v OData v4 použití rozhraní Web API 2.2
====================
podle Zoe Luo

> Tradičně entita může přistupovat pouze pokud byly zapouzdřený v sadu entit. Ale OData v4 poskytuje dvě další možnosti, jednotlivý prvek a členství ve skupině, které podporuje WebAPI 2.2.


Tento článek ukazuje, jak definovat typ singleton v koncový bod OData v Web API 2.2. Informace o jaký typ singleton je a jak vám může přinést použití, naleznete v tématu [použití typu singleton k definování zvláštní entita](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Vytvoření koncového bodu OData V4 v rozhraní Web API najdete v tématu [vytvořit OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md). 

Vytvoříme jednotlivý prvek v projektu webového rozhraní API pomocí modelu následující:

![Datový Model](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Jednotlivý prvek s názvem `Umbrella` se určí na základě typu `Company`a sadu pojmenovaných entit `Employees` se určí na základě typu `Employee`.

Řešení použité v tomto kurzu si můžete stáhnout z [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Definování datového modelu

1. Definování typů CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Generovat model EDM založený na typy CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Tady `builder.Singleton<Company>("Umbrella")` říká Tvůrce modelu vytvořit jednotlivý prvek s názvem `Umbrella` v modelu EDM.

    Vygenerovaný metadata bude vypadat nějak takto:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Z metadat můžeme vidět, který navigační vlastnost `Company` v `Employees` sady entit je vázán na jednotlivý prvek `Umbrella`. Vazba se provádí automaticky `ODataConventionModelBuilder`, protože pouze `Umbrella` má `Company` typu. Pokud je model veškerou nejednoznačnost, můžete použít `HasSingletonBinding` explicitně svázat vlastnost navigace typu singleton; `HasSingletonBinding` má stejný účinek jako použití `Singleton` atributu v definici typu CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Definování typu singleton kontroleru

Jako řadič EntitySet kontroleru typu singleton dědí z `ODataController`, a názvu kontroleru jednotlivý prvek by měl být `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Aby bylo možné zpracovávat různé druhy žádostí, akce musí být předem definované v kontroleru. **Atribut směrování** povolena ve výchozím nastavení WebApi 2.2. Například chcete-li definovat akci, kterou chcete zpracovat dotazování `Revenue` z `Company` používající směrování na atribut, použijte následující:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Pokud nepřijmete definují atributy pro každou akci, stačí definovat akce po [konvencí směrování protokolu OData](../odata-routing-conventions.md). Protože klíč není vyžadována pro dotazování typu singleton, se mírně liší z akcí, které jsou definované v kontroleru entityset akce definované v kontroleru typu singleton.

Pro srovnání podpisy metod pro každou definici akce v kontroleru typu singleton jsou uvedeny níže.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

V podstatě to je všechno, co musíte udělat na straně služby. [Ukázkový projekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) obsahuje kód pro řešení a klient OData, který ukazuje způsob použití typu singleton. Klient je sestavena podle kroků v [vytvoření klientské aplikace OData v4](create-an-odata-v4-client-app.md).

. 

*Děkujeme, že k leo by patřil velice Hu původní obsah tohoto článku.*
