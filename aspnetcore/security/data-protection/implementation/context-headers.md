---
title: "Kontext hlavičky"
author: rick-anderson
description: "Tento dokument popisuje podrobnosti implementace hlaviček kontextu ochrany dat ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: b5ed2e48a55e23d73bccd01a731b35ea68f8944e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="context-headers"></a>Kontext hlavičky

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>Pozadí a teoreticky

V systému ochrany dat "klíč" znamená, že objekt, který může poskytnout ověřen šifrovací služby. Každý klíč je identifikována jedinečné id (GUID), a provede s ním algoritmické informací a materiálů, entropic. Každý klíč k provádění jedinečný šifrování, ale systém nemůže vynutit, a také je potřeba účet pro vývojáře, kteří mohou změnit prstenec klíč ručně změnou algoritmické informace existujícího klíče v řetězci klíč je určeno. K dosažení zadané těchto případech naše požadavky na zabezpečení systému ochrany dat má koncept [kryptografická Flexibilita](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), což umožňuje bezpečně prostřednictvím jednu hodnotu entropic napříč více kryptografické algoritmy.

Většina systémů, které podporují kryptografická flexibilita tomu včetně některých identifikační informace o algoritmus do datové části. Identifikátor OID algoritmus je obecně vhodným kandidátem na to. Však jeden problém, který jsme narazili je, že existuje několik způsobů určení stejného algoritmu: "AES" (CNG) a spravované Aes, AesManaged, AesCryptoServiceProvider, AesCng a RijndaelManaged (zadané konkrétní parametry) třídy jsou všechny ve skutečnosti stejné věcí a My by potřeba udržovat mapování všechny tyto na správný identifikátor OID. Vývojář chtěli poskytnout vlastní algoritmus (nebo i jiné implementace AES!), budou muset Řekněte nám, jeho OID. Tento krok navíc registrace zvlášť bolestivé díky konfiguraci systému.

Krokování s zpět, jsme se rozhodli, že jsme se blíží problém z nesprávný směr. Identifikátor OID řekne, co je algoritmus, ale nemůžeme nezáleží ve skutečnosti to. Pokud je potřeba použít jednu hodnotu entropic bezpečně v dva různé algoritmy, není nutné abychom věděli, jaké algoritmy ve skutečnosti. Co skutečně záleží nám je jejich chování. Libovolný dostatečnou symetrický bloku šifrovací algoritmus je také silné pseudonáhodných Permutace (PRP): Opravte vstupy (klíč, řetězení režimu, IV, ve formátu prostého textu) a výstup ciphertext se s zahltí pravděpodobnosti budou lišit od jiných bloku symetrický šifrovací algoritmus zadané stejnými vstupy. Podobně všechny funkce dostatečnou algoritmus hash je také silné Pseudonáhodná funkce (PRF) a daný vstupní sadu pevné její výstup převážné budou liší od jakýchkoli jiných algoritmus hash – funkce.

