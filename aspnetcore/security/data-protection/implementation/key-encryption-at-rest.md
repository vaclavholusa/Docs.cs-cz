---
title: "Klíče šifrování v klidovém stavu"
author: rick-anderson
description: "Tento dokument popisuje podrobnosti implementace ASP.NET Core data protection klíče šifrování v klidovém stavu."
keywords: "ASP.NET Core, ochrany dat, šifrování klíče"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f2bbbf4e-0945-43ce-be59-8bf19e448798
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: b56dc56ed94662dbedeea49022aa73941bc833c5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="key-encryption-at-rest"></a><span data-ttu-id="ff088-104">Klíče šifrování v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="ff088-104">Key Encryption At Rest</span></span>

<a name="data-protection-implementation-key-encryption-at-rest"></a>

<span data-ttu-id="ff088-105">Ve výchozím nastavení systému ochrany dat [využívá Heuristika](xref:security/data-protection/configuration/default-settings) k určení jak kryptografických materiál klíče by měla být v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="ff088-105">By default, the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine how cryptographic key material should be encrypted at rest.</span></span> <span data-ttu-id="ff088-106">Vývojář můžete přepsat heuristiky a ručně zadat, jak klíče by měla být v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="ff088-106">The developer can override the heuristic and manually specify how keys should be encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="ff088-107">Pokud zadáte explicitní klíče šifrování v mechanismus rest, bude systém ochrany dat zrušení registrace výchozího mechanismu úložiště klíčů, které poskytuje heuristiky.</span><span class="sxs-lookup"><span data-stu-id="ff088-107">If you specify an explicit key encryption at rest mechanism, the data protection system will deregister the default key storage mechanism that the heuristic provided.</span></span> <span data-ttu-id="ff088-108">Je nutné [zadejte mechanismus explicitní úložiště klíčů](key-storage-providers.md#data-protection-implementation-key-storage-providers), jinak systému ochrany dat, nebude možné spustit.</span><span class="sxs-lookup"><span data-stu-id="ff088-108">You must [specify an explicit key storage mechanism](key-storage-providers.md#data-protection-implementation-key-storage-providers), otherwise the data protection system will fail to start.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

<span data-ttu-id="ff088-109">Systém ochrany dat se dodává s tři mechanismy šifrovací klíče v poli.</span><span class="sxs-lookup"><span data-stu-id="ff088-109">The data protection system ships with three in-box key encryption mechanisms.</span></span>

## <a name="windows-dpapi"></a><span data-ttu-id="ff088-110">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="ff088-110">Windows DPAPI</span></span>

<span data-ttu-id="ff088-111">*Tento mechanismus je k dispozici pouze v systému Windows.*</span><span class="sxs-lookup"><span data-stu-id="ff088-111">*This mechanism is available only on Windows.*</span></span>

<span data-ttu-id="ff088-112">Pokud se používá rozhraní Windows DPAPI, materiál klíče, bude se šifrovat prostřednictvím [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) před byly trvalé do úložiště.</span><span class="sxs-lookup"><span data-stu-id="ff088-112">When Windows DPAPI is used, key material will be encrypted via [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) before being persisted to storage.</span></span> <span data-ttu-id="ff088-113">Rozhraní DPAPI je příslušným šifrovacím mechanismus pro data, která budou nikdy číst mimo aktuální počítač (přestože je možné zálohovat tyto klíče až do služby Active Directory, zjistit [DPAPI a cestovní profily](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="ff088-113">DPAPI is an appropriate encryption mechanism for data that will never be read outside of the current machine (though it is possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="ff088-114">Pro příklad konfigurace DPAPI šifrovací klíče na rest.</span><span class="sxs-lookup"><span data-stu-id="ff088-114">For example to configure DPAPI key-at-rest encryption.</span></span>

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

<span data-ttu-id="ff088-115">Pokud `ProtectKeysWithDpapi` je volána bez parametrů, pouze aktuální uživatelský účet systému Windows může dekódovat trvalou materiál klíče.</span><span class="sxs-lookup"><span data-stu-id="ff088-115">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key material.</span></span> <span data-ttu-id="ff088-116">Volitelně můžete zadat, že libovolný uživatelský účet v počítači (nikoli pouze aktuální uživatelský účet) měl být schopný dekódovat materiál klíče, jak je znázorněno následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="ff088-116">You can optionally specify that any user account on the machine (not just the current user account) should be able to decipher the key material, as shown in the below example.</span></span>

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a><span data-ttu-id="ff088-117">Certifikát X.509</span><span class="sxs-lookup"><span data-stu-id="ff088-117">X.509 certificate</span></span>

<span data-ttu-id="ff088-118">*Tento mechanismus není k dispozici na `.NET Core 1.0` nebo `1.1`.*</span><span class="sxs-lookup"><span data-stu-id="ff088-118">*This mechanism is not available on `.NET Core 1.0` or `1.1`.*</span></span>

<span data-ttu-id="ff088-119">Pokud vaše aplikace je rozdělena mezi více počítačů, může být vhodné pro distribuci sdíleného certifikátu X.509 mezi počítači a konfigurace aplikací k použití tohoto certifikátu pro šifrování klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="ff088-119">If your application is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and to configure applications to use this certificate for encryption of keys at rest.</span></span> <span data-ttu-id="ff088-120">Níže najdete příklad.</span><span class="sxs-lookup"><span data-stu-id="ff088-120">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

<span data-ttu-id="ff088-121">Z důvodu omezení rozhraní .NET Framework jsou podporovány pouze certifikáty s CAPI privátních klíčů.</span><span class="sxs-lookup"><span data-stu-id="ff088-121">Due to .NET Framework limitations only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="ff088-122">V tématu [šifrování založené na certifikátech s Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) níže možná řešení těchto omezení.</span><span class="sxs-lookup"><span data-stu-id="ff088-122">See [Certificate-based encryption with Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) below for possible workarounds to these limitations.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a><span data-ttu-id="ff088-123">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="ff088-123">Windows DPAPI-NG</span></span>

<span data-ttu-id="ff088-124">*Tento mechanismus je k dispozici pouze v systému Windows 8 nebo Windows Server 2012 a novější.*</span><span class="sxs-lookup"><span data-stu-id="ff088-124">*This mechanism is available only on Windows 8 / Windows Server 2012 and later.*</span></span>

<span data-ttu-id="ff088-125">Počínaje Windows 8, operační systém podporuje rozhraní DPAPI-NG (také nazývané CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="ff088-125">Beginning with Windows 8, the operating system supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="ff088-126">Microsoft rozložen jeho scénáři použití následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="ff088-126">Microsoft lays out its usage scenario as follows.</span></span>

   <span data-ttu-id="ff088-127">Cloud computing ale často vyžaduje že být dešifrován na jiném tohoto obsahu šifrované na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="ff088-127">Cloud computing, however, often requires that content encrypted on one computer be decrypted on another.</span></span> <span data-ttu-id="ff088-128">Proto počínaje Windows 8, Microsoft rozšířené na nápad použití relativně jednoduché rozhraní API pro zahrnovat scénářích cloudu.</span><span class="sxs-lookup"><span data-stu-id="ff088-128">Therefore, beginning with Windows 8, Microsoft extended the idea of using a relatively straightforward API to encompass cloud scenarios.</span></span> <span data-ttu-id="ff088-129">Toto nové rozhraní API, volá rozhraní DPAPI-NG, umožňuje bezpečně sdílet tajné klíče (klíče, hesla, materiál klíče) a zprávy a chrání je na sadu objektů, které lze použít k odemknutí je na různých počítačích po správné ověření a autorizaci.</span><span class="sxs-lookup"><span data-stu-id="ff088-129">This new API, called DPAPI-NG, enables you to securely share secrets (keys, passwords, key material) and messages by protecting them to a set of principals that can be used to unprotect them on different computers after proper authentication and authorization.</span></span>

   <span data-ttu-id="ff088-130">Z [o CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="ff088-130">From [About CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span></span>

<span data-ttu-id="ff088-131">Objekt je kódovaná jako popisovač pravidla ochrany.</span><span class="sxs-lookup"><span data-stu-id="ff088-131">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="ff088-132">Vezměte v úvahu následujícím příkladu, který šifruje materiál klíče tak, aby pouze připojený k doméně uživatele se zadaným identifikátorem SID mohly dešifrovat materiál klíče.</span><span class="sxs-lookup"><span data-stu-id="ff088-132">Consider the below example, which encrypts key material such that only the domain-joined user with the specified SID can decrypt the key material.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="ff088-133">Je také bez parametrů přetížení `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="ff088-133">There is also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="ff088-134">Toto je metoda užitečný pro zadání pravidlo "SID dolování =", kde dolování je SID aktuální uživatelský účet systému Windows.</span><span class="sxs-lookup"><span data-stu-id="ff088-134">This is a convenience method for specifying the rule "SID=mine", where mine is the SID of the current Windows user account.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

<span data-ttu-id="ff088-135">V tomto scénáři je zodpovědný za distribuci šifrovací klíče, který používá rozhraní DPAPI NG operace řadič domény AD.</span><span class="sxs-lookup"><span data-stu-id="ff088-135">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="ff088-136">Cílový uživatel bude moci dešifrovat šifrovaná data z libovolného počítače připojeného k doméně (za předpokladu, že je proces spuštěný v rámci své identity).</span><span class="sxs-lookup"><span data-stu-id="ff088-136">The target user will be able to decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="ff088-137">Šifrování založené na certifikátech s Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="ff088-137">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="ff088-138">Pokud používáte systém na Windows 8.1 nebo Windows Server 2012 R2 nebo novější, můžete použít Windows DPAPI-NG k provedení na základě certifikátu šifrování, i v případě, že je aplikace spuštěna [.NET Core](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="ff088-138">If you're running on Windows 8.1 / Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption, even if the application is running on [.NET Core](https://www.microsoft.com/net/core).</span></span> <span data-ttu-id="ff088-139">Abyste mohli využívat tohoto objektu, použijte řetězce popisovače pravidla "certifikát = HashId:thumbprint", kde je kryptografický otisk kódováním šestnáctkově SHA1 kryptografický otisk certifikátu pro použití.</span><span class="sxs-lookup"><span data-stu-id="ff088-139">To take advantage of this, use the rule descriptor string "CERTIFICATE=HashId:thumbprint", where thumbprint is the hex-encoded SHA1 thumbprint of the certificate to use.</span></span> <span data-ttu-id="ff088-140">Níže najdete příklad.</span><span class="sxs-lookup"><span data-stu-id="ff088-140">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="ff088-141">Všechny aplikace, která je na tomto úložišti odkazoval musí být spuštěné na Windows 8.1 nebo Windows Server 2012 R2 nebo novější, abyste mohli dekódovat tento klíč.</span><span class="sxs-lookup"><span data-stu-id="ff088-141">Any application which is pointed at this repository must be running on Windows 8.1 / Windows Server 2012 R2 or later to be able to decipher this key.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="ff088-142">Vlastní šifrovací klíče</span><span class="sxs-lookup"><span data-stu-id="ff088-142">Custom key encryption</span></span>

<span data-ttu-id="ff088-143">Pokud v poli mechanismy nejsou vhodné, vývojáře můžete určit vlastní mechanismus šifrování pomocí klíče pomocí vlastní `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="ff088-143">If the in-box mechanisms are not appropriate, the developer can specify their own key encryption mechanism by providing a custom `IXmlEncryptor`.</span></span>
