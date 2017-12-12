---
title: "Rozšiřitelnost kryptografie jádra"
author: rick-anderson
description: "Popisuje IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer a objektu pro vytváření nejvyšší úrovně."
keywords: ASP.NET Core, IAuthenticatedEncryptorDescriptorDeserializer IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor,
ms.author: riande
manager: wpickett
ms.date: 8/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: 69839562c39ab83531085e20dac1bd56e8d13d3f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="core-cryptography-extensibility"></a><span data-ttu-id="39783-104">Rozšiřitelnost kryptografie jádra</span><span class="sxs-lookup"><span data-stu-id="39783-104">Core cryptography extensibility</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="39783-105">Typy, které implementují některá z následujících rozhraní by měly být vláken pro více volající.</span><span class="sxs-lookup"><span data-stu-id="39783-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="39783-106">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="39783-106">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="39783-107">**IAuthenticatedEncryptor** rozhraní je základním stavebním blokem šifrovací subsystém.</span><span class="sxs-lookup"><span data-stu-id="39783-107">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="39783-108">Je obecně jeden IAuthenticatedEncryptor na klíč, a IAuthenticatedEncryptor instance zabalí všechny kryptografické materiál klíče a algoritmické informace potřebné k provedení kryptografické operace.</span><span class="sxs-lookup"><span data-stu-id="39783-108">There is generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="39783-109">Jak naznačuje název, typ zodpovídá za poskytování ověřené šifrovacích a dešifrovacích služeb.</span><span class="sxs-lookup"><span data-stu-id="39783-109">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="39783-110">Zpřístupňuje následující dvě rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="39783-110">It exposes the following two APIs.</span></span>

