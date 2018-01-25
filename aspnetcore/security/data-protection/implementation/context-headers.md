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
ms.openlocfilehash: 8ff0d867e4d3618524b8da98aafed8878d74581b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="context-headers"></a><span data-ttu-id="60db5-103">Kontext hlavičky</span><span class="sxs-lookup"><span data-stu-id="60db5-103">Context headers</span></span>

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a><span data-ttu-id="60db5-104">Pozadí a teoreticky</span><span class="sxs-lookup"><span data-stu-id="60db5-104">Background and theory</span></span>

<span data-ttu-id="60db5-105">V systému ochrany dat "klíč" znamená, že objekt, který může poskytnout ověřen šifrovací služby.</span><span class="sxs-lookup"><span data-stu-id="60db5-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="60db5-106">Každý klíč je identifikována jedinečné id (GUID), a provede s ním algoritmické informací a materiálů, entropic.</span><span class="sxs-lookup"><span data-stu-id="60db5-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="60db5-107">Každý klíč k provádění jedinečný šifrování, ale systém nemůže vynutit, a také je potřeba účet pro vývojáře, kteří mohou změnit prstenec klíč ručně změnou algoritmické informace existujícího klíče v řetězci klíč je určeno.</span><span class="sxs-lookup"><span data-stu-id="60db5-107">It's intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="60db5-108">K dosažení zadané těchto případech naše požadavky na zabezpečení systému ochrany dat má koncept [kryptografická Flexibilita](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), což umožňuje bezpečně prostřednictvím jednu hodnotu entropic napříč více kryptografické algoritmy.</span><span class="sxs-lookup"><span data-stu-id="60db5-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="60db5-109">Většina systémů, které podporují kryptografická flexibilita tomu včetně některých identifikační informace o algoritmus do datové části.</span><span class="sxs-lookup"><span data-stu-id="60db5-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="60db5-110">Identifikátor OID algoritmus je obecně vhodným kandidátem na to.</span><span class="sxs-lookup"><span data-stu-id="60db5-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="60db5-111">Však jeden problém, který jsme narazili je, že existuje několik způsobů určení stejného algoritmu: "AES" (CNG) a spravované Aes, AesManaged, AesCryptoServiceProvider, AesCng a RijndaelManaged (zadané konkrétní parametry) třídy jsou všechny ve skutečnosti stejné věcí a My by potřeba udržovat mapování všechny tyto na správný identifikátor OID.</span><span class="sxs-lookup"><span data-stu-id="60db5-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="60db5-112">Vývojář chtěli poskytnout vlastní algoritmus (nebo i jiné implementace AES!), budou muset Řekněte nám, jeho OID.</span><span class="sxs-lookup"><span data-stu-id="60db5-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="60db5-113">Tento krok navíc registrace zvlášť bolestivé díky konfiguraci systému.</span><span class="sxs-lookup"><span data-stu-id="60db5-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="60db5-114">Krokování s zpět, jsme se rozhodli, že jsme se blíží problém z nesprávný směr.</span><span class="sxs-lookup"><span data-stu-id="60db5-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="60db5-115">Identifikátor OID řekne, co je algoritmus, ale nemůžeme nezáleží ve skutečnosti to.</span><span class="sxs-lookup"><span data-stu-id="60db5-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="60db5-116">Pokud je potřeba použít jednu hodnotu entropic bezpečně v dva různé algoritmy, není nutné abychom věděli, jaké algoritmy ve skutečnosti.</span><span class="sxs-lookup"><span data-stu-id="60db5-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="60db5-117">Co skutečně záleží nám je jejich chování.</span><span class="sxs-lookup"><span data-stu-id="60db5-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="60db5-118">Libovolný dostatečnou symetrický bloku šifrovací algoritmus je také silné pseudonáhodných Permutace (PRP): Opravte vstupy (klíč, řetězení režimu, IV, ve formátu prostého textu) a výstup ciphertext se s zahltí pravděpodobnosti budou lišit od jiných bloku symetrický šifrovací algoritmus zadané stejnými vstupy.</span><span class="sxs-lookup"><span data-stu-id="60db5-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="60db5-119">Podobně všechny funkce dostatečnou algoritmus hash je také silné Pseudonáhodná funkce (PRF) a daný vstupní sadu pevné její výstup převážné budou liší od jakýchkoli jiných algoritmus hash – funkce.</span><span class="sxs-lookup"><span data-stu-id="60db5-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="60db5-120">Tento koncept silné PRPs a PRFs používáme vybudovat hlavičku kontextu.</span><span class="sxs-lookup"><span data-stu-id="60db5-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="60db5-121">Tuto hlavičku kontextu v podstatě funguje jako stabilní kryptografický otisk přes algoritmy používán pro všechny danou operaci, a poskytuje kryptografická flexibilita systému ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="60db5-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="60db5-122">Tuto hlavičku je reprodukovatelnou a se později používá jako součást [podklíčů odvození proces](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="60db5-122">This header is reproducible and is used later as part of the [subkey derivation process](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="60db5-123">Vytvoření kontextu hlavičky v závislosti na režimů operace základní algoritmů dvěma různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="60db5-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="60db5-124">Režimu CBC šifrování + ověřování klíčem HMAC</span><span class="sxs-lookup"><span data-stu-id="60db5-124">CBC-mode encryption + HMAC authentication</span></span>

