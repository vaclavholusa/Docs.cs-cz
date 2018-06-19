---
title: Rozšíření správy klíčů v základní technologie ASP.NET
author: rick-anderson
description: Další informace o rozšíření ochrany dat ASP.NET Core správy klíčů.
manager: wpickett
ms.author: riande
ms.date: 11/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: e3042b371cf7be8fa0218c1906042d2810b180e3
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
ms.locfileid: "30074161"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="248ca-103">Rozšíření správy klíčů v základní technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="248ca-103">Key management extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="248ca-104">Pro čtení [Správa klíčů](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) části před čtením této části, jak vysvětluje některé základní koncepty za tato rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="248ca-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="248ca-105">Typy, které implementují některá z následujících rozhraní by měly být vláken pro více volající.</span><span class="sxs-lookup"><span data-stu-id="248ca-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="248ca-106">Key</span><span class="sxs-lookup"><span data-stu-id="248ca-106">Key</span></span>

<span data-ttu-id="248ca-107">`IKey` Rozhraní je základní reprezentace klíče v cryptosystem.</span><span class="sxs-lookup"><span data-stu-id="248ca-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="248ca-108">Termín používá se zde v abstraktní smysl, ne v tom smyslu literálu "kryptografické klíče materiálu".</span><span class="sxs-lookup"><span data-stu-id="248ca-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="248ca-109">Klíč má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="248ca-109">A key has the following properties:</span></span>

* <span data-ttu-id="248ca-110">Aktivace, vytvoření a datum vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="248ca-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="248ca-111">Stav odvolání</span><span class="sxs-lookup"><span data-stu-id="248ca-111">Revocation status</span></span>