Tento koncept silné PRPs a PRFs používáme vybudovat hlavičku kontextu. Tuto hlavičku kontextu v podstatě funguje jako stabilní kryptografický otisk přes algoritmy používán pro všechny danou operaci, a poskytuje kryptografická flexibilita systému ochrany dat. Tuto hlavičku je reprodukovatelnou a se později používá jako součást [podklíčů odvození proces](subkeyderivation.md#data-protection-implementation-subkey-derivation). Vytvoření kontextu hlavičky v závislosti na režimů operace základní algoritmů dvěma různými způsoby.

## <a name="cbc-mode-encryption--hmac-authentication"></a>Režimu CBC šifrování + ověřování klíčem HMAC

<a name="data-protection-implementation-context-headers-cbc-components"></a>

Hlavička kontextu se skládá z následujících součástí:

* [16 bitů] Hodnota 00 00, což je značku znamená "CBC šifrování + ověřování HMAC s klíčem".

* [32bitová verze] Délka klíče (v bajtech, big endian) šifrovací algoritmus symetrického bloku.

* [32bitová verze] Velikost bloku (v bajtech, big endian) šifrovací algoritmus symetrického bloku.

* [32bitová verze] Délka klíče (v bajtech, big endian) algoritmus HMAC s klíčem. (Aktuálně velikost klíče vždy odpovídá velikost digest.)

* [32bitová verze] Velikost ověřování algoritmem digest (v bajtech, big endian) algoritmus HMAC s klíčem.

* EncCBC (K_E, IV, ""), který je výstup šifrovací algoritmus symetrického blok zadaný vstup prázdný řetězec a kde IV je vektor všechna než nula. Konstrukce K_E je popsáno níže.

* MAC (K_H, ""), což je výstup algoritmus HMAC s klíčem zadaného vstupu prázdný řetězec. Konstrukce K_H je popsáno níže.

V ideálním případě jsme může předávat všechny nula vektory K_E a K_H. Však chceme, aby se zabránilo situaci, kde algoritmus zkontroluje existenci slabé klíče před provedením jakékoli operace (zejména DES a 3DES), který vylučuje pomocí jednoduchý nebo repeatable vzoru, jako je vektor všechna než nula.

Místo toho používáme KDF SP800 108 NIST v režimu čítač (viz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) s nulovou délkou klíče, štítku a kontextu a HMACSHA512 jako základní PRF. Jsme odvozena | K_E | + | K_H | bajtů výstupu, pak rozložit výsledek na K_E a K_H sami. Matematický je reprezentováno následujícím způsobem.

(K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", popisek = "", kontext = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>Příklad: AES-192-CBC + HMACSHA256

Jako příklad zvažte případ, kdy je AES-192-CBC bloku symetrický šifrovací algoritmus a algoritmus pro ověření je HMACSHA256. Systém by vygeneroval hlavičku kontextu pomocí následujících kroků.

Nejprve umožní (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", popisek = "", kontext = ""), kde | K_E | = 192 bitů a | K_H | = 256 bitů na zadaný algoritmy. To vede k K_E = 5BB6... 21DD a K_H = A04A... 00A9 v následujícím příkladu:

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

V dalším kroku výpočetní Enc_CBC (K_E, IV, "") pro AES-192-CBC zadané IV = 0 * a K_E, jak je uvedeno výše.

result := F474B1872B3B53E4721DE19C0841DB6F

V dalším kroku výpočetní MAC (K_H, "") pro HMACSHA256 zadané K_H jako výše.

result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

Tímto se vytvoří hlavičku úplné kontextu níže:

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

Tuto hlavičku kontextu je kryptografický otisk dvojic ověřené šifrovací algoritmus (šifrování AES-192-CBC + HMACSHA256 ověření). Komponenty, jak je popsáno [výše](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) jsou:

* značky (00 00)

* Délka klíče cipher block (00 00 00 18)

* velikost bloku šifrovací blok (00 00 00 10)

* Délka klíče HMAC s klíčem (00 00 00 20)

* velikost digest HMAC s klíčem (00 00 00 20)

* blok šifer výstup zásad replikace HESEL (F4 74 - 6 DB f) a

* výstup PRF HMAC s klíčem (D4 79 - end).

> [!NOTE]
> Režimu CBC šifrování + HMAC záhlaví ověření kontextu je postavené stejným způsobem, bez ohledu na to, jestli jsou uvedeny implementace algoritmy Windows CNG nebo spravované SymmetricAlgorithm a KeyedHashAlgorithm typy. To umožňuje aplikacím spuštěným v různých operačních systémech spolehlivě vytvořit hlavičku stejné kontextu, i když implementace algoritmů liší pro jednotlivé operační systémy. (V praxi, KeyedHashAlgorithm nemusí být správný HMAC. Může být jakéhokoli typu šifrovacího algoritmu.)

### <a name="example-3des-192-cbc--hmacsha1"></a>Příklad: algoritmus 3DES. 192 CBC + HMACSHA1

Nejprve umožní (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", popisek = "", kontext = ""), kde | K_E | = 192 bitů a | K_H | = 160 bitů na zadaný algoritmy. To vede k K_E = A219... E2BB a K_H = DC4A... B464 v následujícím příkladu:

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

V dalším kroku výpočetní Enc_CBC (K_E, IV, "") pro algoritmus 3DES-192-CBC zadané IV = 0 * a K_E, jak je uvedeno výše.

výsledek: = ABB100F81E53E10E

V dalším kroku výpočetní MAC (K_H, "") pro HMACSHA1 zadané K_H jako výše.

result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

Tímto se vytvoří hlavičku úplné kontextu, což je kryptografický otisk ověřený šifrovací pár algoritmus (šifrování 3DES. 192 CBC + HMACSHA1 ověření), vidíte níže:

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

Součásti rozdělení následujícím způsobem:

* značky (00 00)

* Délka klíče cipher block (00 00 00 18)

* velikost bloku šifrovací blok (00 00 00 08)

* Délka klíče HMAC s klíčem (00 00 00 14)

* velikost digest HMAC s klíčem (00 00 00 14)

* blok šifer výstup zásad replikace HESEL (AB B1 - E1 0E) a

* výstup PRF HMAC s klíčem (76 EB - end).

## <a name="galoiscounter-mode-encryption--authentication"></a>Režim Galois/čítač šifrování + ověřování

Hlavička kontextu se skládá z následujících součástí:

* [16 bitů] Hodnota 00 01, což je značku znamená "GCM šifrování + ověřování".

* [32bitová verze] Délka klíče (v bajtech, big endian) šifrovací algoritmus symetrického bloku.

* [32bitová verze] Nonce velikost (v bajtech, big endian) používaných během operací ověřené šifrování. (Pro naše systému, to je stanovena v nonce velikosti = 96 bits.)

* [32bitová verze] Velikost bloku (v bajtech, big endian) šifrovací algoritmus symetrického bloku. (Pro GCM, to je stanoven velikost bloku = 128 bitů.)

* [32bitová verze] Ověřovací značky velikost (v bajtech, big endian) vytvořeného funkcí ověřené šifrování. (Pro naše systém, to je stanoven velikost značky = 128 bitů.)

* [128 bitů] Značky Enc_GCM (K_E nonce, ""), který je výstup šifrovací algoritmus symetrického blok zadaný vstup prázdný řetězec a kde hodnotu nonce vektor všechna nula 96 bitů.

K_E je odvozený pomocí stejného mechanismu jako CBC šifrování + scénář ověřování HMAC s klíčem. Ale vzhledem k tomu, že je zde play žádné K_H, v podstatě máme | K_H | = 0, a algoritmus Hroutí k níže uvedeného formuláře.

K_E = SP800_108_CTR (prf = HMACSHA512, key = "", popisek = "", kontext = "")

### <a name="example-aes-256-gcm"></a>Příklad: AES-256-GCM

Nejprve umožní K_E = SP800_108_CTR (prf = HMACSHA512, key = "", popisek = "", kontext = ""), kde | K_E | = 256 bitů.

K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

V dalším kroku výpočetní ověřování značky Enc_GCM (K_E nonce, "") pro AES-256-GCM danou hodnotu nonce = 096 a K_E jak je uvedeno výše.

result := E7DCCE66DF855A323A6BB7BD7A59BE45

Tímto se vytvoří hlavičku úplné kontextu níže:

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

Součásti rozdělení následujícím způsobem:

   * značky (00 01)

   * Délka klíče cipher block (00 00 00 20)

   * velikost nonce (00 00 00 0 C)

   * velikost bloku šifrovací blok (00 00 00 10)

   * velikost značky ověřování (00 00 00 10) a

   * značky ověřování ve spuštění block cipher (řadič domény E7 - end).
