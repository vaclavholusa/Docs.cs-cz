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
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="7281e-103">Šifrování klíče v klidovém stavu uložených v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7281e-103">Key encryption at rest in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-encryption-at-rest"></a>

<span data-ttu-id="7281e-104">Ve výchozím nastavení systému ochrany dat [využívá Heuristika](xref:security/data-protection/configuration/default-settings) k určení jak kryptografických materiál klíče by měla být v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="7281e-104">By default, the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine how cryptographic key material should be encrypted at rest.</span></span> <span data-ttu-id="7281e-105">Vývojář můžete přepsat heuristiky a ručně zadat, jak klíče by měla být v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="7281e-105">The developer can override the heuristic and manually specify how keys should be encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="7281e-106">Pokud zadáte explicitní klíče šifrování v mechanismus rest, bude systém ochrany dat zrušení registrace výchozího mechanismu úložiště klíčů, které poskytuje heuristiky.</span><span class="sxs-lookup"><span data-stu-id="7281e-106">If you specify an explicit key encryption at rest mechanism, the data protection system will deregister the default key storage mechanism that the heuristic provided.</span></span> <span data-ttu-id="7281e-107">Je nutné [zadejte mechanismus explicitní úložiště klíčů](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), jinak systému ochrany dat, nebude možné spustit.</span><span class="sxs-lookup"><span data-stu-id="7281e-107">You must [specify an explicit key storage mechanism](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), otherwise the data protection system will fail to start.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

<span data-ttu-id="7281e-108">Systém ochrany dat se dodává s tři mechanismy šifrovací klíče v poli.</span><span class="sxs-lookup"><span data-stu-id="7281e-108">The data protection system ships with three in-box key encryption mechanisms.</span></span>

## <a name="windows-dpapi"></a><span data-ttu-id="7281e-109">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="7281e-109">Windows DPAPI</span></span>

<span data-ttu-id="7281e-110">*Tento mechanismus je k dispozici pouze v systému Windows.*</span><span class="sxs-lookup"><span data-stu-id="7281e-110">*This mechanism is available only on Windows.*</span></span>

