---
title: Začínáme s rozhraním API ochrany dat v ASP.NET Core
author: rick-anderson
description: Naučte se používat ochranu dat ASP.NET Core rozhraní API pro ochranu a při rušení dat v aplikaci.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 3a69abd2b58e02f87ccaf2317b0a8a2a7e9d7b4a
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>Začínáme s rozhraním API ochrany dat v ASP.NET Core

<a name="security-data-protection-getting-started"></a>

Ve své nejjednodušší, ochranu dat se skládá z následujících kroků:

1. Vytvoření ochrany dat z zprostředkovatele ochrany data.

2. Volání `Protect` metoda s daty, které chcete chránit.

3. Volání `Unprotect` metoda s daty, které chcete zapnout zpět do prostého textu.

Většina architektury a modely aplikace, jako je například technologie ASP.NET nebo SignalR, už konfiguraci systému ochrany dat a přidat do kontejneru služby, ke kterému přistupujete pomocí vkládání závislostí. Následující příklad ukazuje konfiguraci služby kontejneru pro vkládání závislostí a registrace zásobníku ochrany dat, přijetí zprostředkovatel ochrany dat prostřednictvím DI, vytvoření ochranu a ochranu pak Probíhá rušení ochrany dat

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Když vytvoříte ochranného zařízení je nutné zadat jednu nebo více [účel řetězce](xref:security/data-protection/consumer-apis/purpose-strings). Řetězec účel zajišťuje izolaci mezi příjemci. Například ochranného zařízení vytvořené pomocí účel řetězec "green" nebude moct zrušení ochrany dat poskytované ochranného zařízení s účelem "Fialová".

>[!TIP]
> Instance `IDataProtectionProvider` a `IDataProtector` jsou bezpečné pro přístup z více vláken pro více volající. Rozhraní má který určeno po získá odkaz na komponentu `IDataProtector` prostřednictvím volání `CreateProtector`, tento odkaz se bude používat pro několik volání `Protect` a `Unprotect`.
>
>Volání `Unprotect` cryptographicexception – vyvolá výjimku, pokud nelze ověřit nebo dešifrovat znalosti chráněné datové části. Některé součásti chtít ignorování chyb během zrušení operace; komponenta, která čte ověřovací soubory cookie může zpracovat tuto chybu a považovat požadavku, jako kdyby měl žádný soubor cookie na všech než nesplní žádost přímý. Součásti, které chcete toto chování má konkrétně catch cryptographicexception – místo požití všechny výjimky.