* <span data-ttu-id="39783-111">Dešifrování (ArraySegment<byte> ciphertext ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="39783-111">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

* <span data-ttu-id="39783-112">Šifrování (ArraySegment<byte> ve formátu prostého textu, ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="39783-112">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

<span data-ttu-id="39783-113">Metoda šifrování vrátí objekt blob, který zahrnuje enciphered jako prostý text a značku ověřování.</span><span class="sxs-lookup"><span data-stu-id="39783-113">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="39783-114">Ověřovací značky musí zahrnovat další ověřených dat (AAD), i když AAD samotné nemusí být použitelná pro obnovení z poslední datová část.</span><span class="sxs-lookup"><span data-stu-id="39783-114">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="39783-115">Metoda dešifrování ověří značce ověřování a vrátí deciphered datové části.</span><span class="sxs-lookup"><span data-stu-id="39783-115">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="39783-116">Všechny chyby (s výjimkou ArgumentNullException a podobné) musí být homogenizovány k cryptographicexception –.</span><span class="sxs-lookup"><span data-stu-id="39783-116">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="39783-117">Samotná instance IAuthenticatedEncryptor nemusí ve skutečnosti obsahovat materiál klíče.</span><span class="sxs-lookup"><span data-stu-id="39783-117">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="39783-118">Implementace může například delegovat na modulu hardwarového zabezpečení pro všechny operace.</span><span class="sxs-lookup"><span data-stu-id="39783-118">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="39783-119">Postup vytvoření IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="39783-119">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="39783-120">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="39783-120">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="39783-121">**IAuthenticatedEncryptorFactory** rozhraní představuje typ, který umí vytvořit [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span><span class="sxs-lookup"><span data-stu-id="39783-121">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="39783-122">Jejího rozhraní API je následující.</span><span class="sxs-lookup"><span data-stu-id="39783-122">Its API is as follows.</span></span>

* <span data-ttu-id="39783-123">CreateEncryptorInstance (IKey klíč): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="39783-123">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="39783-124">Pro všechny instance daného IKey všechny ověřené encryptors vytvářené metodou jeho CreateEncryptorInstance by měl být považovány za ekvivalentní, jako v následující ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="39783-124">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="39783-125">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="39783-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="39783-126">**IAuthenticatedEncryptorDescriptor** rozhraní představuje typ, který umí vytvořit [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span><span class="sxs-lookup"><span data-stu-id="39783-126">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="39783-127">Jejího rozhraní API je následující.</span><span class="sxs-lookup"><span data-stu-id="39783-127">Its API is as follows.</span></span>

* <span data-ttu-id="39783-128">CreateEncryptorInstance(): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="39783-128">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="39783-129">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="39783-129">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="39783-130">Jako IAuthenticatedEncryptor je za instance IAuthenticatedEncryptorDescriptor zabalení jeden konkrétní klíč.</span><span class="sxs-lookup"><span data-stu-id="39783-130">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="39783-131">To znamená, že pro všechny instance daného IAuthenticatedEncryptorDescriptor všechny ověřené encryptors vytvářené metodou jeho CreateEncryptorInstance by měl být považovány za ekvivalentní, jako v následující ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="39783-131">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="39783-132">IAuthenticatedEncryptorDescriptor (2.x pouze pro základní technologie ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="39783-132">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="39783-133">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="39783-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="39783-134">**IAuthenticatedEncryptorDescriptor** rozhraní představuje typ, který umí samotné exportovat do formátu XML.</span><span class="sxs-lookup"><span data-stu-id="39783-134">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="39783-135">Jejího rozhraní API je následující.</span><span class="sxs-lookup"><span data-stu-id="39783-135">Its API is as follows.</span></span>

* <span data-ttu-id="39783-136">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="39783-136">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="39783-137">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="39783-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="39783-138">Serializace XML</span><span class="sxs-lookup"><span data-stu-id="39783-138">XML Serialization</span></span>

<span data-ttu-id="39783-139">Hlavní rozdíl mezi IAuthenticatedEncryptor a IAuthenticatedEncryptorDescriptor je, že popisovač umí vytvořit modulu pro šifrování a poskytne mu platnými argumenty.</span><span class="sxs-lookup"><span data-stu-id="39783-139">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="39783-140">Vezměte v úvahu IAuthenticatedEncryptor, jejichž provádění spoléhá na SymmetricAlgorithm a KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="39783-140">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="39783-141">Modulu pro šifrování je úloha je používat tyto typy, ale jeho nemusí nutně vědět, kde tyto typy pochází, takže ji nelze vypsat skutečně správné popis toho, jak znovu sám sebe, pokud se aplikace restartuje.</span><span class="sxs-lookup"><span data-stu-id="39783-141">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="39783-142">Popisovač funguje jako vyšší úroveň nad to.</span><span class="sxs-lookup"><span data-stu-id="39783-142">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="39783-143">Vzhledem k tomu, že popisovač umí vytvořit instanci modulu pro šifrování (například ho umí vytvořit požadované algoritmy), tak, aby po resetování aplikace nelze vytvořit instanci modulu pro šifrování se může serializovat dané znalosti ve formátu XML.</span><span class="sxs-lookup"><span data-stu-id="39783-143">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="39783-144">Popisovač lze serializovat přes její rutiny ExportToXml.</span><span class="sxs-lookup"><span data-stu-id="39783-144">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="39783-145">Tato rutina vrátí XmlSerializedDescriptorInfo, který obsahuje dvě vlastnosti: reprezentace XElement popisovač a typ, který představuje [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) může být použít tento popisovač zadaný odpovídající XElement resurrect.</span><span class="sxs-lookup"><span data-stu-id="39783-145">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="39783-146">Serializované popisovače mohou obsahovat citlivé informace, jako je například materiál kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="39783-146">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="39783-147">Systém ochrany dat má integrovanou podporu pro šifrování informací před obsahuje trvalé do úložiště.</span><span class="sxs-lookup"><span data-stu-id="39783-147">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="39783-148">Abyste mohli využívat tohoto objektu, měli označit popisovač element, který obsahuje citlivé informace s název "requiresEncryption" (xmlns "http://schemas.asp.net/2015/03/dataProtection"), hodnotu "true".</span><span class="sxs-lookup"><span data-stu-id="39783-148">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "http://schemas.asp.net/2015/03/dataProtection"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="39783-149">Pro nastavení tohoto atributu je pomocná rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="39783-149">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="39783-150">Volání metody rozšíření, které XElement.MarkAsRequiresEncryption() nachází v oboru názvů Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span><span class="sxs-lookup"><span data-stu-id="39783-150">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="39783-151">Také může být případy, kdy serializované popisovače neobsahuje citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="39783-151">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="39783-152">Zvažte znovu případ kryptografického klíče uložené v modulu hardwarového zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="39783-152">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="39783-153">Popisovač nelze vypsat materiál klíče při serializaci samotné vzhledem k tomu, že modul hardwarového zabezpečení nebude vystavit materiály v podobě prostého textu.</span><span class="sxs-lookup"><span data-stu-id="39783-153">The descriptor cannot write out the key material when serializing itself since the HSM will not expose the material in plaintext form.</span></span> <span data-ttu-id="39783-154">Místo toho může zapsat popisovač zabalené klíč verze klíč (Pokud je modul hardwarového zabezpečení umožňuje export tímto způsobem) nebo HSM pro vlastní jedinečný identifikátor pro klíč.</span><span class="sxs-lookup"><span data-stu-id="39783-154">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="39783-155">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="39783-155">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="39783-156">**IAuthenticatedEncryptorDescriptorDeserializer** rozhraní představuje typ, který umí k deserializaci instance IAuthenticatedEncryptorDescriptor z XElement.</span><span class="sxs-lookup"><span data-stu-id="39783-156">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="39783-157">Zpřístupňuje jedné metody:</span><span class="sxs-lookup"><span data-stu-id="39783-157">It exposes a single method:</span></span>

* <span data-ttu-id="39783-158">ImportFromXml (XElement element): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="39783-158">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="39783-159">Metoda ImportFromXml přijímá XElement, který vrátila [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) a vytvoří ekvivalentní původní IAuthenticatedEncryptorDescriptor.</span><span class="sxs-lookup"><span data-stu-id="39783-159">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="39783-160">Typy, které implementují třídu IAuthenticatedEncryptorDescriptorDeserializer musí mít jednu z následujících dvou veřejné konstruktory:</span><span class="sxs-lookup"><span data-stu-id="39783-160">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="39783-161">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="39783-161">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="39783-162">.ctor()</span><span class="sxs-lookup"><span data-stu-id="39783-162">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="39783-163">IServiceProvider předaný konstruktoru může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="39783-163">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="39783-164">Objekt factory nejvyšší úrovně</span><span class="sxs-lookup"><span data-stu-id="39783-164">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="39783-165">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="39783-165">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="39783-166">**AlgorithmConfiguration** třída reprezentuje typ, který umí vytvořit [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instance.</span><span class="sxs-lookup"><span data-stu-id="39783-166">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="39783-167">Zpřístupňuje jediného rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="39783-167">It exposes a single API.</span></span>

* <span data-ttu-id="39783-168">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="39783-168">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="39783-169">AlgorithmConfiguration si můžete představit jako objekt pro vytváření nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="39783-169">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="39783-170">Konfigurace slouží jako šablona.</span><span class="sxs-lookup"><span data-stu-id="39783-170">The configuration serves as a template.</span></span> <span data-ttu-id="39783-171">Ho zabalí algoritmické informace (například tuto konfiguraci vytváří popisovače s klíčem standardu AES-128-GCM hlavní), ale ještě není přidružen konkrétní klíč.</span><span class="sxs-lookup"><span data-stu-id="39783-171">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it is not yet associated with a specific key.</span></span>

<span data-ttu-id="39783-172">Když je volána CreateNewDescriptor, výhradně pro toto volání se vytvoří nový materiál klíče a vytváří nové IAuthenticatedEncryptorDescriptor který zabalí tuto materiál klíče a algoritmické informace požadované pro využívat materiál.</span><span class="sxs-lookup"><span data-stu-id="39783-172">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="39783-173">Materiál klíče může vytvořit v softwaru (a uchovávat v paměti), může být vytvořen a uchovávat v modulu hardwarového zabezpečení a tak dále.</span><span class="sxs-lookup"><span data-stu-id="39783-173">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="39783-174">Je velmi důležitý bod je, že jakékoli dvě volání CreateNewDescriptor by nikdy vytvořit ekvivalentní IAuthenticatedEncryptorDescriptor instance.</span><span class="sxs-lookup"><span data-stu-id="39783-174">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="39783-175">Typ AlgorithmConfiguration slouží jako vstupní bod pro vytvoření klíče rutiny jako [automatické klíč vrácení](../implementation/key-management.md#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="39783-175">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](../implementation/key-management.md#key-expiration-and-rolling).</span></span> <span data-ttu-id="39783-176">Chcete-li změnit implementaci pro všechny budoucí klíče, nastavte vlastnost AuthenticatedEncryptorConfiguration v KeyManagementOptions.</span><span class="sxs-lookup"><span data-stu-id="39783-176">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="39783-177">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="39783-177">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="39783-178">**IAuthenticatedEncryptorConfiguration** rozhraní představuje typ, který umí vytvořit [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instance.</span><span class="sxs-lookup"><span data-stu-id="39783-178">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="39783-179">Zpřístupňuje jediného rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="39783-179">It exposes a single API.</span></span>

* <span data-ttu-id="39783-180">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="39783-180">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="39783-181">IAuthenticatedEncryptorConfiguration si můžete představit jako objekt pro vytváření nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="39783-181">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="39783-182">Konfigurace slouží jako šablona.</span><span class="sxs-lookup"><span data-stu-id="39783-182">The configuration serves as a template.</span></span> <span data-ttu-id="39783-183">Ho zabalí algoritmické informace (například tuto konfiguraci vytváří popisovače s klíčem standardu AES-128-GCM hlavní), ale ještě není přidružen konkrétní klíč.</span><span class="sxs-lookup"><span data-stu-id="39783-183">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it is not yet associated with a specific key.</span></span>

<span data-ttu-id="39783-184">Když je volána CreateNewDescriptor, výhradně pro toto volání se vytvoří nový materiál klíče a vytváří nové IAuthenticatedEncryptorDescriptor který zabalí tuto materiál klíče a algoritmické informace požadované pro využívat materiál.</span><span class="sxs-lookup"><span data-stu-id="39783-184">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="39783-185">Materiál klíče může vytvořit v softwaru (a uchovávat v paměti), může být vytvořen a uchovávat v modulu hardwarového zabezpečení a tak dále.</span><span class="sxs-lookup"><span data-stu-id="39783-185">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="39783-186">Je velmi důležitý bod je, že jakékoli dvě volání CreateNewDescriptor by nikdy vytvořit ekvivalentní IAuthenticatedEncryptorDescriptor instance.</span><span class="sxs-lookup"><span data-stu-id="39783-186">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="39783-187">Typ IAuthenticatedEncryptorConfiguration slouží jako vstupní bod pro vytvoření klíče rutiny jako [automatické klíč vrácení](../implementation/key-management.md#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="39783-187">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](../implementation/key-management.md#key-expiration-and-rolling).</span></span> <span data-ttu-id="39783-188">Chcete-li změnit implementaci pro všechny budoucí klíče, zaregistrujte typu singleton IAuthenticatedEncryptorConfiguration v kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="39783-188">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
