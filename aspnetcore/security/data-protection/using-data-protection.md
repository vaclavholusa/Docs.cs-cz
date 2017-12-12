---
title: "Začínáme s rozhraními API ochrany dat."
author: rick-anderson
description: "Tento dokument vysvětluje, jak používat ochranu dat ASP.NET Core rozhraní API pro ochranu a při rušení dat v aplikaci."
keywords: ASP.NET Core, ochrany dat
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 535bfaf2077cda91c27e7d0d68b9804e8596070e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a>Začínáme s rozhraními API ochrany dat.

<a name="security-data-protection-getting-started"></a>

Ve své nejjednodušší, ochranu dat se skládá z následujících kroků:

1. Vytvoření ochrany dat z zprostředkovatele ochrany data.

2. Volání `Protect` metoda s daty, které chcete chránit.

3. Volání `Unprotect` metoda s daty, které chcete zapnout zpět do prostého textu.

Většina architektury a modely aplikace, jako je například technologie ASP.NET nebo SignalR, už konfiguraci systému ochrany dat a přidat do kontejneru služby, ke kterému přistupujete pomocí vkládání závislostí. Následující příklad ukazuje konfiguraci služby kontejneru pro vkládání závislostí a registrace zásobníku ochrany dat, přijetí zprostředkovatel ochrany dat prostřednictvím DI, vytvoření ochranu a ochranu pak Probíhá rušení ochrany dat

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Když vytvoříte ochranného zařízení je nutné zadat jednu nebo více [účel řetězce](consumer-apis/purpose-strings.md). Řetězec účel zajišťuje izolaci mezi příjemci. Například ochranného zařízení vytvořené pomocí účel řetězec "green" nebude moct zrušení ochrany dat poskytované ochranného zařízení s účelem "Fialová".

>[!TIP]
> Instance `IDataProtectionProvider` a `IDataProtector` jsou bezpečné pro přístup z více vláken pro více volající. Je určena, jakmile získá odkaz na komponentu `IDataProtector` prostřednictvím volání `CreateProtector`, tento odkaz se bude používat pro několik volání `Protect` a `Unprotect`.
>
>Volání `Unprotect` cryptographicexception – vyvolá výjimku, pokud nelze ověřit nebo dešifrovat znalosti chráněné datové části. Některé součásti chtít ignorování chyb během zrušení operace; komponenta, která čte ověřovací soubory cookie může zpracovat tuto chybu a považovat požadavku, jako kdyby měl žádný soubor cookie na všech než nesplní žádost přímý. Součásti, které chcete toto chování má konkrétně catch cryptographicexception – místo požití všechny výjimky.
