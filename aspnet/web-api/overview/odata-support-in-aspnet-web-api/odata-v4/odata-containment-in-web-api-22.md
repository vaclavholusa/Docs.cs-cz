---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Zahrnutí v OData v4 použití rozhraní Web API 2.2 | Dokumentace Microsoftu
author: rick-anderson
description: Tradičně entita může přistupovat pouze pokud byly zapouzdřený v sadu entit. Ale OData v4 poskytuje dvě další možnosti, jednotlivý prvek a Con...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: aca263a04df25ca241bc0b9798b3a0b588d4cae8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752838"
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Zahrnutí v OData v4 použití rozhraní Web API 2.2
====================
podle Jinfu Tan

> Tradičně entita může přistupovat pouze pokud byly zapouzdřený v sadu entit. Ale OData v4 poskytuje dvě další možnosti, jednotlivý prvek a členství ve skupině, které podporuje WebAPI 2.2.


Toto téma ukazuje, jak definovat členství ve skupině v koncový bod OData v WebApi 2.2. Další informace o členství ve skupině najdete v tématu [členství ve skupině se chystá pro OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Vytvoření koncového bodu OData V4 v rozhraní Web API najdete v tématu [vytvořit OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).

Nejprve vytvoříme doménový model členství ve skupině služby OData, pomocí tohoto modelu dat:

![Datový model](odata-containment-in-web-api-22/_static/image1.png)

Účet obsahuje mnoho PaymentInstruments (PÍ), ale není definujeme sadu pro na PI entit. Místo toho instrukce pro zpracování je přístupný pouze prostřednictvím účtu.

Řešení použité v tomto tématu si můžete stáhnout [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Definování datového modelu

1. Definování typů CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained` Atribut se používá pro zahrnutí navigační vlastnosti.
2. Generovat model EDM založený na typy CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder` Bude zpracovávat sestavení modelu EDM, pokud `Contained` přidání atributu do odpovídající navigační vlastnost. Pokud je vlastnost typu kolekce `GetCount(string NameContains)` funkce se také vytvoří.

    Vygenerovaný metadata bude vypadat nějak takto:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget` Atribut označuje, že je vlastnost navigace členství ve skupině.

## <a name="define-the-containing-entity-set-controller"></a>Definování řadičem obsahující sadu entit

Omezením entity nemají své vlastní kontroler; akce je definované v kontroleru obsahující sadu entit. V této ukázce je AccountsController, ale žádné PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Pokud cestu OData je 4 nebo více segmentů, pouze atribut směrování funguje, jako například `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` ve výše uvedené kontroleru. V opačném případě funguje atribut a konvenční směrování: například `GetPayInPIs(int key)` odpovídá `GET ~/Accounts(1)/PayinPIs`.

*Děkujeme, že k leo by patřil velice Hu původní obsah tohoto článku.*
