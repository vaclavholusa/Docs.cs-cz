---
title: Ochrana zprostředkovatele dočasných dat v ASP.NET Core
author: rick-anderson
description: Další podrobnosti implementace zprostředkovatelů ochrany dočasných dat ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279464"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>Ochrana zprostředkovatele dočasných dat v ASP.NET Core

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Existují scénáře, pokud aplikace potřebuje throwaway `IDataProtectionProvider`. Například vývojář může být právě experimentování v jednorázové konzolovou aplikaci, nebo vlastní aplikace je přechodný (je vytvořena nebo jednotku testování projektu). Chcete-li tyto scénáře podporovat [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) balíček zahrnuje typ `EphemeralDataProtectionProvider`. Tento typ poskytuje základní implementaci `IDataProtectionProvider` jejichž klíče úložiště trvá výhradně v paměti a není zapsané do jakékoli úložiště zálohování.

Každá instance `EphemeralDataProtectionProvider` používá svůj vlastní jedinečný hlavní klíč. Proto pokud `IDataProtector` root na `EphemeralDataProtectionProvider` generuje chráněné datové části této datové části může být pouze zbaveny ekvivalentní `IDataProtector` (zadaný stejný [účel](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) řetězu) root ve stejné `EphemeralDataProtectionProvider` instance.

Následující příklad ukazuje vytvoření instance `EphemeralDataProtectionProvider` a jeho použití k ochraně a zrušení data.

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
