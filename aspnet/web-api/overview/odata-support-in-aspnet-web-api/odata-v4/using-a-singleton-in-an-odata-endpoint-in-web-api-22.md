---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: "Vytvoření jednotlivý prvek v OData v4 použití rozhraní Web API 2.2 | Microsoft Docs"
author: rick-anderson
description: "Toto téma ukazuje, jak definovat jednotlivý prvek v koncový bod OData v 2.2 webové rozhraní API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Vytvoření jednotlivý prvek v OData v4 použití rozhraní Web API 2.2
====================
podle Zoe Luo

> Obvyklým entita může používat pouze v případě měla zapouzdřený v sadu entit. Ale OData v4 poskytuje dvě další možnosti, jednotlivý prvek a členství ve skupině, které podporuje WebAPI 2.2.


Tento článek ukazuje, jak definovat jednotlivý prvek v koncový bod OData v 2.2 webové rozhraní API. Informace o jaký typ singleton je a jak můžete využívat výhod použití najdete v tématu [pomocí typu singleton, abyste definovali speciální entitě](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Vytvořit koncový bod OData V4 v rozhraní Web API, přečtěte si článek [vytvoření OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md). 

Vytvoříme jednotlivý prvek v projektu webového rozhraní API pomocí modelu následující data:

![Datový Model](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Jednotlivý prvek s názvem `Umbrella` bude určena podle typu `Company`a sadu s názvem entit `Employees` bude určena podle typu `Employee`.

Řešení používá v tomto kurzu lze stáhnout z [webu CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Definování datového modelu

1. Definování typů CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Generovat model EDM založený na typy CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Zde `builder.Singleton<Company>("Umbrella")` informuje Tvůrce modelu k vytvoření jednotlivý prvek s názvem `Umbrella` v modelu EDM.

    Vygenerovaný metadata bude vypadat takto:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Z metadat jsme uvidíte, že navigační vlastnost `Company` v `Employees` sady entit je vázán k singleton `Umbrella`. Vazba se provádí automaticky `ODataConventionModelBuilder`, vzhledem k tomu jenom `Umbrella` má `Company` typu. Pokud neexistuje žádné nejednoznačnosti v modelu, můžete použít `HasSingletonBinding` explicitně vytvořit vazbu na navigační vlastnost typu singleton; `HasSingletonBinding` má stejný účinek jako pomocí `Singleton` atribut v definici typu CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Zadejte řadič singleton

Jako řadič EntitySet, zdědí řadičem singleton `ODataController`, a měl by být názvu řadiče singleton `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Aby bylo možné zpracovávat různé druhy požadavky, akce musí být předem definované v kontroleru. **Atribut směrování** je povoleno ve výchozím nastavení v WebApi 2.2. Například můžete definovat akce pro zpracování dotazování `Revenue` z `Company` pomocí atributu směrování, použijte následující:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Pokud si nejste ochotni definovat atributy pro každou akci, pouze definuje vaše akce následující [konvencí směrování protokolu OData](../odata-routing-conventions.md). Vzhledem k tomu, že klíč se nevyžaduje pro dotazování typu singleton, akce definované v kontroleru typu singleton se mírně liší od akce definované v kontroleru entityset.

Pro srovnání podpisů metoda pro každou definici akce v kontroleru singleton jsou uvedeny níže.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

V podstatě to je všechno, co musíte udělat na straně služby. [Ukázkový projekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) obsahuje všechny kódu pro řešení a klient OData, který ukazuje způsob použití singleton. Klient je sestavena kroků v [vytvořit aplikaci OData v4 klienta](create-an-odata-v4-client-app.md).

. 

*Díky Leo Hu pro původní obsah tohoto článku.*
