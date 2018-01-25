---
title: "Poskytovatelé úložiště klíčů"
author: rick-anderson
description: "Poskytovatelé úložiště klíčů"
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: d7cbb4786be0acf9679f43466460c3833f1db6fb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="key-storage-providers"></a><span data-ttu-id="51757-103">Poskytovatelé úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="51757-103">Key storage providers</span></span>

<a name="data-protection-implementation-key-storage-providers"></a>

<span data-ttu-id="51757-104">Ve výchozím nastavení systému ochrany dat [využívá Heuristika](xref:security/data-protection/configuration/default-settings) k určení, kde by měl natrvalo materiál kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="51757-104">By default the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="51757-105">Vývojář může přepsat heuristiky a ručně zadejte jeho umístění.</span><span class="sxs-lookup"><span data-stu-id="51757-105">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="51757-106">Pokud zadáte umístění služby explicitní trvalost klíče, bude systém ochrany dat zrušit šifrování klíče výchozí v rest mechanismus, který poskytuje heuristiky, tak klíče zašifruje už v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="51757-106">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="51757-107">Je doporučeno, můžete kromě [zadejte mechanismus explicitní šifrování klíče](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) aplikacích v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="51757-107">It's recommended that you additionally [specify an explicit key encryption mechanism](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="51757-108">Systém ochrany dat se dodává s několik poskytovatelů úložiště klíčů v poli.</span><span class="sxs-lookup"><span data-stu-id="51757-108">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="51757-109">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="51757-109">File system</span></span>

<span data-ttu-id="51757-110">Očekáváme, že mnoho aplikace bude používat klíče úložiště založené na systému souborů.</span><span class="sxs-lookup"><span data-stu-id="51757-110">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="51757-111">Chcete-li nakonfigurovat tuto funkci, volejte [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) rutiny konfigurace, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="51757-111">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="51757-112">Zadejte `DirectoryInfo` odkazující na úložiště, které se má uložit klíče.</span><span class="sxs-lookup"><span data-stu-id="51757-112">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="51757-113">`DirectoryInfo` Může ukazovat na adresář v místním počítači, nebo můžete přejít do složky ve sdílené síťové složce.</span><span class="sxs-lookup"><span data-stu-id="51757-113">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="51757-114">Pokud přejdete do adresáře v místním počítači (a tento scénář je, že jenom aplikace v místním počítači, bude třeba použít toto úložiště), zvažte použití [rozhraní Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) k šifrování klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="51757-114">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="51757-115">V opačném případě zvažte použití [certifikát X.509](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) k šifrování klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="51757-115">Otherwise consider using an [X.509 certificate](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="51757-116">Azure a Redis</span><span class="sxs-lookup"><span data-stu-id="51757-116">Azure and Redis</span></span>

<span data-ttu-id="51757-117">`Microsoft.AspNetCore.DataProtection.AzureStorage` a `Microsoft.AspNetCore.DataProtection.Redis` balíčky povolit ukládání klíče ochrany dat v Azure Storage nebo mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="51757-117">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="51757-118">Klíče můžete sdílet mezi několika instancí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="51757-118">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="51757-119">Aplikace ASP.NET Core můžete sdílet soubory cookie pro ověřování nebo ochrany proti útokům CSRF napříč několika servery.</span><span class="sxs-lookup"><span data-stu-id="51757-119">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="51757-120">Pokud chcete konfigurovat v Azure, volání jednoho z [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) přetížení, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="51757-120">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="51757-121">Viz také [Azure testovacího kódu](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="51757-121">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="51757-122">Pokud chcete konfigurovat na Redis, volání jednoho z [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) přetížení, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="51757-122">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

<span data-ttu-id="51757-123">Najdete následující informace:</span><span class="sxs-lookup"><span data-stu-id="51757-123">See the following for more information:</span></span>

- [<span data-ttu-id="51757-124">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="51757-124">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="51757-125">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="51757-125">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="51757-126">[Redis testovacího kódu](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="51757-126">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="51757-127">Registru</span><span class="sxs-lookup"><span data-stu-id="51757-127">Registry</span></span>

<span data-ttu-id="51757-128">Aplikace v některých případech nemusí mít oprávnění k zápisu do systému souborů.</span><span class="sxs-lookup"><span data-stu-id="51757-128">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="51757-129">Představte si třeba situaci, kdy aplikace běží jako účet služby virtual (jako je identita fondu aplikací na w3wp.exe).</span><span class="sxs-lookup"><span data-stu-id="51757-129">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="51757-130">V těchto případech může správce zřizuje klíč registru, který je vhodné ACLed identitu účtu služby.</span><span class="sxs-lookup"><span data-stu-id="51757-130">In these cases, the administrator may have provisioned a registry key that's appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="51757-131">Volání [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) rutiny konfigurace, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="51757-131">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="51757-132">Zadejte `RegistryKey` odkazující na umístění pro uložení kryptografického klíče nebo hodnoty.</span><span class="sxs-lookup"><span data-stu-id="51757-132">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="51757-133">Pokud používáte systémového registru jako vhodný mechanismus trvalosti, zvažte použití [rozhraní Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) k šifrování klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="51757-133">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="51757-134">Vlastní úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="51757-134">Custom key repository</span></span>

<span data-ttu-id="51757-135">Pokud v poli mechanismy nejsou vhodné, vývojáře můžete určit vlastní mechanismu trvalosti klíče pomocí vlastní `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="51757-135">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