<a name="data-protection-implementation-context-headers-cbc-components"></a>

<span data-ttu-id="60db5-125">Hlavička kontextu se skládá z následujících součástí:</span><span class="sxs-lookup"><span data-stu-id="60db5-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="60db5-126">[16 bitů] Hodnota 00 00, což je značku znamená "CBC šifrování + ověřování HMAC s klíčem".</span><span class="sxs-lookup"><span data-stu-id="60db5-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="60db5-127">[32bitová verze] Délka klíče (v bajtech, big endian) šifrovací algoritmus symetrického bloku.</span><span class="sxs-lookup"><span data-stu-id="60db5-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="60db5-128">[32bitová verze] Velikost bloku (v bajtech, big endian) šifrovací algoritmus symetrického bloku.</span><span class="sxs-lookup"><span data-stu-id="60db5-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="60db5-129">[32bitová verze] Délka klíče (v bajtech, big endian) algoritmus HMAC s klíčem.</span><span class="sxs-lookup"><span data-stu-id="60db5-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="60db5-130">(Aktuálně velikost klíče vždy odpovídá velikost digest.)</span><span class="sxs-lookup"><span data-stu-id="60db5-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="60db5-131">[32bitová verze] Velikost ověřování algoritmem digest (v bajtech, big endian) algoritmus HMAC s klíčem.</span><span class="sxs-lookup"><span data-stu-id="60db5-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="60db5-132">EncCBC (K_E, IV, ""), který je výstup šifrovací algoritmus symetrického blok zadaný vstup prázdný řetězec a kde IV je vektor všechna než nula.</span><span class="sxs-lookup"><span data-stu-id="60db5-132">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="60db5-133">Konstrukce K_E je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="60db5-133">The construction of K_E is described below.</span></span>

* <span data-ttu-id="60db5-134">MAC (K_H, ""), což je výstup algoritmus HMAC s klíčem zadaného vstupu prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="60db5-134">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="60db5-135">Konstrukce K_H je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="60db5-135">The construction of K_H is described below.</span></span>

