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
ms.openlocfilehash: 8a3f4cf267998ddc7f393401059ca9d83ef2d8e7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="core-cryptography-extensibility"></a>Rozšiřitelnost kryptografie jádra

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> Typy, které implementují některá z následujících rozhraní by měly být vláken pro více volající.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

**IAuthenticatedEncryptor** rozhraní je základním stavebním blokem šifrovací subsystém. Je obecně jeden IAuthenticatedEncryptor na klíč, a IAuthenticatedEncryptor instance zabalí všechny kryptografické materiál klíče a algoritmické informace potřebné k provedení kryptografické operace.

Jak naznačuje název, typ zodpovídá za poskytování ověřené šifrovacích a dešifrovacích služeb. Zpřístupňuje následující dvě rozhraní API.

* Dešifrování (ArraySegment<byte> ciphertext ArraySegment<byte> additionalAuthenticatedData): byte]

* Šifrování (ArraySegment<byte> ve formátu prostého textu, ArraySegment<byte> additionalAuthenticatedData): byte]

Metoda šifrování vrátí objekt blob, který zahrnuje enciphered jako prostý text a značku ověřování. Ověřovací značky musí zahrnovat další ověřených dat (AAD), i když AAD samotné nemusí být použitelná pro obnovení z poslední datová část. Metoda dešifrování ověří značce ověřování a vrátí deciphered datové části. Všechny chyby (s výjimkou ArgumentNullException a podobné) musí být homogenizovány k cryptographicexception –.

