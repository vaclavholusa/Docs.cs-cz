---
title: "Rozšíření správy klíčů"
author: rick-anderson
description: "Tento dokument popisuje rozšiřitelnost správy klíčů ochrany dat ASP.NET Core."
keywords: "Správa klíčů ASP.NET Core, ochrany dat"
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 0702e13163c0208e9d2863e711b02ffb257f6260
ms.sourcegitcommit: e641c5794525f983485621860926d8ab4e7360c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/23/2017
---
# <a name="key-management-extensibility"></a><span data-ttu-id="50582-104">Rozšíření správy klíčů</span><span class="sxs-lookup"><span data-stu-id="50582-104">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="50582-105">Pro čtení [Správa klíčů](../implementation/key-management.md#data-protection-implementation-key-management) části před čtením této části, jak vysvětluje některé základní koncepty za tato rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="50582-105">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="50582-106">Typy, které implementují některá z následujících rozhraní by měly být vláken pro více volající.</span><span class="sxs-lookup"><span data-stu-id="50582-106">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="50582-107">Key</span><span class="sxs-lookup"><span data-stu-id="50582-107">Key</span></span>

<span data-ttu-id="50582-108">`IKey` Rozhraní je základní reprezentace klíče v cryptosystem.</span><span class="sxs-lookup"><span data-stu-id="50582-108">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="50582-109">Termín používá se zde v abstraktní smysl, ne v tom smyslu literálu "kryptografické klíče materiálu".</span><span class="sxs-lookup"><span data-stu-id="50582-109">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="50582-110">Klíč má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="50582-110">A key has the following properties:</span></span>

* <span data-ttu-id="50582-111">Aktivace, vytvoření a datum vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="50582-111">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="50582-112">Stav odvolání</span><span class="sxs-lookup"><span data-stu-id="50582-112">Revocation status</span></span>

