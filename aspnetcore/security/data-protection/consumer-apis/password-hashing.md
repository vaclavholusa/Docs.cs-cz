---
title: "Hashování hesel"
author: rick-anderson
description: "Tento dokument vysvětluje, jak hodnoty hash hesla pomocí funkce Ochrana dat ASP.NET Core rozhraní API."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 8e2796108be14ef382f46e6deb3d584517120d27
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="password-hashing"></a>Hashování hesel

Kód ochrany dat, základní obsahuje balíček *Microsoft.AspNetCore.Cryptography.KeyDerivation* obsahující funkce odvození kryptografické klíče. Tento balíček je součástí samostatné a nemá žádné závislosti na výkon systému ochrany dat. Můžete použít zcela nezávisle. Zdroj existuje souběžně s kód ochrany dat, základní pro potřeby.

Balíček teď nabízí metody `KeyDerivation.Pbkdf2` což umožňuje použití algoritmu hash hesla pomocí [PBKDF2 algoritmus](https://tools.ietf.org/html/rfc2898#section-5.2). Toto rozhraní API je velmi podobné existující rozhraní .NET Framework [Rfc2898DeriveBytes typu](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), ale jsou tři důležité rozdíly:

1. `KeyDerivation.Pbkdf2` Metoda podporuje použití více PRFs (aktuálně `HMACSHA1`, `HMACSHA256`, a `HMACSHA512`), zatímco `Rfc2898DeriveBytes` zadejte podporuje jenom `HMACSHA1`.

2. `KeyDerivation.Pbkdf2` Metoda zjistí v aktuálním operačním systému a pokusí se zvolí nejvíce optimalizované implementaci rutiny, poskytuje mnohem lepší výkon v některých případech. (V systému Windows 8, nabízí přibližně 10 x propustnost `Rfc2898DeriveBytes`.)

3. `KeyDerivation.Pbkdf2` Metoda vyžaduje volajícímu zadat všechny parametry (salt, PRF a počet iterací). `Rfc2898DeriveBytes` Typ poskytuje výchozí hodnoty pro tyto.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Najdete ve zdrojovém kódu pro identitu ASP.NET Core `PasswordHasher` případ použití typ pro reálného prostředí.
