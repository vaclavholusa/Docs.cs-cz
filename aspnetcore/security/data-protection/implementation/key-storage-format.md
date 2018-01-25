---
title: "Formát úložiště klíčů"
author: tdykstra
description: "Tento dokument vysvětluje podrobnosti implementace formát úložiště klíčů ochrany dat ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 4fcc9d4b526ea85d123a257169039b879dd8e7df
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="key-storage-format"></a><span data-ttu-id="d2520-103">Formát úložiště klíčů</span><span class="sxs-lookup"><span data-stu-id="d2520-103">Key Storage Format</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="d2520-104">Objekty jsou uloženy v klidovém stavu uložených v reprezentaci XML.</span><span class="sxs-lookup"><span data-stu-id="d2520-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="d2520-105">Výchozí adresář pro úložiště klíčů je % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span><span class="sxs-lookup"><span data-stu-id="d2520-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="d2520-106">\<Klíč > elementu</span><span class="sxs-lookup"><span data-stu-id="d2520-106">The \<key> element</span></span>

<span data-ttu-id="d2520-107">Klíče existovat jako nejvyšší úrovně objektů v úložišti klíčů.</span><span class="sxs-lookup"><span data-stu-id="d2520-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="d2520-108">Podle konvence klíče měly název souboru **klíč-{guid} .xml**, kde {guid} je id klíče.</span><span class="sxs-lookup"><span data-stu-id="d2520-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="d2520-109">Každý takový soubor obsahuje jeden klíč.</span><span class="sxs-lookup"><span data-stu-id="d2520-109">Each such file contains a single key.</span></span> <span data-ttu-id="d2520-110">Formát souboru je následující.</span><span class="sxs-lookup"><span data-stu-id="d2520-110">The format of the file is as follows.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

<span data-ttu-id="d2520-111">\<Klíč > prvek obsahuje následující atributy a podřízených prvků:</span><span class="sxs-lookup"><span data-stu-id="d2520-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="d2520-112">Id klíče. Tato hodnota je považován za autoritativní; Název souboru je jednoduše nicety lidské čitelnější.</span><span class="sxs-lookup"><span data-stu-id="d2520-112">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="d2520-113">Verze \<klíč > elementu aktuálně pevné na 1.</span><span class="sxs-lookup"><span data-stu-id="d2520-113">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="d2520-114">Vytvoření, aktivace a datum vypršení platnosti klíče.</span><span class="sxs-lookup"><span data-stu-id="d2520-114">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="d2520-115">A \<popisovač > element, který obsahuje informace o používání ověřeného šifrování obsažené v rámci tohoto klíče.</span><span class="sxs-lookup"><span data-stu-id="d2520-115">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="d2520-116">V předchozím příkladu je id klíče {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, byl vytvořen a aktivovat 19. března 2015, a má životnost 90 dní.</span><span class="sxs-lookup"><span data-stu-id="d2520-116">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="d2520-117">(Někdy datum aktivace může být mírně před datum vytvoření jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="d2520-117">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="d2520-118">Toto je z důvodu jednotky v tom, jak rozhraní API pracovat a je neškodné v praxi.)</span><span class="sxs-lookup"><span data-stu-id="d2520-118">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="d2520-119">\<Popisovač > elementu</span><span class="sxs-lookup"><span data-stu-id="d2520-119">The \<descriptor> element</span></span>

