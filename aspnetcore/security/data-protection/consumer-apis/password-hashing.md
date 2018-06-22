---
title: Hodnota hash hesla v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak hodnoty hash hesla pomocí rozhraní API ASP.NET Core Data Protection.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: aef22ab91e76afdb5f54dc37bcee7128420b6f3b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272984"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="695a6-103">Hodnota hash hesla v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="695a6-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="695a6-104">Kód ochrany dat, základní obsahuje balíček *Microsoft.AspNetCore.Cryptography.KeyDerivation* obsahující funkce odvození kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="695a6-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="695a6-105">Tento balíček je součástí samostatné a nemá žádné závislosti na výkon systému ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="695a6-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="695a6-106">Můžete použít zcela nezávisle.</span><span class="sxs-lookup"><span data-stu-id="695a6-106">It can be used completely independently.</span></span> <span data-ttu-id="695a6-107">Zdroj existuje souběžně s kód ochrany dat, základní pro potřeby.</span><span class="sxs-lookup"><span data-stu-id="695a6-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="695a6-108">Balíček teď nabízí metody `KeyDerivation.Pbkdf2` což umožňuje použití algoritmu hash hesla pomocí [PBKDF2 algoritmus](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="695a6-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="695a6-109">Toto rozhraní API je velmi podobné existující rozhraní .NET Framework [Rfc2898DeriveBytes typu](/dotnet/api/system.security.cryptography.rfc2898derivebytes), ale jsou tři důležité rozdíly:</span><span class="sxs-lookup"><span data-stu-id="695a6-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="695a6-110">`KeyDerivation.Pbkdf2` Metoda podporuje použití více PRFs (aktuálně `HMACSHA1`, `HMACSHA256`, a `HMACSHA512`), zatímco `Rfc2898DeriveBytes` zadejte podporuje jenom `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="695a6-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="695a6-111">`KeyDerivation.Pbkdf2` Metoda zjistí v aktuálním operačním systému a pokusí se zvolí nejvíce optimalizované implementaci rutiny, poskytuje mnohem lepší výkon v některých případech.</span><span class="sxs-lookup"><span data-stu-id="695a6-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="695a6-112">(V systému Windows 8, nabízí přibližně 10 x propustnost `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="695a6-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="695a6-113">`KeyDerivation.Pbkdf2` Metoda vyžaduje volajícímu zadat všechny parametry (salt, PRF a počet iterací).</span><span class="sxs-lookup"><span data-stu-id="695a6-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="695a6-114">`Rfc2898DeriveBytes` Typ poskytuje výchozí hodnoty pro tyto.</span><span class="sxs-lookup"><span data-stu-id="695a6-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="695a6-115">Najdete ve zdrojovém kódu pro identitu ASP.NET Core `PasswordHasher` případ použití typ pro reálného prostředí.</span><span class="sxs-lookup"><span data-stu-id="695a6-115">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
