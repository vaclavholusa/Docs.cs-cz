---
title: Rozhraní API ochrany dat různé ASP.NET Core
author: rick-anderson
description: Další informace o rozhraní ASP.NET Core Data Protection ISecret.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279152"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>Rozhraní API ochrany dat různé ASP.NET Core

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