* <span data-ttu-id="248ca-112">Identifikátor klíče (identifikátor GUID)</span><span class="sxs-lookup"><span data-stu-id="248ca-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="248ca-113">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="248ca-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="248ca-114">Kromě toho `IKey` zpřístupní `CreateEncryptor` metodu, která slouží k vytvoření [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance vázaný na tento klíč.</span><span class="sxs-lookup"><span data-stu-id="248ca-114">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="248ca-115">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="248ca-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="248ca-116">Kromě toho `IKey` zpřístupní `CreateEncryptorInstance` metodu, která slouží k vytvoření [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance vázaný na tento klíč.</span><span class="sxs-lookup"><span data-stu-id="248ca-116">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="248ca-117">Neexistuje žádné rozhraní API pro načtení nezpracovaná kryptografických materiálu z `IKey` instance.</span><span class="sxs-lookup"><span data-stu-id="248ca-117">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="248ca-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="248ca-118">IKeyManager</span></span>

<span data-ttu-id="248ca-119">`IKeyManager` Rozhraní představuje objekt zodpovědný za obecné úložiště klíčů, načítání a zpracování.</span><span class="sxs-lookup"><span data-stu-id="248ca-119">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="248ca-120">Poskytuje tři základní operace:</span><span class="sxs-lookup"><span data-stu-id="248ca-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="248ca-121">Vytvořte nový klíč a zachovat do úložiště.</span><span class="sxs-lookup"><span data-stu-id="248ca-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="248ca-122">Získáte všechny klíče z úložiště.</span><span class="sxs-lookup"><span data-stu-id="248ca-122">Get all keys from storage.</span></span>

* <span data-ttu-id="248ca-123">Odvolání jeden či více klíčů a zachovat informace o odvolání certifikátů do úložiště.</span><span class="sxs-lookup"><span data-stu-id="248ca-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="248ca-124">Zápis `IKeyManager` je velmi pokročilé úlohy a Většina vývojářů neměli.</span><span class="sxs-lookup"><span data-stu-id="248ca-124">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="248ca-125">Místo toho Většina vývojářů by měl využít výhod zařízení, které nabízí [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) třídy.</span><span class="sxs-lookup"><span data-stu-id="248ca-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="248ca-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="248ca-126">XmlKeyManager</span></span>

<span data-ttu-id="248ca-127">`XmlKeyManager` Typ je v poli konkrétní implementace `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="248ca-127">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="248ca-128">Poskytuje několik užitečné zařízení, včetně klíče úschově a šifrovací klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="248ca-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="248ca-129">Klíče v tomto systému jsou reprezentovány jako elementy XML (konkrétně [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="248ca-129">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="248ca-130">`XmlKeyManager` závisí na několik součástí vyřízení její úkoly:</span><span class="sxs-lookup"><span data-stu-id="248ca-130">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="248ca-131">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="248ca-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="248ca-132">`AlgorithmConfiguration`, který určuje algoritmy používané nových klíčů.</span><span class="sxs-lookup"><span data-stu-id="248ca-132">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="248ca-133">`IXmlRepository`, které ovládací prvky, kde jsou klíče uchovávané v úložišti.</span><span class="sxs-lookup"><span data-stu-id="248ca-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="248ca-134">`IXmlEncryptor` [Nepovinné], což umožňuje šifrování klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="248ca-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="248ca-135">`IKeyEscrowSink` [Nepovinné], který poskytuje služby klíče úschově.</span><span class="sxs-lookup"><span data-stu-id="248ca-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="248ca-136">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="248ca-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="248ca-137">`IXmlRepository`, které ovládací prvky, kde jsou klíče uchovávané v úložišti.</span><span class="sxs-lookup"><span data-stu-id="248ca-137">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="248ca-138">`IXmlEncryptor` [Nepovinné], což umožňuje šifrování klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="248ca-138">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="248ca-139">`IKeyEscrowSink` [Nepovinné], který poskytuje služby klíče úschově.</span><span class="sxs-lookup"><span data-stu-id="248ca-139">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="248ca-140">Níže jsou diagramy vysoké, které označují, jak jsou tyto součásti drátové společně v rámci `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="248ca-140">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="248ca-141">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="248ca-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Vytvoření klíče](key-management/_static/keycreation2.png)

   <span data-ttu-id="248ca-143">*Vytvoření klíče nebo CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="248ca-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="248ca-144">Při provádění `CreateNewKey`, `AlgorithmConfiguration` komponenta se používá k vytvoření jedinečný `IAuthenticatedEncryptorDescriptor`, který je pak serializovanou jako XML.</span><span class="sxs-lookup"><span data-stu-id="248ca-144">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="248ca-145">Pokud se klíče úschově podřízený nachází, nezpracovaná XML (nezašifrované) zajišťuje jímky pro dlouhodobé uložení.</span><span class="sxs-lookup"><span data-stu-id="248ca-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="248ca-146">Nezašifrované XML je spusťte `IXmlEncryptor` (v případě potřeby) ke generování šifrovaných dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="248ca-146">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="248ca-147">Tato zašifrovaného dokumentu je trvalé do dlouhodobého úložiště prostřednictvím `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="248ca-147">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="248ca-148">(Pokud žádné `IXmlEncryptor` je nakonfigurován, nezašifrované dokumentu je uchován v `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="248ca-148">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="248ca-149">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="248ca-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Vytvoření klíče](key-management/_static/keycreation1.png)

   <span data-ttu-id="248ca-151">*Vytvoření klíče nebo CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="248ca-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="248ca-152">Při provádění `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` komponenta se používá k vytvoření jedinečný `IAuthenticatedEncryptorDescriptor`, který je pak serializovanou jako XML.</span><span class="sxs-lookup"><span data-stu-id="248ca-152">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="248ca-153">Pokud se klíče úschově podřízený nachází, nezpracovaná XML (nezašifrované) zajišťuje jímky pro dlouhodobé uložení.</span><span class="sxs-lookup"><span data-stu-id="248ca-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="248ca-154">Nezašifrované XML je spusťte `IXmlEncryptor` (v případě potřeby) ke generování šifrovaných dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="248ca-154">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="248ca-155">Tato zašifrovaného dokumentu je trvalé do dlouhodobého úložiště prostřednictvím `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="248ca-155">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="248ca-156">(Pokud žádné `IXmlEncryptor` je nakonfigurován, nezašifrované dokumentu je uchován v `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="248ca-156">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="248ca-157">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="248ca-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Načítání klíče](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="248ca-159">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="248ca-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Načítání klíče](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="248ca-161">*Načítání klíče nebo GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="248ca-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="248ca-162">Při provádění `GetAllKeys`, XML dokumenty představující klíče a odvolání se načítají z základní `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="248ca-162">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="248ca-163">Pokud jsou tyto dokumenty zašifrované, systém bude automaticky jejich dešifrování.</span><span class="sxs-lookup"><span data-stu-id="248ca-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="248ca-164">`XmlKeyManager` vytvoří odpovídající `IAuthenticatedEncryptorDescriptorDeserializer` instance k deserializaci dokumenty zpět do `IAuthenticatedEncryptorDescriptor` instance, které jsou pak uzavřen do individuální `IKey` instance.</span><span class="sxs-lookup"><span data-stu-id="248ca-164">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="248ca-165">Tato kolekce `IKey` instance je vrácen volajícímu.</span><span class="sxs-lookup"><span data-stu-id="248ca-165">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="248ca-166">Další informace o konkrétní elementy XML lze nalézt v [úložiště klíčů formátovat dokument](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="248ca-166">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="248ca-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="248ca-167">IXmlRepository</span></span>

<span data-ttu-id="248ca-168">`IXmlRepository` Rozhraní představuje typ, který můžete zachovat XML tak, aby a načtení XML ze záložnímu úložišti.</span><span class="sxs-lookup"><span data-stu-id="248ca-168">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="248ca-169">Poskytuje dvě rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="248ca-169">It exposes two APIs:</span></span>

* <span data-ttu-id="248ca-170">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="248ca-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="248ca-171">StoreElement (XElement element, friendlyName řetězec)</span><span class="sxs-lookup"><span data-stu-id="248ca-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="248ca-172">Implementace `IXmlRepository` nemusíte analyzovat soubor XML prošla je.</span><span class="sxs-lookup"><span data-stu-id="248ca-172">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="248ca-173">Jejich považovat za neprůhledné dokumentů XML a nechat vyšší vrstvy starat o generování a analýza dokumenty.</span><span class="sxs-lookup"><span data-stu-id="248ca-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="248ca-174">Existují dva předdefinované konkrétní typy, které implementují třídu `IXmlRepository`: `FileSystemXmlRepository` a `RegistryXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="248ca-174">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="248ca-175">Najdete v článku [dokumentu zprostředkovatele úložiště klíčů](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers) Další informace.</span><span class="sxs-lookup"><span data-stu-id="248ca-175">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="248ca-176">Vlastní registrace `IXmlRepository` může být vhodným způsobem použít záložní úložiště, například Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="248ca-176">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="248ca-177">Chcete-li změnit výchozí úložiště celou aplikaci, zaregistrovat vlastní `IXmlRepository` instance:</span><span class="sxs-lookup"><span data-stu-id="248ca-177">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="248ca-178">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="248ca-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="248ca-179">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="248ca-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="248ca-180">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="248ca-180">IXmlEncryptor</span></span>

<span data-ttu-id="248ca-181">`IXmlEncryptor` Rozhraní představuje typ, který můžete šifrovat element XML ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="248ca-181">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="248ca-182">Zpřístupňuje jediného rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="248ca-182">It exposes a single API:</span></span>

* <span data-ttu-id="248ca-183">Šifrování (XElement plaintextElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="248ca-183">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="248ca-184">Pokud serializovaný seznam `IAuthenticatedEncryptorDescriptor` obsahuje všechny elementy označena jako "vyžaduje šifrování", pak `XmlKeyManager` tyto prvky se spustí pomocí konfigurovaného `IXmlEncryptor`na `Encrypt` metoda ale zachová enciphered element místo ve formátu prostého textu elementu na `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="248ca-184">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="248ca-185">Výstup `Encrypt` je metoda `EncryptedXmlInfo` objektu.</span><span class="sxs-lookup"><span data-stu-id="248ca-185">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="248ca-186">Tento objekt je obálky, který obsahuje výsledné enciphered `XElement` a typ, který představuje `IXmlDecryptor` které můžete používat k dešifrování odpovídající element.</span><span class="sxs-lookup"><span data-stu-id="248ca-186">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="248ca-187">Existují čtyři integrované konkrétní typy, které implementují třídu `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="248ca-187">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="248ca-188">Najdete v článku [klíče šifrování v dokumentu rest](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) Další informace.</span><span class="sxs-lookup"><span data-stu-id="248ca-188">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="248ca-189">Chcete-li změnit výchozí klíč šifrování na rest mechanismus celou aplikaci, zaregistrovat vlastní `IXmlEncryptor` instance:</span><span class="sxs-lookup"><span data-stu-id="248ca-189">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="248ca-190">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="248ca-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="248ca-191">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="248ca-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="248ca-192">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="248ca-192">IXmlDecryptor</span></span>

<span data-ttu-id="248ca-193">`IXmlDecryptor` Rozhraní představuje typ, který umí k dešifrování `XElement` , byl enciphered prostřednictvím `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="248ca-193">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="248ca-194">Zpřístupňuje jediného rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="248ca-194">It exposes a single API:</span></span>

* <span data-ttu-id="248ca-195">Dešifrování (XElement encryptedElement): XElement</span><span class="sxs-lookup"><span data-stu-id="248ca-195">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="248ca-196">`Decrypt` Metoda vrátí zpět provádí šifrování `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="248ca-196">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="248ca-197">Obecně platí, každý konkrétní `IXmlEncryptor` implementace bude mít odpovídající konkrétní `IXmlDecryptor` implementace.</span><span class="sxs-lookup"><span data-stu-id="248ca-197">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="248ca-198">Typy, které implementují rozhraní `IXmlDecryptor` musí mít jednu z následujících dvou veřejné konstruktory:</span><span class="sxs-lookup"><span data-stu-id="248ca-198">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="248ca-199">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="248ca-199">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="248ca-200">.ctor()</span><span class="sxs-lookup"><span data-stu-id="248ca-200">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="248ca-201">`IServiceProvider` Předaný konstruktoru může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="248ca-201">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="248ca-202">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="248ca-202">IKeyEscrowSink</span></span>

<span data-ttu-id="248ca-203">`IKeyEscrowSink` Rozhraní představuje typ, který může provádět úschově citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="248ca-203">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="248ca-204">Odvolat serializované popisovače můžou obsahovat citlivé informace (například kryptografické materiálů), a to je co vedla k uvedení [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) zadejte na prvním místě.</span><span class="sxs-lookup"><span data-stu-id="248ca-204">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="248ca-205">Ale dojít kvůli nehodám a klíč okruhy mohl být odstraněn, nebo dojde k poškození.</span><span class="sxs-lookup"><span data-stu-id="248ca-205">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="248ca-206">Poskytuje rozhraní úschově šrafování nouzový řídicí povolení přístupu k nezpracované serializovaných XML předtím, než je transformovat žádné nakonfigurované [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="248ca-206">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="248ca-207">Rozhraní zpřístupní jediného rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="248ca-207">The interface exposes a single API:</span></span>

* <span data-ttu-id="248ca-208">Úložiště (identifikátor Guid keyId, XElement element)</span><span class="sxs-lookup"><span data-stu-id="248ca-208">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="248ca-209">Je až `IKeyEscrowSink` implementace zpracování zadaný element zabezpečeným způsobem konzistentní s firemní zásady.</span><span class="sxs-lookup"><span data-stu-id="248ca-209">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="248ca-210">Jednou z možných implementací může být pro sink úschově k zašifrování elementu XML pomocí známých podnikové certifikátu X.509, kde má byla uloží privátní klíč certifikátu; `CertificateXmlEncryptor` typ s tím pomůže.</span><span class="sxs-lookup"><span data-stu-id="248ca-210">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="248ca-211">`IKeyEscrowSink` Implementace zodpovídá taky za uložením zadaný element správně.</span><span class="sxs-lookup"><span data-stu-id="248ca-211">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="248ca-212">Ve výchozím nastavení je aktivován žádný mechanismus úschově, když správci serveru můžete [nakonfigurujte tato globální](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="248ca-212">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="248ca-213">Můžete také nastavit prostřednictvím kódu programu prostřednictvím `IDataProtectionBuilder.AddKeyEscrowSink` metoda, jak je znázorněno v následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="248ca-213">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="248ca-214">`AddKeyEscrowSink` Zrcadlení přetížení metody `IServiceCollection.AddSingleton` a `IServiceCollection.AddInstance` přetížení, jako `IKeyEscrowSink` instance by měla být jednotlivé prvky.</span><span class="sxs-lookup"><span data-stu-id="248ca-214">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="248ca-215">Pokud je to více `IKeyEscrowSink` instance jsou zaregistrovány, každé z nich bude volána při generování klíče, klíče může být uloží je na více mechanismů současně.</span><span class="sxs-lookup"><span data-stu-id="248ca-215">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="248ca-216">Neexistuje žádné rozhraní API pro čtení materiálu z `IKeyEscrowSink` instance.</span><span class="sxs-lookup"><span data-stu-id="248ca-216">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="248ca-217">To je konzistentní s teoreticky návrhu mechanismu úschově: Tato má zpřístupněte materiál klíče pro důvěryhodnou autoritou, a vzhledem k tomu, že aplikace není samotné důvěryhodnou autoritou, by neměl mít přístup k vlastní escrowed materiálů.</span><span class="sxs-lookup"><span data-stu-id="248ca-217">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="248ca-218">Následující vzorový kód ukazuje vytvoření a registrace `IKeyEscrowSink` kde klíče se uloží tak, že je můžete obnovit pouze členové "CONTOSODomain Admins".</span><span class="sxs-lookup"><span data-stu-id="248ca-218">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="248ca-219">Chcete-li tuto ukázku spustit, musíte být v doméně systému Windows 8 / Windows Server 2012 počítače a řadič domény musí být Windows Server 2012 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="248ca-219">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
