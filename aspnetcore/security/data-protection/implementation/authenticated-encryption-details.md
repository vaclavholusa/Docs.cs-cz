---
title: Podrobnosti o ověřené šifrování v ASP.NET Core
author: rick-anderson
description: Další podrobnosti o implementaci šifrování ochrany dat ASP.NET základní ověření.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 3ca5231e84156ede59793825e1a3e3bea0313055
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077904"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a><span data-ttu-id="30f13-103">Podrobnosti o ověřené šifrování v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30f13-103">Authenticated encryption details in ASP.NET Core</span></span>

<a name="data-protection-implementation-authenticated-encryption-details"></a>

<span data-ttu-id="30f13-104">Volání IDataProtector.Protect jsou operace ověřené šifrování.</span><span class="sxs-lookup"><span data-stu-id="30f13-104">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="30f13-105">Metoda chránit nabízí utajení a pravosti a je vázaný na řetězu účel, která byla použita pro tuto konkrétní instanci IDataProtector odvozovat svůj kořen IDataProtectionProvider.</span><span class="sxs-lookup"><span data-stu-id="30f13-105">The Protect method offers both confidentiality and authenticity, and it's tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="30f13-106">IDataProtector.Protect přebírá parametr ve formátu prostého textu byte [] a vytvoří byte [] chráněné datové části, jejichž formát je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="30f13-106">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="30f13-107">(Je zde také přetížení metody rozšíření, která přijímá řetězcový parametr ve formátu prostého textu a vrátí řetězec chráněné datové části.</span><span class="sxs-lookup"><span data-stu-id="30f13-107">(There's also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="30f13-108">Pokud se používá toto rozhraní API budou mít dál formát chráněné datové části níže struktura, ale bude [kódováním base64url](https://tools.ietf.org/html/rfc4648#section-5).)</span><span class="sxs-lookup"><span data-stu-id="30f13-108">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="30f13-109">Formát chráněných datové části</span><span class="sxs-lookup"><span data-stu-id="30f13-109">Protected payload format</span></span>

<span data-ttu-id="30f13-110">Formát chráněných datové části zahrnuje tři hlavní komponenty:</span><span class="sxs-lookup"><span data-stu-id="30f13-110">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="30f13-111">32-bit magic hlavičku, která identifikuje verzi systému ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="30f13-111">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="30f13-112">Id klíče 128-bit, který identifikuje klíč používaný k ochraně této konkrétní datové části.</span><span class="sxs-lookup"><span data-stu-id="30f13-112">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="30f13-113">Zbývající část chráněné datové části je [specifické pro modulu pro šifrování zapouzdřené pomocí tohoto klíče](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="30f13-113">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="30f13-114">V následujícím příkladu představuje klíč AES-256-CBC + HMACSHA256 modulu pro šifrování a datové části se dále dělí: \* modifikátor klíče A 128 bitů.</span><span class="sxs-lookup"><span data-stu-id="30f13-114">In the example below the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows: \* A 128-bit key modifier.</span></span> <span data-ttu-id="30f13-115">\* Na 128-bit inicializační vektor.</span><span class="sxs-lookup"><span data-stu-id="30f13-115">\* A 128-bit initialization vector.</span></span> <span data-ttu-id="30f13-116">\* 48 bajtů AES-256-CBC výstupu.</span><span class="sxs-lookup"><span data-stu-id="30f13-116">\* 48 bytes of AES-256-CBC output.</span></span> <span data-ttu-id="30f13-117">\* HMACSHA256 ověřování značku.</span><span class="sxs-lookup"><span data-stu-id="30f13-117">\* An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="30f13-118">Ukázka chráněné datové části je zobrazený dole.</span><span class="sxs-lookup"><span data-stu-id="30f13-118">A sample protected payload is illustrated below.</span></span>

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

<span data-ttu-id="30f13-119">Ze Formát datové části výše první 32bitová verze nebo 4 bajtů se hlavičku magic identifikace verze (09 F0 C9 F0)</span><span class="sxs-lookup"><span data-stu-id="30f13-119">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="30f13-120">Další 128 bitů nebo 16 bajtů je identifikátor klíče (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span><span class="sxs-lookup"><span data-stu-id="30f13-120">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="30f13-121">Zbývající obsahuje datové části a je specifická pro formát použitý.</span><span class="sxs-lookup"><span data-stu-id="30f13-121">The remainder contains the payload and is specific to the format used.</span></span>

>[!WARNING]
> <span data-ttu-id="30f13-122">Všechny datové části chráněné k danému klíči bude začínat stejné hlavičce 20bajtová (magic hodnotu, id klíče).</span><span class="sxs-lookup"><span data-stu-id="30f13-122">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="30f13-123">Správci můžou pomocí tuto skutečnost k diagnostickým účelům Přibližná při generování datové části.</span><span class="sxs-lookup"><span data-stu-id="30f13-123">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="30f13-124">Například datové části výše odpovídá klíč {0c819c80-6619-4019-9536-53f8aaffee57}.</span><span class="sxs-lookup"><span data-stu-id="30f13-124">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="30f13-125">Pokud po zkontrolování klíče úložiště zjistíte, že datum aktivace tento konkrétní klíč byl 2015-01-01 a datum vypršení platnosti se 2015-03-01, pak je možné logicky předpokládat, že datová část (Pokud není manipulováno) byl vygenerován v rámci dané okno udělení, nebo provést malá faktor fudge na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="30f13-125">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it's reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>
