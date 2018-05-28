---
title: Konfigurovat ochranu dat ASP.NET Core
author: rick-anderson
description: Informace o konfiguraci ochrany dat v ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 803b81f5f69496900791ca1d1976f70f8c266f29
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/27/2018
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="8fb30-103">Konfigurovat ochranu dat ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8fb30-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="8fb30-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8fb30-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8fb30-105">Při inicializaci systému ochrany dat, se vztahuje [výchozí nastavení](xref:security/data-protection/configuration/default-settings) na základě provozní prostředí.</span><span class="sxs-lookup"><span data-stu-id="8fb30-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="8fb30-106">Tato nastavení jsou obecně vhodné pro aplikace běžící na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="8fb30-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="8fb30-107">Existují případy, kdy vývojář chtít změnit výchozí nastavení:</span><span class="sxs-lookup"><span data-stu-id="8fb30-107">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="8fb30-108">Aplikace je rozdělena mezi více počítačů.</span><span class="sxs-lookup"><span data-stu-id="8fb30-108">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="8fb30-109">Kvůli dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="8fb30-109">For compliance reasons.</span></span>

<span data-ttu-id="8fb30-110">Pro tyto scénáře ochrany dat systému nabízí bohaté konfigurační rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8fb30-110">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="8fb30-111">Podobně jako konfigurační soubory, prstenec klíč ochrany dat je třeba chránit pomocí příslušná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="8fb30-111">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="8fb30-112">Můžete šifrovat klíče v klidovém stavu, ale to není zabránit útočníkům ve vytváření nových klíčů.</span><span class="sxs-lookup"><span data-stu-id="8fb30-112">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="8fb30-113">V důsledku toho je dopad na zabezpečení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="8fb30-113">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="8fb30-114">Umístění úložiště, který je nakonfigurovaný s ochranou dat by měl mít přístup omezené na sebe, podobně jako by chránit konfigurační soubory aplikace.</span><span class="sxs-lookup"><span data-stu-id="8fb30-114">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="8fb30-115">Pokud zvolíte možnost prstenec váš klíč uložit na disk, například pomocí oprávnění systému souborů.</span><span class="sxs-lookup"><span data-stu-id="8fb30-115">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="8fb30-116">Ujistěte se, jenom identitě v rámci kterého běží vaše webová aplikace má ke čtení, zápisu a vytvořte přístup do adresáře.</span><span class="sxs-lookup"><span data-stu-id="8fb30-116">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="8fb30-117">Pokud používáte Azure Table Storage, webové aplikace musí mít možnost čtení, zápisu nebo vytvořit nové položky v tabulce úložiště atd.</span><span class="sxs-lookup"><span data-stu-id="8fb30-117">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="8fb30-118">Metody rozšíření [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) vrátí [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="8fb30-118">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="8fb30-119">`IDataProtectionBuilder` zpřístupní metody rozšíření, můžete společně zřetězit ke konfiguraci ochrany dat možnosti.</span><span class="sxs-lookup"><span data-stu-id="8fb30-119">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="8fb30-120">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="8fb30-120">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="8fb30-121">K ukládání klíčů v [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), nakonfigurujte systém s [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) v `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="8fb30-121">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="8fb30-122">Nastavení umístění úložiště prstenec klíč (například [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="8fb30-122">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="8fb30-123">Umístění musí být nastaveno, protože volání `ProtectKeysWithAzureKeyVault` implementuje [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) zakazující nastavení ochrany dat, včetně umístění úložiště prstenec klíč.</span><span class="sxs-lookup"><span data-stu-id="8fb30-123">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="8fb30-124">V předchozím příkladu používá Azure Blob Storage se zachovat prstenec klíč.</span><span class="sxs-lookup"><span data-stu-id="8fb30-124">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="8fb30-125">Další informace najdete v tématu [klíče poskytovatelé úložiště: Azure a Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="8fb30-125">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="8fb30-126">Můžete také zachovat prstenec klíč místně s [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="8fb30-126">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="8fb30-127">`keyIdentifier` Je identifikátor klíče trezoru klíčů pro šifrování klíče (například `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span><span class="sxs-lookup"><span data-stu-id="8fb30-127">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="8fb30-128">`ProtectKeysWithAzureKeyVault` přetížení:</span><span class="sxs-lookup"><span data-stu-id="8fb30-128">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="8fb30-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) umožňuje použití [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) systém ochrany dat používat Trezor klíčů povolit.</span><span class="sxs-lookup"><span data-stu-id="8fb30-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="8fb30-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, řetězec, řetězec, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) umožňuje použití `ClientId` a [certifikátu x 509](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) systém ochrany dat používat Trezor klíčů povolit.</span><span class="sxs-lookup"><span data-stu-id="8fb30-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="8fb30-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, řetězec, řetězec, řetězec)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) umožňuje použití `ClientId` a `ClientSecret` systém ochrany dat používat Trezor klíčů povolit.</span><span class="sxs-lookup"><span data-stu-id="8fb30-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="8fb30-132">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="8fb30-132">PersistKeysToFileSystem</span></span>