* <span data-ttu-id="50582-113">Identifikátor klíče (identifikátor GUID)</span><span class="sxs-lookup"><span data-stu-id="50582-113">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50582-114">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="50582-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="50582-115">Kromě toho `IKey` zpřístupní `CreateEncryptor` metodu, která slouží k vytvoření [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance vázaný na tento klíč.</span><span class="sxs-lookup"><span data-stu-id="50582-115">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50582-116">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="50582-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="50582-117">Kromě toho `IKey` zpřístupní `CreateEncryptorInstance` metodu, která slouží k vytvoření [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance vázaný na tento klíč.</span><span class="sxs-lookup"><span data-stu-id="50582-117">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="50582-118">Neexistuje žádné rozhraní API pro načtení nezpracovaná kryptografických materiálu z `IKey` instance.</span><span class="sxs-lookup"><span data-stu-id="50582-118">There is no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="50582-119">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="50582-119">IKeyManager</span></span>

<span data-ttu-id="50582-120">`IKeyManager` Rozhraní představuje objekt zodpovědný za obecné úložiště klíčů, načítání a zpracování.</span><span class="sxs-lookup"><span data-stu-id="50582-120">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="50582-121">Poskytuje tři základní operace:</span><span class="sxs-lookup"><span data-stu-id="50582-121">It exposes three high-level operations:</span></span>

* <span data-ttu-id="50582-122">Vytvořte nový klíč a zachovat do úložiště.</span><span class="sxs-lookup"><span data-stu-id="50582-122">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="50582-123">Získáte všechny klíče z úložiště.</span><span class="sxs-lookup"><span data-stu-id="50582-123">Get all keys from storage.</span></span>

* <span data-ttu-id="50582-124">Odvolání jeden či více klíčů a zachovat informace o odvolání certifikátů do úložiště.</span><span class="sxs-lookup"><span data-stu-id="50582-124">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="50582-125">Zápis `IKeyManager` je velmi pokročilé úlohy a Většina vývojářů neměli.</span><span class="sxs-lookup"><span data-stu-id="50582-125">Writing an `IKeyManager` is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="50582-126">Místo toho Většina vývojářů by měl využít výhod zařízení, které nabízí [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) třídy.</span><span class="sxs-lookup"><span data-stu-id="50582-126">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="50582-127">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="50582-127">XmlKeyManager</span></span>

<span data-ttu-id="50582-128">`XmlKeyManager` Typ je v poli konkrétní implementace `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="50582-128">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="50582-129">Poskytuje několik užitečné zařízení, včetně klíče úschově a šifrovací klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="50582-129">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="50582-130">Klíče v tomto systému jsou reprezentovány jako elementy XML (konkrétně [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="50582-130">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="50582-131">`XmlKeyManager`závisí na několik součástí vyřízení její úkoly:</span><span class="sxs-lookup"><span data-stu-id="50582-131">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50582-132">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="50582-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="50582-133">`AlgorithmConfiguration`, který určuje algoritmy používané nových klíčů.</span><span class="sxs-lookup"><span data-stu-id="50582-133">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="50582-134">`IXmlRepository`, které ovládací prvky, kde jsou klíče uchovávané v úložišti.</span><span class="sxs-lookup"><span data-stu-id="50582-134">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="50582-135">`IXmlEncryptor`[Nepovinné], což umožňuje šifrování klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="50582-135">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="50582-136">`IKeyEscrowSink`[Nepovinné], který poskytuje služby klíče úschově.</span><span class="sxs-lookup"><span data-stu-id="50582-136">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50582-137">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="50582-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="50582-138">`IXmlRepository`, které ovládací prvky, kde jsou klíče uchovávané v úložišti.</span><span class="sxs-lookup"><span data-stu-id="50582-138">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="50582-139">`IXmlEncryptor`[Nepovinné], což umožňuje šifrování klíče v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="50582-139">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="50582-140">`IKeyEscrowSink`[Nepovinné], který poskytuje služby klíče úschově.</span><span class="sxs-lookup"><span data-stu-id="50582-140">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="50582-141">Níže jsou diagramy vysoké, které označují, jak jsou tyto součásti drátové společně v rámci `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="50582-141">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50582-142">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="50582-142">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Vytvoření klíče](key-management/_static/keycreation2.png)

   <span data-ttu-id="50582-144">*Vytvoření klíče nebo CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="50582-144">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="50582-145">Při provádění `CreateNewKey`, `AlgorithmConfiguration` komponenta se používá k vytvoření jedinečný `IAuthenticatedEncryptorDescriptor`, který je pak serializovanou jako XML.</span><span class="sxs-lookup"><span data-stu-id="50582-145">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="50582-146">Pokud se klíče úschově podřízený nachází, nezpracovaná XML (nezašifrované) zajišťuje jímky pro dlouhodobé uložení.</span><span class="sxs-lookup"><span data-stu-id="50582-146">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="50582-147">Nezašifrované XML je spusťte `IXmlEncryptor` (v případě potřeby) ke generování šifrovaných dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="50582-147">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="50582-148">Tato zašifrovaného dokumentu je trvalé do dlouhodobého úložiště prostřednictvím `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="50582-148">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="50582-149">(Pokud žádné `IXmlEncryptor` je nakonfigurován, nezašifrované dokumentu je uchován v `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="50582-149">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50582-150">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="50582-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Vytvoření klíče](key-management/_static/keycreation1.png)

   <span data-ttu-id="50582-152">*Vytvoření klíče nebo CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="50582-152">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="50582-153">Při provádění `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` komponenta se používá k vytvoření jedinečný `IAuthenticatedEncryptorDescriptor`, který je pak serializovanou jako XML.</span><span class="sxs-lookup"><span data-stu-id="50582-153">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="50582-154">Pokud se klíče úschově podřízený nachází, nezpracovaná XML (nezašifrované) zajišťuje jímky pro dlouhodobé uložení.</span><span class="sxs-lookup"><span data-stu-id="50582-154">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="50582-155">Nezašifrované XML je spusťte `IXmlEncryptor` (v případě potřeby) ke generování šifrovaných dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="50582-155">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="50582-156">Tato zašifrovaného dokumentu je trvalé do dlouhodobého úložiště prostřednictvím `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="50582-156">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="50582-157">(Pokud žádné `IXmlEncryptor` je nakonfigurován, nezašifrované dokumentu je uchován v `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="50582-157">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50582-158">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="50582-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Načítání klíče](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50582-160">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="50582-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Načítání klíče](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="50582-162">*Načítání klíče nebo GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="50582-162">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="50582-163">Při provádění `GetAllKeys`, XML dokumenty představující klíče a odvolání se načítají z základní `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="50582-163">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="50582-164">Pokud jsou tyto dokumenty zašifrované, systém bude automaticky jejich dešifrování.</span><span class="sxs-lookup"><span data-stu-id="50582-164">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="50582-165">`XmlKeyManager`vytvoří odpovídající `IAuthenticatedEncryptorDescriptorDeserializer` instance k deserializaci dokumenty zpět do `IAuthenticatedEncryptorDescriptor` instance, které jsou pak uzavřen do individuální `IKey` instance.</span><span class="sxs-lookup"><span data-stu-id="50582-165">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="50582-166">Tato kolekce `IKey` instance je vrácen volajícímu.</span><span class="sxs-lookup"><span data-stu-id="50582-166">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="50582-167">Další informace o konkrétní elementy XML lze nalézt v [úložiště klíčů formátovat dokument](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="50582-167">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="50582-168">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="50582-168">IXmlRepository</span></span>

<span data-ttu-id="50582-169">`IXmlRepository` Rozhraní představuje typ, který můžete zachovat XML tak, aby a načtení XML ze záložnímu úložišti.</span><span class="sxs-lookup"><span data-stu-id="50582-169">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="50582-170">Poskytuje dvě rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="50582-170">It exposes two APIs:</span></span>

* <span data-ttu-id="50582-171">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="50582-171">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="50582-172">StoreElement (XElement element, friendlyName řetězec)</span><span class="sxs-lookup"><span data-stu-id="50582-172">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="50582-173">Implementace `IXmlRepository` nemusíte analyzovat soubor XML prošla je.</span><span class="sxs-lookup"><span data-stu-id="50582-173">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="50582-174">Jejich považovat za neprůhledné dokumentů XML a nechat vyšší vrstvy starat o generování a analýza dokumenty.</span><span class="sxs-lookup"><span data-stu-id="50582-174">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="50582-175">Existují dva předdefinované konkrétní typy, které implementují třídu `IXmlRepository`: `FileSystemXmlRepository` a `RegistryXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="50582-175">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="50582-176">Najdete v článku [dokumentu zprostředkovatele úložiště klíčů](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) Další informace.</span><span class="sxs-lookup"><span data-stu-id="50582-176">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="50582-177">Vlastní registrace `IXmlRepository` může být vhodným způsobem použít záložní úložiště, například Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="50582-177">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="50582-178">Chcete-li změnit výchozí úložiště celou aplikaci, zaregistrovat vlastní `IXmlRepository` instance:</span><span class="sxs-lookup"><span data-stu-id="50582-178">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50582-179">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="50582-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50582-180">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="50582-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="50582-181">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="50582-181">IXmlEncryptor</span></span>

<span data-ttu-id="50582-182">`IXmlEncryptor` Rozhraní představuje typ, který můžete šifrovat element XML ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="50582-182">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="50582-183">Zpřístupňuje jediného rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="50582-183">It exposes a single API:</span></span>

* <span data-ttu-id="50582-184">Šifrování (XElement plaintextElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="50582-184">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="50582-185">Pokud serializovaný seznam `IAuthenticatedEncryptorDescriptor` obsahuje všechny elementy označena jako "vyžaduje šifrování", pak `XmlKeyManager` tyto prvky se spustí pomocí konfigurovaného `IXmlEncryptor`na `Encrypt` metoda ale zachová enciphered element místo ve formátu prostého textu elementu na `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="50582-185">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="50582-186">Výstup `Encrypt` je metoda `EncryptedXmlInfo` objektu.</span><span class="sxs-lookup"><span data-stu-id="50582-186">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="50582-187">Tento objekt je obálky, který obsahuje výsledné enciphered `XElement` a typ, který představuje `IXmlDecryptor` které můžete používat k dešifrování odpovídající element.</span><span class="sxs-lookup"><span data-stu-id="50582-187">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="50582-188">Existují čtyři integrované konkrétní typy, které implementují třídu `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="50582-188">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="50582-189">Najdete v článku [klíče šifrování v dokumentu rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) Další informace.</span><span class="sxs-lookup"><span data-stu-id="50582-189">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="50582-190">Chcete-li změnit výchozí klíč šifrování na rest mechanismus celou aplikaci, zaregistrovat vlastní `IXmlEncryptor` instance:</span><span class="sxs-lookup"><span data-stu-id="50582-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50582-191">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="50582-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50582-192">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="50582-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="50582-193">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="50582-193">IXmlDecryptor</span></span>

<span data-ttu-id="50582-194">`IXmlDecryptor` Rozhraní představuje typ, který umí k dešifrování `XElement` , byl enciphered prostřednictvím `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="50582-194">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="50582-195">Zpřístupňuje jediného rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="50582-195">It exposes a single API:</span></span>

* <span data-ttu-id="50582-196">Dešifrování (XElement encryptedElement): XElement</span><span class="sxs-lookup"><span data-stu-id="50582-196">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="50582-197">`Decrypt` Metoda vrátí zpět provádí šifrování `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="50582-197">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="50582-198">Obecně platí, každý konkrétní `IXmlEncryptor` implementace bude mít odpovídající konkrétní `IXmlDecryptor` implementace.</span><span class="sxs-lookup"><span data-stu-id="50582-198">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="50582-199">Typy, které implementují rozhraní `IXmlDecryptor` musí mít jednu z následujících dvou veřejné konstruktory:</span><span class="sxs-lookup"><span data-stu-id="50582-199">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="50582-200">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="50582-200">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="50582-201">.ctor()</span><span class="sxs-lookup"><span data-stu-id="50582-201">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="50582-202">`IServiceProvider` Předaný konstruktoru může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="50582-202">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="50582-203">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="50582-203">IKeyEscrowSink</span></span>

<span data-ttu-id="50582-204">`IKeyEscrowSink` Rozhraní představuje typ, který může provádět úschově citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="50582-204">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="50582-205">Odvolat serializované popisovače můžou obsahovat citlivé informace (například kryptografické materiálů), a to je co vedla k uvedení [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) zadejte na prvním místě.</span><span class="sxs-lookup"><span data-stu-id="50582-205">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="50582-206">Ale dojít kvůli nehodám a klíč okruhy mohl být odstraněn, nebo dojde k poškození.</span><span class="sxs-lookup"><span data-stu-id="50582-206">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="50582-207">Poskytuje rozhraní úschově šrafování nouzový řídicí povolení přístupu k nezpracované serializovaných XML předtím, než je transformovat žádné nakonfigurované [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="50582-207">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="50582-208">Rozhraní zpřístupní jediného rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="50582-208">The interface exposes a single API:</span></span>

* <span data-ttu-id="50582-209">Úložiště (identifikátor Guid keyId, XElement element)</span><span class="sxs-lookup"><span data-stu-id="50582-209">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="50582-210">Je až `IKeyEscrowSink` implementace zpracování zadaný element zabezpečeným způsobem konzistentní s firemní zásady.</span><span class="sxs-lookup"><span data-stu-id="50582-210">It is up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="50582-211">Jednou z možných implementací může být pro sink úschově k zašifrování elementu XML pomocí známých podnikové certifikátu X.509, kde má byla uloží privátní klíč certifikátu; `CertificateXmlEncryptor` typ s tím pomůže.</span><span class="sxs-lookup"><span data-stu-id="50582-211">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="50582-212">`IKeyEscrowSink` Implementace zodpovídá taky za uložením zadaný element správně.</span><span class="sxs-lookup"><span data-stu-id="50582-212">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="50582-213">Ve výchozím nastavení je aktivován žádný mechanismus úschově, když správci serveru můžete [nakonfigurujte tato globální](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="50582-213">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="50582-214">Můžete také nastavit prostřednictvím kódu programu prostřednictvím `IDataProtectionBuilder.AddKeyEscrowSink` metoda, jak je znázorněno v následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="50582-214">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="50582-215">`AddKeyEscrowSink` Zrcadlení přetížení metody `IServiceCollection.AddSingleton` a `IServiceCollection.AddInstance` přetížení, jako `IKeyEscrowSink` instance by měla být jednotlivé prvky.</span><span class="sxs-lookup"><span data-stu-id="50582-215">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="50582-216">Pokud je to více `IKeyEscrowSink` instance jsou zaregistrovány, každé z nich bude volána při generování klíče, klíče může být uloží je na více mechanismů současně.</span><span class="sxs-lookup"><span data-stu-id="50582-216">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="50582-217">Neexistuje žádné rozhraní API pro čtení materiálu z `IKeyEscrowSink` instance.</span><span class="sxs-lookup"><span data-stu-id="50582-217">There is no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="50582-218">To je konzistentní s teoreticky návrhu mechanismu úschově: Tato má zpřístupněte materiál klíče pro důvěryhodnou autoritou, a vzhledem k tomu, že aplikace není samotné důvěryhodnou autoritou, by neměl mít přístup k vlastní escrowed materiálů.</span><span class="sxs-lookup"><span data-stu-id="50582-218">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="50582-219">Následující vzorový kód ukazuje vytvoření a registrace `IKeyEscrowSink` kde klíče se uloží tak, že je můžete obnovit pouze členové "CONTOSODomain Admins".</span><span class="sxs-lookup"><span data-stu-id="50582-219">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="50582-220">Chcete-li tuto ukázku spustit, musíte být v doméně systému Windows 8 / Windows Server 2012 počítače a řadič domény musí být Windows Server 2012 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="50582-220">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
