---
title: "Ostatní rozhraní API"
author: rick-anderson
description: "Tento dokument popisuje rozhraní ASP.NET Core ochrany dat ISecret."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 26474de3a1c681a8c7f8b7812e9ef859f5d448bb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="miscellaneous-apis"></a>Ostatní rozhraní API

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Typy, které implementují některá z následujících rozhraní by měly být vláken pro více volající.

## <a name="isecret"></a>ISecret

`ISecret` Rozhraní představuje tajná hodnota, jako je například materiál kryptografické klíče. Obsahuje následující plochy rozhraní API:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer` Metoda naplní poskytnutá vyrovnávací paměť s nezpracovaná tajná hodnota. Z důvodu toto rozhraní API trvá vyrovnávací paměti jako parametr místo vrácení `byte[]` přímo je, že díky volající možnost připnout objekt vyrovnávací paměti, omezuje jeho vystavení tajný spravované uvolňování paměti.

`Secret` Typ je konkrétní implementaci `ISecret` se uloží tajná hodnota v paměti v procesu. Na platformách systému Windows, jsou zašifrovaná tajná hodnota prostřednictvím [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
