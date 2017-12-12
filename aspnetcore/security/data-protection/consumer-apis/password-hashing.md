---
title: "Hashování hesel"
author: rick-anderson
description: "Tento dokument vysvětluje, jak hodnoty hash hesla pomocí funkce Ochrana dat ASP.NET Core rozhraní API."
keywords: ASP.NET Core, ochrany dat, hodnoty hash hesla
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 982a1eb2-1e6f-4909-896f-82784364472d
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: da9f505f58f18f7ab3b93753bae079eb976b3939
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="password-hashing"></a><span data-ttu-id="60cd0-104">Hashování hesel</span><span class="sxs-lookup"><span data-stu-id="60cd0-104">Password Hashing</span></span>

<span data-ttu-id="60cd0-105">Kód ochrany dat, základní obsahuje balíček *Microsoft.AspNetCore.Cryptography.KeyDerivation* obsahující funkce odvození kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="60cd0-105">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="60cd0-106">Tento balíček je součástí samostatné a nemá žádné závislosti na výkon systému ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="60cd0-106">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="60cd0-107">Můžete použít zcela nezávisle.</span><span class="sxs-lookup"><span data-stu-id="60cd0-107">It can be used completely independently.</span></span> <span data-ttu-id="60cd0-108">Zdroj existuje souběžně s kód ochrany dat, základní pro potřeby.</span><span class="sxs-lookup"><span data-stu-id="60cd0-108">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="60cd0-109">Balíček teď nabízí metody `KeyDerivation.Pbkdf2` což umožňuje použití algoritmu hash hesla pomocí [PBKDF2 algoritmus](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="60cd0-109">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="60cd0-110">Toto rozhraní API je velmi podobné existující rozhraní .NET Framework [Rfc2898DeriveBytes typu](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), ale jsou tři důležité rozdíly:</span><span class="sxs-lookup"><span data-stu-id="60cd0-110">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="60cd0-111">`KeyDerivation.Pbkdf2` Metoda podporuje použití více PRFs (aktuálně `HMACSHA1`, `HMACSHA256`, a `HMACSHA512`), zatímco `Rfc2898DeriveBytes` zadejte podporuje jenom `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="60cd0-111">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="60cd0-112">`KeyDerivation.Pbkdf2` Metoda zjistí v aktuálním operačním systému a pokusí se zvolí nejvíce optimalizované implementaci rutiny, poskytuje mnohem lepší výkon v některých případech.</span><span class="sxs-lookup"><span data-stu-id="60cd0-112">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="60cd0-113">(V systému Windows 8, nabízí přibližně 10 x propustnost `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="60cd0-113">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="60cd0-114">`KeyDerivation.Pbkdf2` Metoda vyžaduje volajícímu zadat všechny parametry (salt, PRF a počet iterací).</span><span class="sxs-lookup"><span data-stu-id="60cd0-114">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="60cd0-115">`Rfc2898DeriveBytes` Typ poskytuje výchozí hodnoty pro tyto.</span><span class="sxs-lookup"><span data-stu-id="60cd0-115">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="60cd0-116">Najdete ve zdrojovém kódu pro identitu ASP.NET Core `PasswordHasher` případ použití typ pro reálného prostředí.</span><span class="sxs-lookup"><span data-stu-id="60cd0-116">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
