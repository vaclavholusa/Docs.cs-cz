---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: "Omezení v OData v4 použití rozhraní Web API 2.2 | Microsoft Docs"
author: rick-anderson
description: "Obvyklým entita může používat pouze v případě měla zapouzdřený v sadu entit. Ale OData v4 poskytuje dvě další možnosti, jednotlivý prvek a Con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Omezení v OData v4 použití rozhraní Web API 2.2
====================
podle Jinfu Tan

> Obvyklým entita může používat pouze v případě měla zapouzdřený v sadu entit. Ale OData v4 poskytuje dvě další možnosti, jednotlivý prvek a členství ve skupině, které podporuje WebAPI 2.2.


Toto téma ukazuje, jak definovat omezení v koncový bod OData v WebApi 2.2. Další informace o členství ve skupině najdete v tématu [členství ve skupině pochází s OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Vytvořit koncový bod OData V4 v rozhraní Web API, přečtěte si článek [vytvoření OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).

Nejdříve vytvoříme modelu omezení domény ve službě OData pomocí tento datový model:

![Datový model](odata-containment-in-web-api-22/_static/image1.png)

Účet obsahuje mnoho PaymentInstruments (PI), ale nemáte definujeme entitu pí. Místo toho můžete instrukce pro zpracování přistupovat pouze prostřednictvím účtu.

Si můžete stáhnout řešení použitým v tomto tématu z [webu CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Definování datový model

1. Definování typů CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained` Atribut se používá pro zahrnutí navigační vlastnosti.
2. Generovat model EDM založený na typy CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder` Zpracuje vytváření EDM model, pokud `Contained` přidání atributu do odpovídající navigační vlastnost. Pokud je vlastnost typu kolekce `GetCount(string NameContains)` bude vytvořen také funkce.

    Vygenerovaný metadata bude vypadat takto:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget` Atribut označuje, že je vlastnost navigace členství ve skupině.

## <a name="define-the-containing-entity-set-controller"></a>Definování řadičem obsahující sadu entit

Obsažené entity nemají své vlastní řadiče; akce je definované v kontroleru obsahující sadu entit. V této ukázce je AccountsController, ale žádné PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Pokud cestu OData 4 nebo více segmenty, pouze atribut směrování funguje, jako například `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` ve výše uvedené kontroleru. Jinak hodnota atributu a standardní metody směrování funguje: například `GetPayInPIs(int key)` odpovídá `GET ~/Accounts(1)/PayinPIs`.

*Díky Leo Hu pro původní obsah tohoto článku.*