<span data-ttu-id="60db5-136">V ideálním případě jsme může předávat všechny nula vektory K_E a K_H.</span><span class="sxs-lookup"><span data-stu-id="60db5-136">Ideally, we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="60db5-137">Však chceme, aby se zabránilo situaci, kde algoritmus zkontroluje existenci slabé klíče před provedením jakékoli operace (zejména DES a 3DES), který vylučuje pomocí jednoduchý nebo repeatable vzoru, jako je vektor všechna než nula.</span><span class="sxs-lookup"><span data-stu-id="60db5-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="60db5-138">Místo toho používáme KDF SP800 108 NIST v režimu čítač (viz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) s nulovou délkou klíče, štítku a kontextu a HMACSHA512 jako základní PRF.</span><span class="sxs-lookup"><span data-stu-id="60db5-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="60db5-139">Jsme odvozena | K_E | + | K_H | bajtů výstupu, pak rozložit výsledek na K_E a K_H sami.</span><span class="sxs-lookup"><span data-stu-id="60db5-139">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="60db5-140">Matematický je reprezentováno následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="60db5-140">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="60db5-141">(K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", popisek = "", kontext = "")</span><span class="sxs-lookup"><span data-stu-id="60db5-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="60db5-142">Příklad: AES-192-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="60db5-142">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="60db5-143">Jako příklad zvažte případ, kdy je AES-192-CBC bloku symetrický šifrovací algoritmus a algoritmus pro ověření je HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="60db5-143">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="60db5-144">Systém by vygeneroval hlavičku kontextu pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="60db5-144">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="60db5-145">Nejprve umožní (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", popisek = "", kontext = ""), kde | K_E | = 192 bitů a | K_H | = 256 bitů na zadaný algoritmy.</span><span class="sxs-lookup"><span data-stu-id="60db5-145">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="60db5-146">To vede k K_E = 5BB6... 21DD a K_H = A04A... 00A9 v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="60db5-146">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

<span data-ttu-id="60db5-147">V dalším kroku výpočetní Enc_CBC (K_E, IV, "") pro AES-192-CBC zadané IV = 0 \* a K_E, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="60db5-147">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="60db5-148">result := F474B1872B3B53E4721DE19C0841DB6F</span><span class="sxs-lookup"><span data-stu-id="60db5-148">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="60db5-149">V dalším kroku výpočetní MAC (K_H, "") pro HMACSHA256 zadané K_H jako výše.</span><span class="sxs-lookup"><span data-stu-id="60db5-149">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="60db5-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span><span class="sxs-lookup"><span data-stu-id="60db5-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="60db5-151">Tímto se vytvoří hlavičku úplné kontextu níže:</span><span class="sxs-lookup"><span data-stu-id="60db5-151">This produces the full context header below:</span></span>

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

<span data-ttu-id="60db5-152">Tuto hlavičku kontextu je kryptografický otisk dvojic ověřené šifrovací algoritmus (šifrování AES-192-CBC + HMACSHA256 ověření).</span><span class="sxs-lookup"><span data-stu-id="60db5-152">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="60db5-153">Komponenty, jak je popsáno [výše](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) jsou:</span><span class="sxs-lookup"><span data-stu-id="60db5-153">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="60db5-154">značky (00 00)</span><span class="sxs-lookup"><span data-stu-id="60db5-154">the marker (00 00)</span></span>

* <span data-ttu-id="60db5-155">Délka klíče cipher block (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="60db5-155">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="60db5-156">velikost bloku šifrovací blok (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="60db5-156">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="60db5-157">Délka klíče HMAC s klíčem (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="60db5-157">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="60db5-158">velikost digest HMAC s klíčem (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="60db5-158">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="60db5-159">blok šifer výstup zásad replikace HESEL (F4 74 - 6 DB f) a</span><span class="sxs-lookup"><span data-stu-id="60db5-159">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="60db5-160">výstup PRF HMAC s klíčem (D4 79 - end).</span><span class="sxs-lookup"><span data-stu-id="60db5-160">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="60db5-161">Režimu CBC šifrování + HMAC záhlaví ověření kontextu je postavené stejným způsobem, bez ohledu na to, jestli jsou uvedeny implementace algoritmy Windows CNG nebo spravované SymmetricAlgorithm a KeyedHashAlgorithm typy.</span><span class="sxs-lookup"><span data-stu-id="60db5-161">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="60db5-162">To umožňuje aplikacím spuštěným v různých operačních systémech spolehlivě vytvořit hlavičku stejné kontextu, i když implementace algoritmů liší pro jednotlivé operační systémy.</span><span class="sxs-lookup"><span data-stu-id="60db5-162">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="60db5-163">(V praxi, KeyedHashAlgorithm nemusí být správný HMAC.</span><span class="sxs-lookup"><span data-stu-id="60db5-163">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="60db5-164">Může být jakéhokoli typu šifrovacího algoritmu.)</span><span class="sxs-lookup"><span data-stu-id="60db5-164">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="60db5-165">Příklad: algoritmus 3DES. 192 CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="60db5-165">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="60db5-166">Nejprve umožní (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", popisek = "", kontext = ""), kde | K_E | = 192 bitů a | K_H | = 160 bitů na zadaný algoritmy.</span><span class="sxs-lookup"><span data-stu-id="60db5-166">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="60db5-167">To vede k K_E = A219... E2BB a K_H = DC4A... B464 v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="60db5-167">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

<span data-ttu-id="60db5-168">V dalším kroku výpočetní Enc_CBC (K_E, IV, "") pro algoritmus 3DES-192-CBC zadané IV = 0 \* a K_E, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="60db5-168">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="60db5-169">výsledek: = ABB100F81E53E10E</span><span class="sxs-lookup"><span data-stu-id="60db5-169">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="60db5-170">V dalším kroku výpočetní MAC (K_H, "") pro HMACSHA1 zadané K_H jako výše.</span><span class="sxs-lookup"><span data-stu-id="60db5-170">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="60db5-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span><span class="sxs-lookup"><span data-stu-id="60db5-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="60db5-172">Tímto se vytvoří hlavičku úplné kontextu, což je kryptografický otisk ověřený šifrovací pár algoritmus (šifrování 3DES. 192 CBC + HMACSHA1 ověření), vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="60db5-172">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

<span data-ttu-id="60db5-173">Součásti rozdělení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="60db5-173">The components break down as follows:</span></span>

* <span data-ttu-id="60db5-174">značky (00 00)</span><span class="sxs-lookup"><span data-stu-id="60db5-174">the marker (00 00)</span></span>

* <span data-ttu-id="60db5-175">Délka klíče cipher block (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="60db5-175">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="60db5-176">velikost bloku šifrovací blok (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="60db5-176">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="60db5-177">Délka klíče HMAC s klíčem (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="60db5-177">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="60db5-178">velikost digest HMAC s klíčem (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="60db5-178">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="60db5-179">blok šifer výstup zásad replikace HESEL (AB B1 - E1 0E) a</span><span class="sxs-lookup"><span data-stu-id="60db5-179">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="60db5-180">výstup PRF HMAC s klíčem (76 EB - end).</span><span class="sxs-lookup"><span data-stu-id="60db5-180">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="60db5-181">Režim Galois/čítač šifrování + ověřování</span><span class="sxs-lookup"><span data-stu-id="60db5-181">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="60db5-182">Hlavička kontextu se skládá z následujících součástí:</span><span class="sxs-lookup"><span data-stu-id="60db5-182">The context header consists of the following components:</span></span>

* <span data-ttu-id="60db5-183">[16 bitů] Hodnota 00 01, což je značku znamená "GCM šifrování + ověřování".</span><span class="sxs-lookup"><span data-stu-id="60db5-183">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="60db5-184">[32bitová verze] Délka klíče (v bajtech, big endian) šifrovací algoritmus symetrického bloku.</span><span class="sxs-lookup"><span data-stu-id="60db5-184">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="60db5-185">[32bitová verze] Nonce velikost (v bajtech, big endian) používaných během operací ověřené šifrování.</span><span class="sxs-lookup"><span data-stu-id="60db5-185">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="60db5-186">(Pro naše systému, to je stanovena v nonce velikosti = 96 bits.)</span><span class="sxs-lookup"><span data-stu-id="60db5-186">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="60db5-187">[32bitová verze] Velikost bloku (v bajtech, big endian) šifrovací algoritmus symetrického bloku.</span><span class="sxs-lookup"><span data-stu-id="60db5-187">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="60db5-188">(Pro GCM, to je stanoven velikost bloku = 128 bitů.)</span><span class="sxs-lookup"><span data-stu-id="60db5-188">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="60db5-189">[32bitová verze] Ověřovací značky velikost (v bajtech, big endian) vytvořeného funkcí ověřené šifrování.</span><span class="sxs-lookup"><span data-stu-id="60db5-189">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="60db5-190">(Pro naše systém, to je stanoven velikost značky = 128 bitů.)</span><span class="sxs-lookup"><span data-stu-id="60db5-190">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="60db5-191">[128 bitů] Značky Enc_GCM (K_E nonce, ""), který je výstup šifrovací algoritmus symetrického blok zadaný vstup prázdný řetězec a kde hodnotu nonce vektor všechna nula 96 bitů.</span><span class="sxs-lookup"><span data-stu-id="60db5-191">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="60db5-192">K_E je odvozený pomocí stejného mechanismu jako CBC šifrování + scénář ověřování HMAC s klíčem.</span><span class="sxs-lookup"><span data-stu-id="60db5-192">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="60db5-193">Ale vzhledem k tomu, že je zde play žádné K_H, v podstatě máme | K_H | = 0, a algoritmus Hroutí k níže uvedeného formuláře.</span><span class="sxs-lookup"><span data-stu-id="60db5-193">However, since there's no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="60db5-194">K_E = SP800_108_CTR (prf = HMACSHA512, key = "", popisek = "", kontext = "")</span><span class="sxs-lookup"><span data-stu-id="60db5-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="60db5-195">Příklad: AES-256-GCM</span><span class="sxs-lookup"><span data-stu-id="60db5-195">Example: AES-256-GCM</span></span>

<span data-ttu-id="60db5-196">Nejprve umožní K_E = SP800_108_CTR (prf = HMACSHA512, key = "", popisek = "", kontext = ""), kde | K_E | = 256 bitů.</span><span class="sxs-lookup"><span data-stu-id="60db5-196">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="60db5-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span><span class="sxs-lookup"><span data-stu-id="60db5-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="60db5-198">V dalším kroku výpočetní ověřování značky Enc_GCM (K_E nonce, "") pro AES-256-GCM danou hodnotu nonce = 096 a K_E jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="60db5-198">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="60db5-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span><span class="sxs-lookup"><span data-stu-id="60db5-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="60db5-200">Tímto se vytvoří hlavičku úplné kontextu níže:</span><span class="sxs-lookup"><span data-stu-id="60db5-200">This produces the full context header below:</span></span>

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

<span data-ttu-id="60db5-201">Součásti rozdělení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="60db5-201">The components break down as follows:</span></span>

   * <span data-ttu-id="60db5-202">značky (00 01)</span><span class="sxs-lookup"><span data-stu-id="60db5-202">the marker (00 01)</span></span>

   * <span data-ttu-id="60db5-203">Délka klíče cipher block (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="60db5-203">the block cipher key length (00 00 00 20)</span></span>

   * <span data-ttu-id="60db5-204">velikost nonce (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="60db5-204">the nonce size (00 00 00 0C)</span></span>

   * <span data-ttu-id="60db5-205">velikost bloku šifrovací blok (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="60db5-205">the block cipher block size (00 00 00 10)</span></span>

   * <span data-ttu-id="60db5-206">velikost značky ověřování (00 00 00 10) a</span><span class="sxs-lookup"><span data-stu-id="60db5-206">the authentication tag size (00 00 00 10) and</span></span>

   * <span data-ttu-id="60db5-207">značky ověřování ve spuštění block cipher (řadič domény E7 - end).</span><span class="sxs-lookup"><span data-stu-id="60db5-207">the authentication tag from running the block cipher (E7 DC - end).</span></span>
