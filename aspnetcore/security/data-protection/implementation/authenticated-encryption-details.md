---
title: "Podrobnosti o ověřené šifrování"
author: rick-anderson
description: "Tento dokument obsahuje přehled podrobnosti implementace ochrany dat ASP.NET Core ověřit šifrování."
keywords: "ASP.NET Core, ochrany dat, ověřený šifrování"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 826e6d5d-9620-44e6-ad93-3b1d9969b70b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: dc96412f6578e612a39e86ce00e1dc5a20cf84e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="authenticated-encryption-details"></a><span data-ttu-id="72e92-104">Podrobnosti o ověřené šifrování</span><span class="sxs-lookup"><span data-stu-id="72e92-104">Authenticated encryption details</span></span>

<a name="data-protection-implementation-authenticated-encryption-details"></a>

<span data-ttu-id="72e92-105">Volání IDataProtector.Protect jsou operace ověřené šifrování.</span><span class="sxs-lookup"><span data-stu-id="72e92-105">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="72e92-106">Metoda chránit nabízí utajení a pravosti a je vázaný na řetězu účel, která byla použita pro tuto konkrétní instanci IDataProtector odvozovat svůj kořen IDataProtectionProvider.</span><span class="sxs-lookup"><span data-stu-id="72e92-106">The Protect method offers both confidentiality and authenticity, and it is tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="72e92-107">IDataProtector.Protect přebírá parametr ve formátu prostého textu byte [] a vytvoří byte [] chráněné datové části, jejichž formát je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="72e92-107">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="72e92-108">(Je zde také přetížení metody rozšíření, která přijímá řetězcový parametr ve formátu prostého textu a vrátí řetězec chráněné datové části.</span><span class="sxs-lookup"><span data-stu-id="72e92-108">(There is also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="72e92-109">Pokud se používá toto rozhraní API budou mít dál formát chráněné datové části níže struktura, ale bude [kódováním base64url](https://tools.ietf.org/html/rfc4648#section-5).)</span><span class="sxs-lookup"><span data-stu-id="72e92-109">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="72e92-110">Formát chráněných datové části</span><span class="sxs-lookup"><span data-stu-id="72e92-110">Protected payload format</span></span>

<span data-ttu-id="72e92-111">Formát chráněných datové části zahrnuje tři hlavní komponenty:</span><span class="sxs-lookup"><span data-stu-id="72e92-111">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="72e92-112">32-bit magic hlavičku, která identifikuje verzi systému ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="72e92-112">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="72e92-113">Id klíče 128-bit, který identifikuje klíč používaný k ochraně této konkrétní datové části.</span><span class="sxs-lookup"><span data-stu-id="72e92-113">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="72e92-114">Zbývající část chráněné datové části je [specifické pro modulu pro šifrování zapouzdřené pomocí tohoto klíče](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="72e92-114">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="72e92-115">V následujícím příkladu představuje klíč AES-256-CBC + HMACSHA256 modulu pro šifrování a datové části se dále dělí: * modifikátor klíče A 128 bitů.</span><span class="sxs-lookup"><span data-stu-id="72e92-115">In the example below the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows: * A 128-bit key modifier.</span></span> <span data-ttu-id="72e92-116">* Na 128-bit inicializační vektor.</span><span class="sxs-lookup"><span data-stu-id="72e92-116">* A 128-bit initialization vector.</span></span> <span data-ttu-id="72e92-117">* 48 bajtů AES-256-CBC výstupu.</span><span class="sxs-lookup"><span data-stu-id="72e92-117">* 48 bytes of AES-256-CBC output.</span></span> <span data-ttu-id="72e92-118">* HMACSHA256 ověřování značku.</span><span class="sxs-lookup"><span data-stu-id="72e92-118">* An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="72e92-119">Ukázka chráněné datové části je zobrazený dole.</span><span class="sxs-lookup"><span data-stu-id="72e92-119">A sample protected payload is illustrated below.</span></span>

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

<span data-ttu-id="72e92-120">Ze Formát datové části výše první 32bitová verze nebo 4 bajtů se hlavičku magic identifikace verze (09 F0 C9 F0)</span><span class="sxs-lookup"><span data-stu-id="72e92-120">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="72e92-121">Další 128 bitů nebo 16 bajtů je identifikátor klíče (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span><span class="sxs-lookup"><span data-stu-id="72e92-121">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="72e92-122">Zbývající obsahuje datové části a je specifická pro formát použitý.</span><span class="sxs-lookup"><span data-stu-id="72e92-122">The remainder contains the payload and is specific to the format used.</span></span>

>[!WARNING]
> <span data-ttu-id="72e92-123">Všechny datové části chráněné k danému klíči bude začínat stejné hlavičce 20bajtová (magic hodnotu, id klíče).</span><span class="sxs-lookup"><span data-stu-id="72e92-123">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="72e92-124">Správci můžou pomocí tuto skutečnost k diagnostickým účelům Přibližná při generování datové části.</span><span class="sxs-lookup"><span data-stu-id="72e92-124">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="72e92-125">Například datové části výše odpovídá klíč {0c819c80-6619-4019-9536-53f8aaffee57}.</span><span class="sxs-lookup"><span data-stu-id="72e92-125">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="72e92-126">Pokud po zkontrolování klíče úložiště zjistíte, že datum aktivace tento konkrétní klíč byl 2015-01-01 a datum vypršení platnosti se 2015-03-01, pak je možné logicky předpokládat, že datová část (Pokud není manipulováno) byl vygenerován v rámci dané okno udělení, nebo provést malá faktor fudge na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="72e92-126">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it is reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>
