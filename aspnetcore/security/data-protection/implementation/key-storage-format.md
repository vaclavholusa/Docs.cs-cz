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
# <a name="key-storage-format"></a>Formát úložiště klíčů

<a name="data-protection-implementation-key-storage-format"></a>

Objekty jsou uloženy v klidovém stavu uložených v reprezentaci XML. Výchozí adresář pro úložiště klíčů je % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.

## <a name="the-key-element"></a>\<Klíč > elementu

Klíče existovat jako nejvyšší úrovně objektů v úložišti klíčů. Podle konvence klíče měly název souboru **klíč-{guid} .xml**, kde {guid} je id klíče. Každý takový soubor obsahuje jeden klíč. Formát souboru je následující.

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

\<Klíč > prvek obsahuje následující atributy a podřízených prvků:

* Id klíče. Tato hodnota je považován za autoritativní; Název souboru je jednoduše nicety lidské čitelnější.

* Verze \<klíč > elementu aktuálně pevné na 1.

* Vytvoření, aktivace a datum vypršení platnosti klíče.

* A \<popisovač > element, který obsahuje informace o používání ověřeného šifrování obsažené v rámci tohoto klíče.

V předchozím příkladu je id klíče {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, byl vytvořen a aktivovat 19. března 2015, a má životnost 90 dní. (Někdy datum aktivace může být mírně před datum vytvoření jako v následujícím příkladu. Toto je z důvodu jednotky v tom, jak rozhraní API pracovat a je neškodné v praxi.)

## <a name="the-descriptor-element"></a>\<Popisovač > elementu

Vnější \<popisovač > element obsahuje atribut deserializerType, což je sestavení kvalifikovaný název typu, který implementuje IAuthenticatedEncryptorDescriptorDeserializer. Tento typ je zodpovědná za čtení vnitřní \<popisovač > elementu a k analýze informace obsažené v rámci.

Konkrétní formát \<popisovač > element závisí na implementaci ověřené modulu pro šifrování zapouzdřené pomocí klíče a každý typ deserializátor očekává mírně odlišný formát pro tento. Obecně platí, ale tento prvek bude obsahovat algoritmické informace (názvy, typy, identifikátory OID, nebo podobné) a materiál tajné klíče. V předchozím příkladu popisovač určuje, že tento klíč zabalí šifrování AES-256-CBC + HMACSHA256 ověření.

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret > elementu

<encryptedSecret> Element, který obsahuje šifrované podobě tajné klíče materiálu mohou být přítomné Pokud [je povolené šifrování tajných klíčů v klidovém stavu](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest). Atribut decryptorType bude sestavení kvalifikovaný název typu, který implementuje IXmlDecryptor. Tento typ je zodpovědná za čtení vnitřní <encryptedKey> elementu a dešifrování ji chcete obnovit původní ve formátu prostého textu.

Stejně jako u \<popisovač >, konkrétní formát <encryptedSecret> element závisí na mechanismu šifrování na rest používán. V předchozím příkladu je hlavní klíč šifrované pomocí rozhraní Windows DPAPI na komentář.

## <a name="the-revocation-element"></a>\<Odvolání > elementu

Odvolání existovat jako nejvyšší úrovně objektů v úložišti klíčů. Podle konvence odvolání mít název souboru **odvolání-{časové razítko} .xml** (pro všechny klíče odvolání před určitým datem) nebo **odvolání-{guid} .xml** (pro odvolání určitého klíče). Každý soubor obsahuje jediný \<odvolání > elementu.

Pro odvolání jednotlivých klíčů, bude obsah souboru jako níže.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

Zruší se v takovém případě se zadaným klíčem. Pokud je id klíče "*", ale stejně jako na následujícím příkladu jsou všechny klíče, jejichž datum vytvoření je před datem zadaný odvolání odvolán.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<Důvod > elementu je nikdy pro čtení, v systému. Je jednoduše vhodné místo pro ukládání čitelná pro člověka důvod zrušení.