> [!NOTE]
> Samotná instance IAuthenticatedEncryptor nemusí ve skutečnosti obsahovat materiál klíče. Implementace může například delegovat na modulu hardwarového zabezpečení pro všechny operace.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>Postup vytvoření IAuthenticatedEncryptor

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorFactory** rozhraní představuje typ, který umí vytvořit [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance. Jejího rozhraní API je následující.

* CreateEncryptorInstance (IKey klíč): IAuthenticatedEncryptor

Pro všechny instance daného IKey všechny ověřené encryptors vytvářené metodou jeho CreateEncryptorInstance by měl být považovány za ekvivalentní, jako v následující ukázka kódu.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorDescriptor** rozhraní představuje typ, který umí vytvořit [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance. Jejího rozhraní API je následující.

* CreateEncryptorInstance(): IAuthenticatedEncryptor

* ExportToXml(): XmlSerializedDescriptorInfo

Jako IAuthenticatedEncryptor je za instance IAuthenticatedEncryptorDescriptor zabalení jeden konkrétní klíč. To znamená, že pro všechny instance daného IAuthenticatedEncryptorDescriptor všechny ověřené encryptors vytvářené metodou jeho CreateEncryptorInstance by měl být považovány za ekvivalentní, jako v následující ukázka kódu.

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor (2.x pouze pro základní technologie ASP.NET)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorDescriptor** rozhraní představuje typ, který umí samotné exportovat do formátu XML. Jejího rozhraní API je následující.

* ExportToXml(): XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>Serializace XML

Hlavní rozdíl mezi IAuthenticatedEncryptor a IAuthenticatedEncryptorDescriptor je, že popisovač umí vytvořit modulu pro šifrování a poskytne mu platnými argumenty. Vezměte v úvahu IAuthenticatedEncryptor, jejichž provádění spoléhá na SymmetricAlgorithm a KeyedHashAlgorithm. Modulu pro šifrování je úloha je používat tyto typy, ale jeho nemusí nutně vědět, kde tyto typy pochází, takže ji nelze vypsat skutečně správné popis toho, jak znovu sám sebe, pokud se aplikace restartuje. Popisovač funguje jako vyšší úroveň nad to. Vzhledem k tomu, že popisovač umí vytvořit instanci modulu pro šifrování (například ho umí vytvořit požadované algoritmy), tak, aby po resetování aplikace nelze vytvořit instanci modulu pro šifrování se může serializovat dané znalosti ve formátu XML.

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

Popisovač lze serializovat přes její rutiny ExportToXml. Tato rutina vrátí XmlSerializedDescriptorInfo, který obsahuje dvě vlastnosti: reprezentace XElement popisovač a typ, který představuje [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) může být použít tento popisovač zadaný odpovídající XElement resurrect.

Serializované popisovače mohou obsahovat citlivé informace, jako je například materiál kryptografické klíče. Systém ochrany dat má integrovanou podporu pro šifrování informací před obsahuje trvalé do úložiště. Abyste mohli využívat tohoto objektu, měli označit popisovač element, který obsahuje citlivé informace s název "requiresEncryption" (xmlns "http://schemas.asp.net/2015/03/dataProtection"), hodnotu "true".

>[!TIP]
> Pro nastavení tohoto atributu je pomocná rozhraní API. Volání metody rozšíření, které XElement.MarkAsRequiresEncryption() nachází v oboru názvů Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.

Také může být případy, kdy serializované popisovače neobsahuje citlivé informace. Zvažte znovu případ kryptografického klíče uložené v modulu hardwarového zabezpečení. Popisovač nelze vypsat materiál klíče při serializaci samotné vzhledem k tomu, že modul hardwarového zabezpečení nebude vystavit materiály v podobě prostého textu. Místo toho může zapsat popisovač zabalené klíč verze klíč (Pokud je modul hardwarového zabezpečení umožňuje export tímto způsobem) nebo HSM pro vlastní jedinečný identifikátor pro klíč.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

**IAuthenticatedEncryptorDescriptorDeserializer** rozhraní představuje typ, který umí k deserializaci instance IAuthenticatedEncryptorDescriptor z XElement. Zpřístupňuje jedné metody:

* ImportFromXml (XElement element): IAuthenticatedEncryptorDescriptor

Metoda ImportFromXml přijímá XElement, který vrátila [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) a vytvoří ekvivalentní původní IAuthenticatedEncryptorDescriptor.

Typy, které implementují třídu IAuthenticatedEncryptorDescriptorDeserializer musí mít jednu z následujících dvou veřejné konstruktory:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> IServiceProvider předaný konstruktoru může mít hodnotu null.

## <a name="the-top-level-factory"></a>Objekt factory nejvyšší úrovně

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

**AlgorithmConfiguration** třída reprezentuje typ, který umí vytvořit [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instance. Zpřístupňuje jediného rozhraní API.

* CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor

AlgorithmConfiguration si můžete představit jako objekt pro vytváření nejvyšší úrovně. Konfigurace slouží jako šablona. Ho zabalí algoritmické informace (například tuto konfiguraci vytváří popisovače s klíčem standardu AES-128-GCM hlavní), ale ještě nebyla je spojen s konkrétní klíč.

Když je volána CreateNewDescriptor, výhradně pro toto volání se vytvoří nový materiál klíče a vytváří nové IAuthenticatedEncryptorDescriptor který zabalí tuto materiál klíče a algoritmické informace požadované pro využívat materiál. Materiál klíče může vytvořit v softwaru (a uchovávat v paměti), může být vytvořen a uchovávat v modulu hardwarového zabezpečení a tak dále. Je velmi důležitý bod je, že jakékoli dvě volání CreateNewDescriptor by nikdy vytvořit ekvivalentní IAuthenticatedEncryptorDescriptor instance.

Typ AlgorithmConfiguration slouží jako vstupní bod pro vytvoření klíče rutiny jako [automatické klíč vrácení](../implementation/key-management.md#key-expiration-and-rolling). Chcete-li změnit implementaci pro všechny budoucí klíče, nastavte vlastnost AuthenticatedEncryptorConfiguration v KeyManagementOptions.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorConfiguration** rozhraní představuje typ, který umí vytvořit [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instance. Zpřístupňuje jediného rozhraní API.

* CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor

IAuthenticatedEncryptorConfiguration si můžete představit jako objekt pro vytváření nejvyšší úrovně. Konfigurace slouží jako šablona. Ho zabalí algoritmické informace (například tuto konfiguraci vytváří popisovače s klíčem standardu AES-128-GCM hlavní), ale ještě nebyla je spojen s konkrétní klíč.

Když je volána CreateNewDescriptor, výhradně pro toto volání se vytvoří nový materiál klíče a vytváří nové IAuthenticatedEncryptorDescriptor který zabalí tuto materiál klíče a algoritmické informace požadované pro využívat materiál. Materiál klíče může vytvořit v softwaru (a uchovávat v paměti), může být vytvořen a uchovávat v modulu hardwarového zabezpečení a tak dále. Je velmi důležitý bod je, že jakékoli dvě volání CreateNewDescriptor by nikdy vytvořit ekvivalentní IAuthenticatedEncryptorDescriptor instance.

Typ IAuthenticatedEncryptorConfiguration slouží jako vstupní bod pro vytvoření klíče rutiny jako [automatické klíč vrácení](../implementation/key-management.md#key-expiration-and-rolling). Chcete-li změnit implementaci pro všechny budoucí klíče, zaregistrujte typu singleton IAuthenticatedEncryptorConfiguration v kontejneru služby.

---
