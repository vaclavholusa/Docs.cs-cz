---
title: "Rozšiřitelnost kryptografie jádra"
author: rick-anderson
description: "Popisuje IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer a objektu pro vytváření nejvyšší úrovně."
ms.author: riande
manager: wpickett
ms.date: 8/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: b82c30fe40c4badc74645dafa9f0d13f6ffae031
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="core-cryptography-extensibility"></a><span data-ttu-id="a5524-103">Rozšiřitelnost kryptografie jádra</span><span class="sxs-lookup"><span data-stu-id="a5524-103">Core cryptography extensibility</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="a5524-104">Typy, které implementují některá z následujících rozhraní by měly být vláken pro více volající.</span><span class="sxs-lookup"><span data-stu-id="a5524-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="a5524-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="a5524-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="a5524-106">**IAuthenticatedEncryptor** rozhraní je základním stavebním blokem šifrovací subsystém.</span><span class="sxs-lookup"><span data-stu-id="a5524-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="a5524-107">Je obecně jeden IAuthenticatedEncryptor na klíč, a IAuthenticatedEncryptor instance zabalí všechny kryptografické materiál klíče a algoritmické informace potřebné k provedení kryptografické operace.</span><span class="sxs-lookup"><span data-stu-id="a5524-107">There is generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="a5524-108">Jak naznačuje název, typ zodpovídá za poskytování ověřené šifrovacích a dešifrovacích služeb.</span><span class="sxs-lookup"><span data-stu-id="a5524-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="a5524-109">Zpřístupňuje následující dvě rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a5524-109">It exposes the following two APIs.</span></span>

