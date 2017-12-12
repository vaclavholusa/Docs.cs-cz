---
title: "Přehled rozhraní API příjemce"
author: rick-anderson
description: "Tento dokument obsahuje stručný přehled různých příjemce rozhraní API, které jsou k dispozici v knihovně ochrany dat ASP.NET Core."
keywords: "ASP.NET Core, ochrany dat příjemce rozhraní API"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f69beb9d-a519-43a8-857c-f6b01886a903
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: c80ed22776b0dbbaccb686f8c2ea534e6f5d9a74
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="consumer-apis-overview"></a>Přehled rozhraní API příjemce

`IDataProtectionProvider` a `IDataProtector` základní rozhraní, pomocí kterých příjemci použít systém ochrany dat jsou rozhraní. Soubory jsou uložené [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) balíčku.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

Rozhraní poskytovatele reprezentuje kořen systému ochrany dat. Ji nelze použít přímo k ochraně nebo zrušení ochrany dat. Místo toho musíte příjemce získat odkaz na `IDataProtector` voláním `IDataProtectionProvider.CreateProtector(purpose)`, kde účel je řetězec, který popisuje případ použití zamýšlený příjemce. V tématu [účel řetězce](purpose-strings.md) mnohem Další informace o záměr tento parametr a jak vybrat odpovídající hodnotu.

## <a name="idataprotector"></a>IDataProtector

Vrátí rozhraní Ochrana volání `CreateProtector`, a je toto rozhraní, které příjemci můžete využít k ochraně a zrušení operace.

K ochraně část dat, předat data, která mají `Protect` metoda. Základní rozhraní definuje metodu, která převede byte [] -> byte [], ale k dispozici je také přetížení (poskytuje se jako metody rozšíření), který převádí řetězec -> řetězec. Zabezpečení, které nabízí dvě metody je stejný jako; vývojáři měli vybrat kteroukoli přetížení je nejvhodnější pro případ jejich použití. Bez ohledu na přetížení, jste vybrali, hodnoty vrácené Chraňte je metoda teď chráněné (enciphered a zabezpečeny zfalšování) a aplikace můžete ho odeslat do nedůvěryhodného klienta.

Chcete-li zrušit ochranu dřív chránila část dat, předejte chráněných dat na `Unprotect` metoda. (Existují byte [] – na základě a na základě řetězec přetížení důvodu usnadnění práce vývojářů.) Pokud chráněné datové části vygenerovalo starší volání `Protect` na tento stejný `IDataProtector`, `Unprotect` metoda vrátí původní nechráněný datové části. Pokud chráněný datovou ní bylo neoprávněně manipulováno, nebo bylo vytvořeno pomocí jiné `IDataProtector`, `Unprotect` vyvolá metoda výjimku cryptographicexception –.

Koncept stejné vs. jiné `IDataProtector` ties zpět na koncept účel. Pokud dva `IDataProtector` instancí, které byly generovány od stejnou kořenovou `IDataProtectionProvider` ale prostřednictvím různých účel řetězce ve volání `IDataProtectionProvider.CreateProtector`, pak jsou považovány za [různých ochrany](purpose-strings.md), a jeden nebude možné zrušit ochranu generuje jinými datové části.

## <a name="consuming-these-interfaces"></a>Využívání těchto rozhraní

Pro komponentu podporou DI zamýšlené použití je, že součást trvat `IDataProtectionProvider` parametr v jeho konstruktoru a že DI systému automaticky poskytuje tuto službu, při vytváření instance komponentu.

> [!NOTE]
> Některé aplikace (například konzolové aplikace nebo aplikace ASP.NET 4.x) nemusí být DI deklaracemi, nemůžete použít tento mechanismus popsané tady. Pro tyto scénáře naleznete [jiné scénáře využívající DI](../configuration/non-di-scenarios.md) dokumentu pro další informace o získávání instanci `IDataProtection` zprostředkovatele bez průchodu přes DI.

Následující příklad ukazuje tři koncepty:

1. [Přidání systému ochrany dat](../configuration/overview.md) ke kontejneru služby

2. Pomocí DI přijímat instanci `IDataProtectionProvider`, a

3. Vytvoření `IDataProtector` z `IDataProtectionProvider` a jeho použití k ochraně a zrušení data.

[!code-csharp[Main](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Balíček Microsoft.AspNetCore.DataProtection.Abstractions obsahuje metody rozšíření `IServiceProvider.GetDataProtector` v zájmu usnadnění práce vývojářů. Zapouzdřit jako najednou i načítání `IDataProtectionProvider` z poskytovatele služeb a volání `IDataProtectionProvider.CreateProtector`. Následující příklad ukazuje jeho použití.

[!code-csharp[Main](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Instance `IDataProtectionProvider` a `IDataProtector` jsou bezpečné pro přístup z více vláken pro více volající. Je určena, jakmile získá odkaz na komponentu `IDataProtector` prostřednictvím volání `CreateProtector`, tento odkaz se bude používat pro několik volání `Protect` a `Unprotect`. Volání `Unprotect` cryptographicexception – vyvolá výjimku, pokud nelze ověřit nebo dešifrovat znalosti chráněné datové části. Některé součásti chtít ignorování chyb během zrušení operace; komponenta, která čte ověřovací soubory cookie může zpracovat tuto chybu a považovat požadavku, jako kdyby měl žádný soubor cookie na všech než nesplní žádost přímý. Součásti, které chcete toto chování má konkrétně catch cryptographicexception – místo požití všechny výjimky.
