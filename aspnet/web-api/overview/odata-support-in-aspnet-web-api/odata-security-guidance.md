---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Doprovodné materiály zabezpečení pro rozhraní ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 41b05f2a2f8247853d8358e6cc1246c8b438a6db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868705"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Doprovodné materiály zabezpečení pro rozhraní ASP.NET Web API 2 OData
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Toto téma popisuje některé problémy zabezpečení, které byste měli zvážit při vystavení datové sady přes OData.

## <a name="edm-security"></a>Zabezpečení EDM

Sémantiku dotazů jsou založené na entity data model (EDM), není základní typy modelu. Může být vyloučena určitá vlastnost z EDM a nebude viditelná pro dotaz. Předpokládejme například, že model obsahuje typ zaměstnance s mzda vlastností. Můžete chtít vyloučit tuto vlastnost z EDM skrytí od klientů.

Existují dva způsoby vyloučit vlastnost z modelu EDM. Můžete nastavit **[IgnoreDataMember]** atribut na vlastnost ve třídě modelu:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

Můžete také odebrat vlastnost z EDM prostřednictvím kódu programu:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Dotaz zabezpečení

Klient škodlivý nebo naïve můžou vytvořit dotaz, který trvá velmi dlouhou dobu spuštění. V nejhorším případě to může narušit poskytování přístupu k službě.

**[Queryable]** atribut je filtr akce, které analyzuje, ověří a použije dotaz. Filtr převede na výrazu LINQ možnosti dotazu. Při vrácení kontroler OData **IQueryable** typu, **IQueryable** LINQ zprostředkovatele převede výrazu LINQ do dotazu. Proto výkonu závisí na LINQ zprostředkovatele, který se používá a také na konkrétní vlastnosti schéma datové sady nebo databáze.

Další informace o použití možností dotazu OData v rozhraní ASP.NET Web API najdete v tématu [podporu možností dotazu OData](supporting-odata-query-options.md).

Pokud víte, že všichni klienti jsou důvěryhodné (například v podnikovém prostředí), nebo pokud vaše datová sada je malý, nemusí být výkon dotazů problém. Jinak měli byste zvážit následující doporučení.

- Testování vaší služby s různé dotazy a profil databáze.
- Povolte stránkování řízené serverem, aby se zabránilo vrácení velké sady dat v jednom dotazu. Další informace najdete v tématu [Server-Driven stránkování](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- Potřebujete $filter a $orderby? Některé aplikace může povolit klienta stránkování, pomocí $top a $skip, ale zakažte jiné možnosti dotazu. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Vezměte v úvahu omezení $orderby k vlastnosti v clusterovaný index. Řazení velkých objemů dat bez clusterovaného indexu je pomalý. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Počet uzlů maximální: **MaxNodeCount** vlastnost **[Queryable]** Nastaví maximální počet uzlů, které jsou povolené ve stromu syntaxe $filter. Výchozí hodnota je 100, ale můžete chtít nastavit nižší hodnotu, protože velký počet uzlů může být pomalé zkompilovat. To je zvlášť true, pokud používáte LINQ na objekty (tj, dotazů LINQ na kolekci v paměti, ale bez použití zprostředkující LINQ poskytovatele). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Zvažte zakázání funkce any() a all(), protože to může být pomalé. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Pokud žádné vlastnosti řetězce obsahovat velké řetězce & #8212for příklad, popis produktu nebo položku blogu & #8212consider zakázání funkce řetězec. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Vezměte v úvahu zakazuje filtrování na navigační vlastnosti. Filtrování na navigační vlastnosti může mít za následek spojení, což může být pomalé, v závislosti na svého schématu databáze. Následující kód ukazuje validátor dotazu, který brání filtrování na navigační vlastnosti. Další informace o dotazu validátory najdete v tématu [ověření dotazu](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Vezměte v úvahu omezení $filter dotazy napsáním validátor, které je přizpůsobené pro vaši databázi. Představte si třeba tyto dva dotazy: 

  - Všechny filmy se účastníky, jejichž příjmení začíná "A".
  - Všechny filmy vydané v roce 1994 za.

    Pokud jsou filmy indexovat pomocí aktéři, první dotaz může vyžadovat databázový stroj ke kontrole celý seznam filmy. Zatímco druhý dotaz zřejmě není přijatelné, předpokladu filmy jsou indexované podle verze roku.

    Následující kód ukazuje validátor, který umožňuje filtrování na vlastnosti "ReleaseYear" a "Title", ale žádné jiné vlastnosti.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- Obecně platí zvažte $filter funkcích, které potřebujete. Pokud vaši klienti nepotřebují úplné expressiveness $filter, můžete omezit mezi povolené funkce.
