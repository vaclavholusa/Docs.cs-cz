---
title: "Ostatní rozhraní API"
author: rick-anderson
description: "Tento dokument popisuje rozhraní ASP.NET Core ochrany dat ISecret."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 88a08a25abf4c4e1ba0746087b05b1cc8fa13024
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="eafe4-103">Ostatní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="eafe4-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="eafe4-104">Typy, které implementují některá z následujících rozhraní by měly být vláken pro více volající.</span><span class="sxs-lookup"><span data-stu-id="eafe4-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="eafe4-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="eafe4-105">ISecret</span></span>

<span data-ttu-id="eafe4-106">`ISecret` Rozhraní představuje tajná hodnota, jako je například materiál kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="eafe4-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="eafe4-107">Obsahuje následující plochy rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="eafe4-107">It contains the following API surface:</span></span>

* <span data-ttu-id="eafe4-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="eafe4-108">`Length`: `int`</span></span>

* <span data-ttu-id="eafe4-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="eafe4-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="eafe4-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="eafe4-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="eafe4-111">`WriteSecretIntoBuffer` Metoda naplní poskytnutá vyrovnávací paměť s nezpracovaná tajná hodnota.</span><span class="sxs-lookup"><span data-stu-id="eafe4-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="eafe4-112">Z důvodu toto rozhraní API trvá vyrovnávací paměti jako parametr místo vrácení `byte[]` přímo je, že díky volající možnost připnout objekt vyrovnávací paměti, omezuje jeho vystavení tajný spravované uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="eafe4-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="eafe4-113">`Secret` Typ je konkrétní implementaci `ISecret` se uloží tajná hodnota v paměti v procesu.</span><span class="sxs-lookup"><span data-stu-id="eafe4-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="eafe4-114">Na platformách systému Windows, jsou zašifrovaná tajná hodnota prostřednictvím [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="eafe4-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
