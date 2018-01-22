---
title: "Hashování hesel"
author: rick-anderson
description: "Tento dokument vysvětluje, jak hodnoty hash hesla pomocí funkce Ochrana dat ASP.NET Core rozhraní API."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: e97d17b5f6de2e0ddcde6d51618675388b43911a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="password-hashing"></a><span data-ttu-id="4c406-103">Hashování hesel</span><span class="sxs-lookup"><span data-stu-id="4c406-103">Password Hashing</span></span>

<span data-ttu-id="4c406-104">Kód ochrany dat, základní obsahuje balíček *Microsoft.AspNetCore.Cryptography.KeyDerivation* obsahující funkce odvození kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="4c406-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="4c406-105">Tento balíček je součástí samostatné a nemá žádné závislosti na výkon systému ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="4c406-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="4c406-106">Můžete použít zcela nezávisle.</span><span class="sxs-lookup"><span data-stu-id="4c406-106">It can be used completely independently.</span></span> <span data-ttu-id="4c406-107">Zdroj existuje souběžně s kód ochrany dat, základní pro potřeby.</span><span class="sxs-lookup"><span data-stu-id="4c406-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="4c406-108">Balíček teď nabízí metody `KeyDerivation.Pbkdf2` což umožňuje použití algoritmu hash hesla pomocí [PBKDF2 algoritmus](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="4c406-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="4c406-109">Toto rozhraní API je velmi podobné existující rozhraní .NET Framework [Rfc2898DeriveBytes typu](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), ale jsou tři důležité rozdíly:</span><span class="sxs-lookup"><span data-stu-id="4c406-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="4c406-110">`KeyDerivation.Pbkdf2` Metoda podporuje použití více PRFs (aktuálně `HMACSHA1`, `HMACSHA256`, a `HMACSHA512`), zatímco `Rfc2898DeriveBytes` zadejte podporuje jenom `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="4c406-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="4c406-111">`KeyDerivation.Pbkdf2` Metoda zjistí v aktuálním operačním systému a pokusí se zvolí nejvíce optimalizované implementaci rutiny, poskytuje mnohem lepší výkon v některých případech.</span><span class="sxs-lookup"><span data-stu-id="4c406-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="4c406-112">(V systému Windows 8, nabízí přibližně 10 x propustnost `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="4c406-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="4c406-113">`KeyDerivation.Pbkdf2` Metoda vyžaduje volajícímu zadat všechny parametry (salt, PRF a počet iterací).</span><span class="sxs-lookup"><span data-stu-id="4c406-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="4c406-114">`Rfc2898DeriveBytes` Typ poskytuje výchozí hodnoty pro tyto.</span><span class="sxs-lookup"><span data-stu-id="4c406-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="4c406-115">Najdete ve zdrojovém kódu pro identitu ASP.NET Core `PasswordHasher` případ použití typ pro reálného prostředí.</span><span class="sxs-lookup"><span data-stu-id="4c406-115">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
