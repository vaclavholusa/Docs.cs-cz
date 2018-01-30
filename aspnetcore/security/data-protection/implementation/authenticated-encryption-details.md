---
title: "Podrobnosti o ověřené šifrování"
author: rick-anderson
description: "Tento dokument obsahuje přehled podrobnosti implementace ochrany dat ASP.NET Core ověřit šifrování."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: b58f36a5f0353da69d6f1ef4db542aba8267027a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="authenticated-encryption-details"></a>Podrobnosti o ověřené šifrování

<a name="data-protection-implementation-authenticated-encryption-details"></a>

Volání IDataProtector.Protect jsou operace ověřené šifrování. Metoda chránit nabízí utajení a pravosti a je vázaný na řetězu účel, která byla použita pro tuto konkrétní instanci IDataProtector odvozovat svůj kořen IDataProtectionProvider.

IDataProtector.Protect přebírá parametr ve formátu prostého textu byte [] a vytvoří byte [] chráněné datové části, jejichž formát je popsáno níže. (Je zde také přetížení metody rozšíření, která přijímá řetězcový parametr ve formátu prostého textu a vrátí řetězec chráněné datové části. Pokud se používá toto rozhraní API budou mít dál formát chráněné datové části níže struktura, ale bude [kódováním base64url](https://tools.ietf.org/html/rfc4648#section-5).)

## <a name="protected-payload-format"></a>Formát chráněných datové části

Formát chráněných datové části zahrnuje tři hlavní komponenty:

* 32-bit magic hlavičku, která identifikuje verzi systému ochrany dat.

* Id klíče 128-bit, který identifikuje klíč používaný k ochraně této konkrétní datové části.

* Zbývající část chráněné datové části je [specifické pro modulu pro šifrování zapouzdřené pomocí tohoto klíče](subkeyderivation.md#data-protection-implementation-subkey-derivation). V následujícím příkladu představuje klíč AES-256-CBC + HMACSHA256 modulu pro šifrování a datové části se dále dělí: * modifikátor klíče A 128 bitů. * Na 128-bit inicializační vektor. * 48 bajtů AES-256-CBC výstupu. * HMACSHA256 ověřování značku.

Ukázka chráněné datové části je zobrazený dole.

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

Ze Formát datové části výše první 32bitová verze nebo 4 bajtů se hlavičku magic identifikace verze (09 F0 C9 F0)

Další 128 bitů nebo 16 bajtů je identifikátor klíče (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)

Zbývající obsahuje datové části a je specifická pro formát použitý.

>[!WARNING]
> Všechny datové části chráněné k danému klíči bude začínat stejné hlavičce 20bajtová (magic hodnotu, id klíče). Správci můžou pomocí tuto skutečnost k diagnostickým účelům Přibližná při generování datové části. Například datové části výše odpovídá klíč {0c819c80-6619-4019-9536-53f8aaffee57}. Pokud po zkontrolování klíče úložiště zjistíte, že datum aktivace tento konkrétní klíč byl 2015-01-01 a datum vypršení platnosti se 2015-03-01, pak je možné logicky předpokládat, že datová část (Pokud není manipulováno) byl vygenerován v rámci dané okno udělení, nebo provést malá faktor fudge na obou stranách.
