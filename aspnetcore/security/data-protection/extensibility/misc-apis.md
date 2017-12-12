---
title: "Ostatní rozhraní API"
author: rick-anderson
description: "Tento dokument popisuje rozhraní ASP.NET Core ochrany dat ISecret."
keywords: ASP.NET Core, ochrany dat, ISecret
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: f5d6920f9f229bd480a76c952dab30efb7d9eff5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="1390e-104">Ostatní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1390e-104">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="1390e-105">Typy, které implementují některá z následujících rozhraní by měly být vláken pro více volající.</span><span class="sxs-lookup"><span data-stu-id="1390e-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="1390e-106">ISecret</span><span class="sxs-lookup"><span data-stu-id="1390e-106">ISecret</span></span>

<span data-ttu-id="1390e-107">`ISecret` Rozhraní představuje tajná hodnota, jako je například materiál kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="1390e-107">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="1390e-108">Obsahuje následující plochy rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="1390e-108">It contains the following API surface:</span></span>

* <span data-ttu-id="1390e-109">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="1390e-109">`Length`: `int`</span></span>

* <span data-ttu-id="1390e-110">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="1390e-110">`Dispose()`: `void`</span></span>

* <span data-ttu-id="1390e-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="1390e-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="1390e-112">`WriteSecretIntoBuffer` Metoda naplní poskytnutá vyrovnávací paměť s nezpracovaná tajná hodnota.</span><span class="sxs-lookup"><span data-stu-id="1390e-112">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="1390e-113">Z důvodu toto rozhraní API trvá vyrovnávací paměti jako parametr místo vrácení `byte[]` přímo je, že díky volající možnost připnout objekt vyrovnávací paměti, omezuje jeho vystavení tajný spravované uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="1390e-113">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="1390e-114">`Secret` Typ je konkrétní implementaci `ISecret` se uloží tajná hodnota v paměti v procesu.</span><span class="sxs-lookup"><span data-stu-id="1390e-114">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="1390e-115">Na platformách systému Windows, jsou zašifrovaná tajná hodnota prostřednictvím [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="1390e-115">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