<span data-ttu-id="8fb30-133">Chcete ukládat klíče na sdílenou jednotku UNC místo na *LOCALAPPDATA %* výchozí umístění, nakonfigurujte systém s [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="8fb30-133">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="8fb30-134">Pokud změníte umístění klíče trvalost, systém šifruje nebude automaticky klíče v klidu, vzhledem k tomu, že není známo, zda je rozhraní DPAPI mechanismus příslušným šifrovacím.</span><span class="sxs-lookup"><span data-stu-id="8fb30-134">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="8fb30-135">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="8fb30-135">ProtectKeysWith\*</span></span>

<span data-ttu-id="8fb30-136">Můžete nakonfigurovat systému k ochraně klíče v klidovém stavu pomocí některé z volání [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) rozhraní API pro konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="8fb30-136">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="8fb30-137">Vezměte v úvahu následující příklad, který ukládá klíče na sdílenou jednotku UNC a šifruje tyto klíče v klidovém stavu s konkrétní certifikát X.509:</span><span class="sxs-lookup"><span data-stu-id="8fb30-137">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="8fb30-138">V tématu [klíč šifrování na Rest](xref:security/data-protection/implementation/key-encryption-at-rest) Další příklady a zabývat mechanismy integrovaný šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="8fb30-138">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="8fb30-139">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="8fb30-139">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="8fb30-140">Chcete-li konfigurovat systém pro použití klíče životnost 14 dnů místo výchozí 90 dnů, použijte [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="8fb30-140">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="8fb30-141">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="8fb30-141">SetApplicationName</span></span>

<span data-ttu-id="8fb30-142">Ve výchozím nastavení ochrany dat systému izoluje aplikace od sebe navzájem i v případě, že se jejich sdílení stejného fyzického úložiště klíčů.</span><span class="sxs-lookup"><span data-stu-id="8fb30-142">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="8fb30-143">To brání pochopení vzájemně chráněných datových částí aplikace.</span><span class="sxs-lookup"><span data-stu-id="8fb30-143">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="8fb30-144">Chcete-li sdílet chráněné datové části mezi dvěma aplikacemi, použijte [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) se stejnou hodnotou pro každou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="8fb30-144">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="8fb30-145">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="8fb30-145">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="8fb30-146">Můžete mít scénář, kde nechcete, aby aplikace automaticky vrátit klíče (vytvořit nové klíče) jako přiblížení se vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="8fb30-146">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="8fb30-147">Příkladem toho je možné nastavit ve vztahu primární a sekundární, kde je zodpovědná za riziko z hlediska správy klíčů pouze aplikace primární a sekundární aplikací mít jednoduše zobrazení jen pro čtení prstence klíč aplikace.</span><span class="sxs-lookup"><span data-stu-id="8fb30-147">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="8fb30-148">Sekundární aplikace lze nakonfigurovat, aby považovat za prstenec klíč jen pro čtení tak, že nakonfigurujete systém s [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="8fb30-148">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="8fb30-149">Izolace za aplikací</span><span class="sxs-lookup"><span data-stu-id="8fb30-149">Per-application isolation</span></span>

<span data-ttu-id="8fb30-150">Je-li ASP.NET Core hostitelů v systému ochrany dat, je aplikace od sebe navzájem automaticky izoluje, i když tyto aplikace běží pod stejným účtem pracovní proces a používají stejné materiál hlavního klíče.</span><span class="sxs-lookup"><span data-stu-id="8fb30-150">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="8fb30-151">Toto je trochu podobné modifikátor IsolateApps z prostředí System.Web na  **\<machineKey >** element.</span><span class="sxs-lookup"><span data-stu-id="8fb30-151">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="8fb30-152">Tento mechanismus izolace funguje tak, že s každou aplikaci v místním počítači jako jedinečný klienta, proto [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) root pro aplikace automaticky zahrnují ID aplikace jako diskriminátoru.</span><span class="sxs-lookup"><span data-stu-id="8fb30-152">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="8fb30-153">Jedinečné ID aplikace pochází z jednoho ze dvou míst:</span><span class="sxs-lookup"><span data-stu-id="8fb30-153">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="8fb30-154">Pokud je aplikace hostované ve službě IIS, je jedinečný identifikátor cesta konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="8fb30-154">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="8fb30-155">Pokud je aplikace nasazená v prostředí webové farmy, tato hodnota by měla být stabilní, za předpokladu, že prostředí služby IIS jsou nakonfigurované podobně mezi všechny počítače ve webové farmě.</span><span class="sxs-lookup"><span data-stu-id="8fb30-155">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="8fb30-156">Pokud aplikace není hostovaný ve službě IIS, je jedinečný identifikátor fyzickou cestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="8fb30-156">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="8fb30-157">Jedinečný identifikátor slouží k zůstanou platné i po resetování &mdash; jednotlivé aplikace a počítače sám sebe.</span><span class="sxs-lookup"><span data-stu-id="8fb30-157">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="8fb30-158">Tento mechanismus izolace předpokládá, že aplikací nejsou škodlivý.</span><span class="sxs-lookup"><span data-stu-id="8fb30-158">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="8fb30-159">Škodlivý aplikace může ovlivnit vždy žádné jiné aplikace spuštěna pod stejným účtem pracovní proces.</span><span class="sxs-lookup"><span data-stu-id="8fb30-159">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="8fb30-160">Ve sdíleném hostování prostředí kde aplikace jsou vzájemně nedůvěryhodných by měl poskytovatel hostingu provést kroky k zajištění OS úroveň izolace mezi aplikacemi, včetně oddělení aplikací základní klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="8fb30-160">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="8fb30-161">Pokud není zadaný systému ochrany dat ASP.NET Core hostitelů v (například pokud instanci můžete vytvořit pomocí `DataProtectionProvider` konkrétní typ) izolace aplikace ve výchozím nastavení vypnutá.</span><span class="sxs-lookup"><span data-stu-id="8fb30-161">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="8fb30-162">Při izolaci aplikace je zakázaná, můžete všechny aplikace založenou na stejný klíčový materiál tak dlouho, dokud poskytují odpovídající sdílet datových částí [účely](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="8fb30-162">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="8fb30-163">K zajištění izolace aplikace v tomto prostředí, volání [SetApplicationName](#setapplicationname) metoda na konfiguraci objektu a zadejte jedinečný název pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8fb30-163">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="8fb30-164">Změna algoritmy s UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="8fb30-164">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="8fb30-165">Ochrana dat zásobníku umožňuje změnit výchozí algoritmus používaný nově vygenerované klíče.</span><span class="sxs-lookup"><span data-stu-id="8fb30-165">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="8fb30-166">Nejjednodušší způsob, jak to udělat je volat [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) ze zpětného volání konfigurace:</span><span class="sxs-lookup"><span data-stu-id="8fb30-166">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8fb30-167">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="8fb30-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8fb30-168">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8fb30-168">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="8fb30-169">Výchozí hodnota EncryptionAlgorithm je AES-256-CBC a ValidationAlgorithm výchozí hodnota je HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="8fb30-169">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="8fb30-170">Výchozí zásady můžete nastavit, že správce systému prostřednictvím [celého zásad](xref:security/data-protection/configuration/machine-wide-policy), ale explicitní volání `UseCryptographicAlgorithms` přepíše výchozí zásadu.</span><span class="sxs-lookup"><span data-stu-id="8fb30-170">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="8fb30-171">Volání metody `UseCryptographicAlgorithms` vám umožní určit požadovanou algoritmus z předdefinovaného seznamu předdefinované.</span><span class="sxs-lookup"><span data-stu-id="8fb30-171">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="8fb30-172">Nemusíte si dělat starosti o implementaci algoritmu.</span><span class="sxs-lookup"><span data-stu-id="8fb30-172">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="8fb30-173">Ve scénáři výše ochrany dat systému pokusí použít CNG implementace AES, pokud v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="8fb30-173">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="8fb30-174">Jinak se vrátí k spravovaný [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) třídy.</span><span class="sxs-lookup"><span data-stu-id="8fb30-174">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="8fb30-175">Můžete ručně zadat implementace prostřednictvím volání [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="8fb30-175">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="8fb30-176">Změna algoritmy neovlivní existující klíče v řetězci klíč.</span><span class="sxs-lookup"><span data-stu-id="8fb30-176">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="8fb30-177">Ovlivňuje jenom nově vygenerované klíče.</span><span class="sxs-lookup"><span data-stu-id="8fb30-177">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="8fb30-178">Určení vlastních spravované algoritmy</span><span class="sxs-lookup"><span data-stu-id="8fb30-178">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8fb30-179">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="8fb30-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8fb30-180">Pokud chcete zadat vlastní spravované algoritmů, vytvářet [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance, která odkazuje na typy implementace:</span><span class="sxs-lookup"><span data-stu-id="8fb30-180">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8fb30-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8fb30-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8fb30-182">Pokud chcete zadat vlastní spravované algoritmů, vytvářet [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance, která odkazuje na typy implementace:</span><span class="sxs-lookup"><span data-stu-id="8fb30-182">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

<span data-ttu-id="8fb30-183">Obecně \*typ vlastnosti musí odkazovat na konkrétní, instantiable (prostřednictvím veřejného konstruktoru bez parametrů) implementace [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) a [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), ačkoli systém některé hodnoty jako na případy zvláštní `typeof(Aes)` ke zvýšení pohodlí.</span><span class="sxs-lookup"><span data-stu-id="8fb30-183">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="8fb30-184">SymmetricAlgorithm musí mít délku klíče ≥ 128 bitů a velikost bloku ≥ 64bitová verze a musí podporovat režimu CBC šifrování s odsazení PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="8fb30-184">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="8fb30-185">KeyedHashAlgorithm musí mít velikost digest > = 128 bitů, a musí podporovat klíče délce délku digest algoritmus hash.</span><span class="sxs-lookup"><span data-stu-id="8fb30-185">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="8fb30-186">KeyedHashAlgorithm není nezbytně nutné být HMAC s klíčem.</span><span class="sxs-lookup"><span data-stu-id="8fb30-186">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="8fb30-187">Určení vlastních algoritmy Windows CNG</span><span class="sxs-lookup"><span data-stu-id="8fb30-187">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8fb30-188">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="8fb30-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8fb30-189">Chcete-li zadat vlastní Windows CNG algoritmus HMAC s klíčem ověření pomocí režimu CBC šifrování, vytvořte [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance, který obsahuje algoritmické informace:</span><span class="sxs-lookup"><span data-stu-id="8fb30-189">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8fb30-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8fb30-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8fb30-191">Chcete-li zadat vlastní Windows CNG algoritmus HMAC s klíčem ověření pomocí režimu CBC šifrování, vytvořte [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance, který obsahuje algoritmické informace:</span><span class="sxs-lookup"><span data-stu-id="8fb30-191">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> <span data-ttu-id="8fb30-192">Šifrovací algoritmus symetrického bloku musí mít délku klíče > = 128 bitů, velikost bloku > = 64 bitů, a musí podporovat režimu CBC šifrování s odsazení PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="8fb30-192">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="8fb30-193">Algoritmus hash musí mít velikost digest > = 128 bitů a musí podporovat otevíráte s BCRYPT\_ALG\_zpracování\_HMAC\_příznak příznak.</span><span class="sxs-lookup"><span data-stu-id="8fb30-193">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="8fb30-194">\*Lze nastavit vlastnosti zprostředkovatele hodnoty null lze použít výchozího poskytovatele pro zadaný algoritmus.</span><span class="sxs-lookup"><span data-stu-id="8fb30-194">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="8fb30-195">Najdete v článku [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Další informace naleznete v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="8fb30-195">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8fb30-196">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="8fb30-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8fb30-197">Chcete-li zadat vlastní Windows CNG algoritmus šifrování Galois/čítač režimu pomocí ověření, vytvořte [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance, který obsahuje algoritmické informace:</span><span class="sxs-lookup"><span data-stu-id="8fb30-197">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8fb30-198">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8fb30-198">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8fb30-199">Chcete-li zadat vlastní Windows CNG algoritmus šifrování Galois/čítač režimu pomocí ověření, vytvořte [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance, který obsahuje algoritmické informace:</span><span class="sxs-lookup"><span data-stu-id="8fb30-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> <span data-ttu-id="8fb30-200">Šifrovací algoritmus symetrického bloku musí mít délku klíče > = 128 bitů, velikost bloku přesně 128 bitů, a musí podporovat GCM šifrování.</span><span class="sxs-lookup"><span data-stu-id="8fb30-200">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="8fb30-201">Můžete nastavit [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) vlastnost na hodnotu null. Chcete-li použít výchozího poskytovatele pro zadaný algoritmus.</span><span class="sxs-lookup"><span data-stu-id="8fb30-201">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="8fb30-202">Najdete v článku [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Další informace naleznete v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="8fb30-202">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="8fb30-203">Určení dalších vlastních algoritmů</span><span class="sxs-lookup"><span data-stu-id="8fb30-203">Specifying other custom algorithms</span></span>

<span data-ttu-id="8fb30-204">I když nejsou viditelné jako první třídy rozhraní API, ochrany dat systému je dostatečně extensible umožňující zadání téměř k libovolnému druhu algoritmu.</span><span class="sxs-lookup"><span data-stu-id="8fb30-204">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="8fb30-205">Například je možné, aby všechny klíče, které jsou obsažené v modulu hardwarového zabezpečení (HSM) a k zajištění vlastní implementace základních šifrování a dešifrování rutiny.</span><span class="sxs-lookup"><span data-stu-id="8fb30-205">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="8fb30-206">V tématu [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) v [základní kryptografie rozšiřitelnost](xref:security/data-protection/extensibility/core-crypto) Další informace.</span><span class="sxs-lookup"><span data-stu-id="8fb30-206">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="8fb30-207">Zachování klíče při hostování v kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="8fb30-207">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="8fb30-208">Při hostování v [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) kontejneru klíčů by se měl zachovat buď:</span><span class="sxs-lookup"><span data-stu-id="8fb30-208">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="8fb30-209">Složka, která je Docker svazek, který potrvají nad rámec životnost kontejneru, například sdíleného svazku nebo hostitele připojeného svazku.</span><span class="sxs-lookup"><span data-stu-id="8fb30-209">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="8fb30-210">Externího poskytovatele, jako například [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) nebo [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="8fb30-210">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="8fb30-211">Viz také</span><span class="sxs-lookup"><span data-stu-id="8fb30-211">See also</span></span>

* [<span data-ttu-id="8fb30-212">Scénáře využívající bez DI</span><span class="sxs-lookup"><span data-stu-id="8fb30-212">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="8fb30-213">Široké zásady počítače</span><span class="sxs-lookup"><span data-stu-id="8fb30-213">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