* <span data-ttu-id="a5524-110">Dešifrování (ArraySegment<byte> ciphertext ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="a5524-110">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

* <span data-ttu-id="a5524-111">Šifrování (ArraySegment<byte> ve formátu prostého textu, ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="a5524-111">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

<span data-ttu-id="a5524-112">Metoda šifrování vrátí objekt blob, který zahrnuje enciphered jako prostý text a značku ověřování.</span><span class="sxs-lookup"><span data-stu-id="a5524-112">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="a5524-113">Ověřovací značky musí zahrnovat další ověřených dat (AAD), i když AAD samotné nemusí být použitelná pro obnovení z poslední datová část.</span><span class="sxs-lookup"><span data-stu-id="a5524-113">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="a5524-114">Metoda dešifrování ověří značce ověřování a vrátí deciphered datové části.</span><span class="sxs-lookup"><span data-stu-id="a5524-114">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="a5524-115">Všechny chyby (s výjimkou ArgumentNullException a podobné) musí být homogenizovány k cryptographicexception –.</span><span class="sxs-lookup"><span data-stu-id="a5524-115">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="a5524-116">Samotná instance IAuthenticatedEncryptor nemusí ve skutečnosti obsahovat materiál klíče.</span><span class="sxs-lookup"><span data-stu-id="a5524-116">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="a5524-117">Implementace může například delegovat na modulu hardwarového zabezpečení pro všechny operace.</span><span class="sxs-lookup"><span data-stu-id="a5524-117">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="a5524-118">Postup vytvoření IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="a5524-118">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5524-119">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="a5524-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a5524-120">**IAuthenticatedEncryptorFactory** rozhraní představuje typ, který umí vytvořit [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span><span class="sxs-lookup"><span data-stu-id="a5524-120">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="a5524-121">Jejího rozhraní API je následující.</span><span class="sxs-lookup"><span data-stu-id="a5524-121">Its API is as follows.</span></span>

* <span data-ttu-id="a5524-122">CreateEncryptorInstance (IKey klíč): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="a5524-122">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="a5524-123">Pro všechny instance daného IKey všechny ověřené encryptors vytvářené metodou jeho CreateEncryptorInstance by měl být považovány za ekvivalentní, jako v následující ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="a5524-123">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5524-124">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="a5524-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a5524-125">**IAuthenticatedEncryptorDescriptor** rozhraní představuje typ, který umí vytvořit [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span><span class="sxs-lookup"><span data-stu-id="a5524-125">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="a5524-126">Jejího rozhraní API je následující.</span><span class="sxs-lookup"><span data-stu-id="a5524-126">Its API is as follows.</span></span>

* <span data-ttu-id="a5524-127">CreateEncryptorInstance(): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="a5524-127">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="a5524-128">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="a5524-128">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="a5524-129">Jako IAuthenticatedEncryptor je za instance IAuthenticatedEncryptorDescriptor zabalení jeden konkrétní klíč.</span><span class="sxs-lookup"><span data-stu-id="a5524-129">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="a5524-130">To znamená, že pro všechny instance daného IAuthenticatedEncryptorDescriptor všechny ověřené encryptors vytvářené metodou jeho CreateEncryptorInstance by měl být považovány za ekvivalentní, jako v následující ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="a5524-130">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="a5524-131">IAuthenticatedEncryptorDescriptor (2.x pouze pro základní technologie ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="a5524-131">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5524-132">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="a5524-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a5524-133">**IAuthenticatedEncryptorDescriptor** rozhraní představuje typ, který umí samotné exportovat do formátu XML.</span><span class="sxs-lookup"><span data-stu-id="a5524-133">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="a5524-134">Jejího rozhraní API je následující.</span><span class="sxs-lookup"><span data-stu-id="a5524-134">Its API is as follows.</span></span>

* <span data-ttu-id="a5524-135">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="a5524-135">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5524-136">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="a5524-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="a5524-137">Serializace XML</span><span class="sxs-lookup"><span data-stu-id="a5524-137">XML Serialization</span></span>

<span data-ttu-id="a5524-138">Hlavní rozdíl mezi IAuthenticatedEncryptor a IAuthenticatedEncryptorDescriptor je, že popisovač umí vytvořit modulu pro šifrování a poskytne mu platnými argumenty.</span><span class="sxs-lookup"><span data-stu-id="a5524-138">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="a5524-139">Vezměte v úvahu IAuthenticatedEncryptor, jejichž provádění spoléhá na SymmetricAlgorithm a KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="a5524-139">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="a5524-140">Modulu pro šifrování je úloha je používat tyto typy, ale jeho nemusí nutně vědět, kde tyto typy pochází, takže ji nelze vypsat skutečně správné popis toho, jak znovu sám sebe, pokud se aplikace restartuje.</span><span class="sxs-lookup"><span data-stu-id="a5524-140">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="a5524-141">Popisovač funguje jako vyšší úroveň nad to.</span><span class="sxs-lookup"><span data-stu-id="a5524-141">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="a5524-142">Vzhledem k tomu, že popisovač umí vytvořit instanci modulu pro šifrování (například ho umí vytvořit požadované algoritmy), tak, aby po resetování aplikace nelze vytvořit instanci modulu pro šifrování se může serializovat dané znalosti ve formátu XML.</span><span class="sxs-lookup"><span data-stu-id="a5524-142">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="a5524-143">Popisovač lze serializovat přes její rutiny ExportToXml.</span><span class="sxs-lookup"><span data-stu-id="a5524-143">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="a5524-144">Tato rutina vrátí XmlSerializedDescriptorInfo, který obsahuje dvě vlastnosti: reprezentace XElement popisovač a typ, který představuje [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) může být použít tento popisovač zadaný odpovídající XElement resurrect.</span><span class="sxs-lookup"><span data-stu-id="a5524-144">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="a5524-145">Serializované popisovače mohou obsahovat citlivé informace, jako je například materiál kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="a5524-145">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="a5524-146">Systém ochrany dat má integrovanou podporu pro šifrování informací před obsahuje trvalé do úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5524-146">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="a5524-147">Abyste mohli využívat tohoto objektu, měli označit popisovač element, který obsahuje citlivé informace s název "requiresEncryption" (xmlns "http://schemas.asp.net/2015/03/dataProtection"), hodnotu "true".</span><span class="sxs-lookup"><span data-stu-id="a5524-147">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "http://schemas.asp.net/2015/03/dataProtection"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="a5524-148">Pro nastavení tohoto atributu je pomocná rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a5524-148">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="a5524-149">Volání metody rozšíření, které XElement.MarkAsRequiresEncryption() nachází v oboru názvů Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span><span class="sxs-lookup"><span data-stu-id="a5524-149">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="a5524-150">Také může být případy, kdy serializované popisovače neobsahuje citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="a5524-150">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="a5524-151">Zvažte znovu případ kryptografického klíče uložené v modulu hardwarového zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="a5524-151">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="a5524-152">Popisovač nelze vypsat materiál klíče při serializaci samotné vzhledem k tomu, že modul hardwarového zabezpečení nebude vystavit materiály v podobě prostého textu.</span><span class="sxs-lookup"><span data-stu-id="a5524-152">The descriptor cannot write out the key material when serializing itself since the HSM will not expose the material in plaintext form.</span></span> <span data-ttu-id="a5524-153">Místo toho může zapsat popisovač zabalené klíč verze klíč (Pokud je modul hardwarového zabezpečení umožňuje export tímto způsobem) nebo HSM pro vlastní jedinečný identifikátor pro klíč.</span><span class="sxs-lookup"><span data-stu-id="a5524-153">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="a5524-154">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="a5524-154">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="a5524-155">**IAuthenticatedEncryptorDescriptorDeserializer** rozhraní představuje typ, který umí k deserializaci instance IAuthenticatedEncryptorDescriptor z XElement.</span><span class="sxs-lookup"><span data-stu-id="a5524-155">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="a5524-156">Zpřístupňuje jedné metody:</span><span class="sxs-lookup"><span data-stu-id="a5524-156">It exposes a single method:</span></span>

* <span data-ttu-id="a5524-157">ImportFromXml (XElement element): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="a5524-157">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="a5524-158">Metoda ImportFromXml přijímá XElement, který vrátila [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) a vytvoří ekvivalentní původní IAuthenticatedEncryptorDescriptor.</span><span class="sxs-lookup"><span data-stu-id="a5524-158">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="a5524-159">Typy, které implementují třídu IAuthenticatedEncryptorDescriptorDeserializer musí mít jednu z následujících dvou veřejné konstruktory:</span><span class="sxs-lookup"><span data-stu-id="a5524-159">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="a5524-160">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="a5524-160">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="a5524-161">.ctor()</span><span class="sxs-lookup"><span data-stu-id="a5524-161">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="a5524-162">IServiceProvider předaný konstruktoru může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="a5524-162">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="a5524-163">Objekt factory nejvyšší úrovně</span><span class="sxs-lookup"><span data-stu-id="a5524-163">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5524-164">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="a5524-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a5524-165">**AlgorithmConfiguration** třída reprezentuje typ, který umí vytvořit [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instance.</span><span class="sxs-lookup"><span data-stu-id="a5524-165">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="a5524-166">Zpřístupňuje jediného rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a5524-166">It exposes a single API.</span></span>

* <span data-ttu-id="a5524-167">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="a5524-167">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="a5524-168">AlgorithmConfiguration si můžete představit jako objekt pro vytváření nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="a5524-168">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="a5524-169">Konfigurace slouží jako šablona.</span><span class="sxs-lookup"><span data-stu-id="a5524-169">The configuration serves as a template.</span></span> <span data-ttu-id="a5524-170">Ho zabalí algoritmické informace (například tuto konfiguraci vytváří popisovače s klíčem standardu AES-128-GCM hlavní), ale ještě není přidružen konkrétní klíč.</span><span class="sxs-lookup"><span data-stu-id="a5524-170">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it is not yet associated with a specific key.</span></span>

<span data-ttu-id="a5524-171">Když je volána CreateNewDescriptor, výhradně pro toto volání se vytvoří nový materiál klíče a vytváří nové IAuthenticatedEncryptorDescriptor který zabalí tuto materiál klíče a algoritmické informace požadované pro využívat materiál.</span><span class="sxs-lookup"><span data-stu-id="a5524-171">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="a5524-172">Materiál klíče může vytvořit v softwaru (a uchovávat v paměti), může být vytvořen a uchovávat v modulu hardwarového zabezpečení a tak dále.</span><span class="sxs-lookup"><span data-stu-id="a5524-172">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="a5524-173">Je velmi důležitý bod je, že jakékoli dvě volání CreateNewDescriptor by nikdy vytvořit ekvivalentní IAuthenticatedEncryptorDescriptor instance.</span><span class="sxs-lookup"><span data-stu-id="a5524-173">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="a5524-174">Typ AlgorithmConfiguration slouží jako vstupní bod pro vytvoření klíče rutiny jako [automatické klíč vrácení](../implementation/key-management.md#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="a5524-174">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](../implementation/key-management.md#key-expiration-and-rolling).</span></span> <span data-ttu-id="a5524-175">Chcete-li změnit implementaci pro všechny budoucí klíče, nastavte vlastnost AuthenticatedEncryptorConfiguration v KeyManagementOptions.</span><span class="sxs-lookup"><span data-stu-id="a5524-175">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5524-176">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="a5524-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a5524-177">**IAuthenticatedEncryptorConfiguration** rozhraní představuje typ, který umí vytvořit [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instance.</span><span class="sxs-lookup"><span data-stu-id="a5524-177">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="a5524-178">Zpřístupňuje jediného rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a5524-178">It exposes a single API.</span></span>

* <span data-ttu-id="a5524-179">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="a5524-179">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="a5524-180">IAuthenticatedEncryptorConfiguration si můžete představit jako objekt pro vytváření nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="a5524-180">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="a5524-181">Konfigurace slouží jako šablona.</span><span class="sxs-lookup"><span data-stu-id="a5524-181">The configuration serves as a template.</span></span> <span data-ttu-id="a5524-182">Ho zabalí algoritmické informace (například tuto konfiguraci vytváří popisovače s klíčem standardu AES-128-GCM hlavní), ale ještě není přidružen konkrétní klíč.</span><span class="sxs-lookup"><span data-stu-id="a5524-182">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it is not yet associated with a specific key.</span></span>

<span data-ttu-id="a5524-183">Když je volána CreateNewDescriptor, výhradně pro toto volání se vytvoří nový materiál klíče a vytváří nové IAuthenticatedEncryptorDescriptor který zabalí tuto materiál klíče a algoritmické informace požadované pro využívat materiál.</span><span class="sxs-lookup"><span data-stu-id="a5524-183">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="a5524-184">Materiál klíče může vytvořit v softwaru (a uchovávat v paměti), může být vytvořen a uchovávat v modulu hardwarového zabezpečení a tak dále.</span><span class="sxs-lookup"><span data-stu-id="a5524-184">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="a5524-185">Je velmi důležitý bod je, že jakékoli dvě volání CreateNewDescriptor by nikdy vytvořit ekvivalentní IAuthenticatedEncryptorDescriptor instance.</span><span class="sxs-lookup"><span data-stu-id="a5524-185">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="a5524-186">Typ IAuthenticatedEncryptorConfiguration slouží jako vstupní bod pro vytvoření klíče rutiny jako [automatické klíč vrácení](../implementation/key-management.md#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="a5524-186">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](../implementation/key-management.md#key-expiration-and-rolling).</span></span> <span data-ttu-id="a5524-187">Chcete-li změnit implementaci pro všechny budoucí klíče, zaregistrujte typu singleton IAuthenticatedEncryptorConfiguration v kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="a5524-187">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
