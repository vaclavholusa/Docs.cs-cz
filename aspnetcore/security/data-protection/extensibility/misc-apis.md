---
title: Rozhraní API ochrany dat různé ASP.NET Core
author: rick-anderson
description: Další informace o rozhraní ASP.NET Core Data Protection ISecret.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 484c6a0979a10e7cf2b801873655caa99a42532c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072761"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="0be06-103">Rozhraní API ochrany dat různé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0be06-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="0be06-104">Typy, které implementují některá z následujících rozhraní by měly být vláken pro více volající.</span><span class="sxs-lookup"><span data-stu-id="0be06-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="0be06-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="0be06-105">ISecret</span></span>

<span data-ttu-id="0be06-106">`ISecret` Rozhraní představuje tajná hodnota, jako je například materiál kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="0be06-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="0be06-107">Obsahuje následující plochy rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="0be06-107">It contains the following API surface:</span></span>

* <span data-ttu-id="0be06-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="0be06-108">`Length`: `int`</span></span>

* <span data-ttu-id="0be06-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="0be06-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="0be06-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="0be06-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="0be06-111">`WriteSecretIntoBuffer` Metoda naplní poskytnutá vyrovnávací paměť s nezpracovaná tajná hodnota.</span><span class="sxs-lookup"><span data-stu-id="0be06-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="0be06-112">Z důvodu toto rozhraní API trvá vyrovnávací paměti jako parametr místo vrácení `byte[]` přímo je, že díky volající možnost připnout objekt vyrovnávací paměti, omezuje jeho vystavení tajný spravované uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="0be06-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="0be06-113">`Secret` Typ je konkrétní implementaci `ISecret` se uloží tajná hodnota v paměti v procesu.</span><span class="sxs-lookup"><span data-stu-id="0be06-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="0be06-114">Na platformách systému Windows, jsou zašifrovaná tajná hodnota prostřednictvím [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="0be06-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