<span data-ttu-id="7281e-111">Pokud se používá rozhraní Windows DPAPI, materiál klíče, bude se šifrovat prostřednictvím [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) před byly trvalé do úložiště.</span><span class="sxs-lookup"><span data-stu-id="7281e-111">When Windows DPAPI is used, key material will be encrypted via [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) before being persisted to storage.</span></span> <span data-ttu-id="7281e-112">Rozhraní DPAPI je příslušným šifrovacím mechanismus pro data, která budou nikdy číst mimo aktuální počítač (přestože je možné zálohovat tyto klíče až do služby Active Directory, zjistit [DPAPI a cestovní profily](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="7281e-112">DPAPI is an appropriate encryption mechanism for data that will never be read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="7281e-113">Pro příklad konfigurace DPAPI šifrovací klíče na rest.</span><span class="sxs-lookup"><span data-stu-id="7281e-113">For example to configure DPAPI key-at-rest encryption.</span></span>

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

<span data-ttu-id="7281e-114">Pokud `ProtectKeysWithDpapi` je volána bez parametrů, pouze aktuální uživatelský účet systému Windows může dekódovat trvalou materiál klíče.</span><span class="sxs-lookup"><span data-stu-id="7281e-114">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key material.</span></span> <span data-ttu-id="7281e-115">Volitelně můžete zadat, že libovolný uživatelský účet v počítači (nikoli pouze aktuální uživatelský účet) měl být schopný dekódovat materiál klíče, jak je znázorněno následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="7281e-115">You can optionally specify that any user account on the machine (not just the current user account) should be able to decipher the key material, as shown in the below example.</span></span>

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a><span data-ttu-id="7281e-116">Certifikát X.509</span><span class="sxs-lookup"><span data-stu-id="7281e-116">X.509 certificate</span></span>

<span data-ttu-id="7281e-117">*Tento mechanismus není k dispozici na `.NET Core 1.0` nebo `1.1`.*</span><span class="sxs-lookup"><span data-stu-id="7281e-117">*This mechanism isn't available on `.NET Core 1.0` or `1.1`.*</span></span>

<span data-ttu-id="7281e-118">Pokud vaše aplikace je rozdělena mezi více počítačů, může být vhodné pro distribuci sdíleného certifikátu X.509 mezi počítači a konfigurace aplikací k použití tohoto certifikátu pro šifrování klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="7281e-118">If your application is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and to configure applications to use this certificate for encryption of keys at rest.</span></span> <span data-ttu-id="7281e-119">Níže najdete příklad.</span><span class="sxs-lookup"><span data-stu-id="7281e-119">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

<span data-ttu-id="7281e-120">Z důvodu omezení rozhraní .NET Framework jsou podporovány pouze certifikáty s CAPI privátních klíčů.</span><span class="sxs-lookup"><span data-stu-id="7281e-120">Due to .NET Framework limitations only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="7281e-121">V tématu [šifrování založené na certifikátech s Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) níže možná řešení těchto omezení.</span><span class="sxs-lookup"><span data-stu-id="7281e-121">See [Certificate-based encryption with Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) below for possible workarounds to these limitations.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a><span data-ttu-id="7281e-122">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="7281e-122">Windows DPAPI-NG</span></span>

<span data-ttu-id="7281e-123">*Tento mechanismus je k dispozici pouze v systému Windows 8 nebo Windows Server 2012 a novější.*</span><span class="sxs-lookup"><span data-stu-id="7281e-123">*This mechanism is available only on Windows 8 / Windows Server 2012 and later.*</span></span>

<span data-ttu-id="7281e-124">Počínaje Windows 8, operační systém podporuje rozhraní DPAPI-NG (také nazývané CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="7281e-124">Beginning with Windows 8, the operating system supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="7281e-125">Microsoft rozložen jeho scénáři použití následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="7281e-125">Microsoft lays out its usage scenario as follows.</span></span>

   <span data-ttu-id="7281e-126">Cloud computing ale často vyžaduje že být dešifrován na jiném tohoto obsahu šifrované na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="7281e-126">Cloud computing, however, often requires that content encrypted on one computer be decrypted on another.</span></span> <span data-ttu-id="7281e-127">Proto počínaje Windows 8, Microsoft rozšířené na nápad použití relativně jednoduché rozhraní API pro zahrnovat scénářích cloudu.</span><span class="sxs-lookup"><span data-stu-id="7281e-127">Therefore, beginning with Windows 8, Microsoft extended the idea of using a relatively straightforward API to encompass cloud scenarios.</span></span> <span data-ttu-id="7281e-128">Toto nové rozhraní API, volá rozhraní DPAPI-NG, umožňuje bezpečně sdílet tajné klíče (klíče, hesla, materiál klíče) a zprávy a chrání je na sadu objektů, které lze použít k odemknutí je na různých počítačích po správné ověření a autorizaci.</span><span class="sxs-lookup"><span data-stu-id="7281e-128">This new API, called DPAPI-NG, enables you to securely share secrets (keys, passwords, key material) and messages by protecting them to a set of principals that can be used to unprotect them on different computers after proper authentication and authorization.</span></span>

   <span data-ttu-id="7281e-129">Z [o CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="7281e-129">From [About CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span></span>

<span data-ttu-id="7281e-130">Objekt je kódovaná jako popisovač pravidla ochrany.</span><span class="sxs-lookup"><span data-stu-id="7281e-130">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="7281e-131">Vezměte v úvahu následujícím příkladu, který šifruje materiál klíče tak, aby pouze připojený k doméně uživatele se zadaným identifikátorem SID mohly dešifrovat materiál klíče.</span><span class="sxs-lookup"><span data-stu-id="7281e-131">Consider the below example, which encrypts key material such that only the domain-joined user with the specified SID can decrypt the key material.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="7281e-132">Je také bez parametrů přetížení `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="7281e-132">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="7281e-133">Toto je metoda užitečný pro zadání pravidlo "SID dolování =", kde dolování je SID aktuální uživatelský účet systému Windows.</span><span class="sxs-lookup"><span data-stu-id="7281e-133">This is a convenience method for specifying the rule "SID=mine", where mine is the SID of the current Windows user account.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

<span data-ttu-id="7281e-134">V tomto scénáři je zodpovědný za distribuci šifrovací klíče, který používá rozhraní DPAPI NG operace řadič domény AD.</span><span class="sxs-lookup"><span data-stu-id="7281e-134">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="7281e-135">Cílový uživatel bude moci dešifrovat šifrovaná data z libovolného počítače připojeného k doméně (za předpokladu, že je proces spuštěný v rámci své identity).</span><span class="sxs-lookup"><span data-stu-id="7281e-135">The target user will be able to decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="7281e-136">Šifrování založené na certifikátech s Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="7281e-136">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="7281e-137">Pokud používáte systém na Windows 8.1 nebo Windows Server 2012 R2 nebo novější, můžete použít Windows DPAPI-NG k provedení na základě certifikátu šifrování, i když aplikace běží na .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7281e-137">If you're running on Windows 8.1 / Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption, even if the application is running on .NET Core.</span></span> <span data-ttu-id="7281e-138">Abyste mohli využívat tohoto objektu, použijte řetězce popisovače pravidla "certifikát = HashId:thumbprint", kde je kryptografický otisk kódováním šestnáctkově SHA1 kryptografický otisk certifikátu pro použití.</span><span class="sxs-lookup"><span data-stu-id="7281e-138">To take advantage of this, use the rule descriptor string "CERTIFICATE=HashId:thumbprint", where thumbprint is the hex-encoded SHA1 thumbprint of the certificate to use.</span></span> <span data-ttu-id="7281e-139">Níže najdete příklad.</span><span class="sxs-lookup"><span data-stu-id="7281e-139">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="7281e-140">Všechny aplikace, která je na tomto úložišti odkazoval musí být spuštěné na Windows 8.1 nebo Windows Server 2012 R2 nebo novější, abyste mohli dekódovat tento klíč.</span><span class="sxs-lookup"><span data-stu-id="7281e-140">Any application which is pointed at this repository must be running on Windows 8.1 / Windows Server 2012 R2 or later to be able to decipher this key.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="7281e-141">Vlastní šifrovací klíče</span><span class="sxs-lookup"><span data-stu-id="7281e-141">Custom key encryption</span></span>

<span data-ttu-id="7281e-142">Pokud v poli mechanismy nejsou vhodné, vývojáře můžete určit vlastní mechanismus šifrování pomocí klíče pomocí vlastní `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="7281e-142">If the in-box mechanisms are not appropriate, the developer can specify their own key encryption mechanism by providing a custom `IXmlEncryptor`.</span></span>
