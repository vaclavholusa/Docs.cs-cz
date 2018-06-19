---
title: Šifrování klíče v klidovém stavu uložených v ASP.NET Core
author: rick-anderson
description: Další podrobnosti implementace ochrany dat ASP.NET Core klíče šifrování v klidovém stavu.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: e5082d831dd4822fad0fb3211fe2b8c76ff967bf
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851115"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a>Šifrování klíče v klidovém stavu uložených v ASP.NET Core

<a name="data-protection-implementation-key-encryption-at-rest"></a>

Ve výchozím nastavení systému ochrany dat [využívá Heuristika](xref:security/data-protection/configuration/default-settings) k určení jak kryptografických materiál klíče by měla být v zašifrované podobě. Vývojář můžete přepsat heuristiky a ručně zadat, jak klíče by měla být v zašifrované podobě.

> [!NOTE]
> Pokud zadáte explicitní klíče šifrování v mechanismus rest, bude systém ochrany dat zrušení registrace výchozího mechanismu úložiště klíčů, které poskytuje heuristiky. Je nutné [zadejte mechanismus explicitní úložiště klíčů](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), jinak systému ochrany dat, nebude možné spustit.

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

Systém ochrany dat se dodává s tři mechanismy šifrovací klíče v poli.

## <a name="windows-dpapi"></a>Windows DPAPI

*Tento mechanismus je k dispozici pouze v systému Windows.*

Pokud se používá rozhraní Windows DPAPI, materiál klíče, bude se šifrovat prostřednictvím [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) před byly trvalé do úložiště. Rozhraní DPAPI je příslušným šifrovacím mechanismus pro data, která budou nikdy číst mimo aktuální počítač (přestože je možné zálohovat tyto klíče až do služby Active Directory, zjistit [DPAPI a cestovní profily](https://support.microsoft.com/kb/309408/#6)). Pro příklad konfigurace DPAPI šifrovací klíče na rest.

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

Pokud `ProtectKeysWithDpapi` je volána bez parametrů, pouze aktuální uživatelský účet systému Windows může dekódovat trvalou materiál klíče. Volitelně můžete zadat, že libovolný uživatelský účet v počítači (nikoli pouze aktuální uživatelský účet) měl být schopný dekódovat materiál klíče, jak je znázorněno následujícím příkladu.

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a>Certifikát X.509

*Tento mechanismus není k dispozici na `.NET Core 1.0` nebo `1.1`.*

Pokud vaše aplikace je rozdělena mezi více počítačů, může být vhodné pro distribuci sdíleného certifikátu X.509 mezi počítači a konfigurace aplikací k použití tohoto certifikátu pro šifrování klíče v klidovém stavu. Níže najdete příklad.

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

Z důvodu omezení rozhraní .NET Framework jsou podporovány pouze certifikáty s CAPI privátních klíčů. V tématu [šifrování založené na certifikátech s Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) níže možná řešení těchto omezení.

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

*Tento mechanismus je k dispozici pouze v systému Windows 8 nebo Windows Server 2012 a novější.*

Počínaje Windows 8, operační systém podporuje rozhraní DPAPI-NG (také nazývané CNG DPAPI). Microsoft rozložen jeho scénáři použití následujícím způsobem.

   Cloud computing ale často vyžaduje že být dešifrován na jiném tohoto obsahu šifrované na jednom počítači. Proto počínaje Windows 8, Microsoft rozšířené na nápad použití relativně jednoduché rozhraní API pro zahrnovat scénářích cloudu. Toto nové rozhraní API, volá rozhraní DPAPI-NG, umožňuje bezpečně sdílet tajné klíče (klíče, hesla, materiál klíče) a zprávy a chrání je na sadu objektů, které lze použít k odemknutí je na různých počítačích po správné ověření a autorizaci.

   Z [o CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)

Objekt je kódovaná jako popisovač pravidla ochrany. Vezměte v úvahu následujícím příkladu, který šifruje materiál klíče tak, aby pouze připojený k doméně uživatele se zadaným identifikátorem SID mohly dešifrovat materiál klíče.

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

Je také bez parametrů přetížení `ProtectKeysWithDpapiNG`. Toto je metoda užitečný pro zadání pravidlo "SID dolování =", kde dolování je SID aktuální uživatelský účet systému Windows.

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

V tomto scénáři je zodpovědný za distribuci šifrovací klíče, který používá rozhraní DPAPI NG operace řadič domény AD. Cílový uživatel bude moci dešifrovat šifrovaná data z libovolného počítače připojeného k doméně (za předpokladu, že je proces spuštěný v rámci své identity).

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Šifrování založené na certifikátech s Windows DPAPI-NG

Pokud používáte systém na Windows 8.1 nebo Windows Server 2012 R2 nebo novější, můžete použít Windows DPAPI-NG k provedení na základě certifikátu šifrování, i když aplikace běží na .NET Core. Abyste mohli využívat tohoto objektu, použijte řetězce popisovače pravidla "certifikát = HashId:thumbprint", kde je kryptografický otisk kódováním šestnáctkově SHA1 kryptografický otisk certifikátu pro použití. Níže najdete příklad.

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

Všechny aplikace, která je na tomto úložišti odkazoval musí být spuštěné na Windows 8.1 nebo Windows Server 2012 R2 nebo novější, abyste mohli dekódovat tento klíč.

## <a name="custom-key-encryption"></a>Vlastní šifrovací klíče

Pokud v poli mechanismy nejsou vhodné, vývojáře můžete určit vlastní mechanismus šifrování pomocí klíče pomocí vlastní `IXmlEncryptor`.
