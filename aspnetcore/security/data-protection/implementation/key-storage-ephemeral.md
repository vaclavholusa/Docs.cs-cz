---
title: Ochrana zprostředkovatele dočasných dat v ASP.NET Core
author: rick-anderson
description: Další podrobnosti implementace zprostředkovatelů ochrany dočasných dat ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9cdd3949826091807f3139f51aa0c05ddaac696a
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a><span data-ttu-id="8853d-103">Ochrana zprostředkovatele dočasných dat v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8853d-103">Ephemeral data protection providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="8853d-104">Existují scénáře, pokud aplikace potřebuje throwaway `IDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="8853d-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="8853d-105">Například vývojář může být právě experimentování v jednorázové konzolovou aplikaci, nebo vlastní aplikace je přechodný (je vytvořena nebo jednotku testování projektu).</span><span class="sxs-lookup"><span data-stu-id="8853d-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="8853d-106">Chcete-li tyto scénáře podporovat [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) balíček zahrnuje typ `EphemeralDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="8853d-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="8853d-107">Tento typ poskytuje základní implementaci `IDataProtectionProvider` jejichž klíče úložiště trvá výhradně v paměti a není zapsané do jakékoli úložiště zálohování.</span><span class="sxs-lookup"><span data-stu-id="8853d-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="8853d-108">Každá instance `EphemeralDataProtectionProvider` používá svůj vlastní jedinečný hlavní klíč.</span><span class="sxs-lookup"><span data-stu-id="8853d-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="8853d-109">Proto pokud `IDataProtector` root na `EphemeralDataProtectionProvider` generuje chráněné datové části této datové části může být pouze zbaveny ekvivalentní `IDataProtector` (zadaný stejný [účel](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) řetězu) root ve stejné `EphemeralDataProtectionProvider` instance.</span><span class="sxs-lookup"><span data-stu-id="8853d-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="8853d-110">Následující příklad ukazuje vytvoření instance `EphemeralDataProtectionProvider` a jeho použití k ochraně a zrušení data.</span><span class="sxs-lookup"><span data-stu-id="8853d-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