<span data-ttu-id="d2520-120">Vnější \<popisovač > element obsahuje atribut deserializerType, což je sestavení kvalifikovaný název typu, který implementuje IAuthenticatedEncryptorDescriptorDeserializer.</span><span class="sxs-lookup"><span data-stu-id="d2520-120">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="d2520-121">Tento typ je zodpovědná za čtení vnitřní \<popisovač > elementu a k analýze informace obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="d2520-121">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="d2520-122">Konkrétní formát \<popisovač > element závisí na implementaci ověřené modulu pro šifrování zapouzdřené pomocí klíče a každý typ deserializátor očekává mírně odlišný formát pro tento.</span><span class="sxs-lookup"><span data-stu-id="d2520-122">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="d2520-123">Obecně platí, ale tento prvek bude obsahovat algoritmické informace (názvy, typy, identifikátory OID, nebo podobné) a materiál tajné klíče.</span><span class="sxs-lookup"><span data-stu-id="d2520-123">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="d2520-124">V předchozím příkladu popisovač určuje, že tento klíč zabalí šifrování AES-256-CBC + HMACSHA256 ověření.</span><span class="sxs-lookup"><span data-stu-id="d2520-124">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="d2520-125">\<EncryptedSecret > elementu</span><span class="sxs-lookup"><span data-stu-id="d2520-125">The \<encryptedSecret> element</span></span>

<span data-ttu-id="d2520-126"><encryptedSecret> Element, který obsahuje šifrované podobě tajné klíče materiálu mohou být přítomné Pokud [je povolené šifrování tajných klíčů v klidovém stavu](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="d2520-126">An <encryptedSecret> element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span></span> <span data-ttu-id="d2520-127">Atribut decryptorType bude sestavení kvalifikovaný název typu, který implementuje IXmlDecryptor.</span><span class="sxs-lookup"><span data-stu-id="d2520-127">The attribute decryptorType will be the assembly-qualified name of a type which implements IXmlDecryptor.</span></span> <span data-ttu-id="d2520-128">Tento typ je zodpovědná za čtení vnitřní <encryptedKey> elementu a dešifrování ji chcete obnovit původní ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="d2520-128">This type is responsible for reading the inner <encryptedKey> element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="d2520-129">Stejně jako u \<popisovač >, konkrétní formát <encryptedSecret> element závisí na mechanismu šifrování na rest používán.</span><span class="sxs-lookup"><span data-stu-id="d2520-129">As with \<descriptor>, the particular format of the <encryptedSecret> element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="d2520-130">V předchozím příkladu je hlavní klíč šifrované pomocí rozhraní Windows DPAPI na komentář.</span><span class="sxs-lookup"><span data-stu-id="d2520-130">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="d2520-131">\<Odvolání > elementu</span><span class="sxs-lookup"><span data-stu-id="d2520-131">The \<revocation> element</span></span>

<span data-ttu-id="d2520-132">Odvolání existovat jako nejvyšší úrovně objektů v úložišti klíčů.</span><span class="sxs-lookup"><span data-stu-id="d2520-132">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="d2520-133">Podle konvence odvolání mít název souboru **odvolání-{časové razítko} .xml** (pro všechny klíče odvolání před určitým datem) nebo **odvolání-{guid} .xml** (pro odvolání určitého klíče).</span><span class="sxs-lookup"><span data-stu-id="d2520-133">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="d2520-134">Každý soubor obsahuje jediný \<odvolání > elementu.</span><span class="sxs-lookup"><span data-stu-id="d2520-134">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="d2520-135">Pro odvolání jednotlivých klíčů, bude obsah souboru jako níže.</span><span class="sxs-lookup"><span data-stu-id="d2520-135">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="d2520-136">Zruší se v takovém případě se zadaným klíčem.</span><span class="sxs-lookup"><span data-stu-id="d2520-136">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="d2520-137">Pokud je id klíče "\*", ale stejně jako na následujícím příkladu jsou všechny klíče, jejichž datum vytvoření je před datem zadaný odvolání odvolán.</span><span class="sxs-lookup"><span data-stu-id="d2520-137">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="d2520-138">\<Důvod > elementu je nikdy pro čtení, v systému.</span><span class="sxs-lookup"><span data-stu-id="d2520-138">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="d2520-139">Je jednoduše vhodné místo pro ukládání čitelná pro člověka důvod zrušení.</span><span class="sxs-lookup"><span data-stu-id="d2520-139">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
