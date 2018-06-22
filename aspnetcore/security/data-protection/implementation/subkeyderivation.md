---
title: Odvození podklíčů a ověřené šifrování v ASP.NET Core
author: rick-anderson
description: Další podrobnosti implementace ochrany dat ASP.NET Core podklíčů odvození a ověřovat šifrování.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: 37e7b01700e8a6b755b5ed16a9d7d75a9eeb970e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275720"
---
# <a name="subkey-derivation-and-authenticated-encryption-in-aspnet-core"></a>Odvození podklíčů a ověřené šifrování v ASP.NET Core

<a name="data-protection-implementation-subkey-derivation"></a>

Většina klíčů v řetězci klíč bude obsahovat určitou formu šifrování a bude mít algoritmické informace s oznámením "režimu CBC šifrování + ověření HMAC s klíčem" nebo "GCM šifrování + ověření". V těchto případech označujeme embedded entropie jako jejich obsah (nebo KM) pro tento klíč a jsme provádět odvození klíče funkce odvození klíčů, které se použije pro vlastní kryptografické operace.

> [!NOTE]
> Klíče jsou abstraktní a vlastní implementaci nemusí chovat, jak je uvedeno níže. Pokud klíč poskytuje svou vlastní implementaci `IAuthenticatedEncryptor` místo pomocí jednoho z našich předdefinované objekty Factory, tento mechanismus popsaných v této části už neplatí.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Další ověřených dat a podklíčů odvození

`IAuthenticatedEncryptor` Rozhraní slouží jako základní rozhraní pro všechny operace ověřené šifrování. Jeho `Encrypt` metoda přebírá dva vyrovnávací paměti: ve formátu prostého textu a additionalAuthenticatedData (AAD). Tok obsah ve formátu prostého textu beze změny volání `IDataProtector.Protect`, ale AAD je generováno systémem a se skládá ze tří částí:

1. 32-bit magic hlavička 09 F0 C9 F0, který identifikuje tuto verzi systému ochrany dat.

2. Id klíče 128 bitů.

3. Řetězec s proměnnou délkou vytvořen z řetězu účel, který vytvořili `IDataProtector` , je provádění této operace.

AAD je jedinečný pro řazené kolekce členů všech tří součástí jsme můžete použít k odvození nových klíčů ze KM místo použití KM samotné ve všech našich kryptografických operací. Pro každé volání `IAuthenticatedEncryptor.Encrypt`, následující proces odvození klíče:

(K_E, K_H) = SP800_108_CTR_HMACSHA512 (K_M, AAD, contextHeader || keyModifier)

Zde voláme KDF SP800 108 NIST v režimu čítač (viz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) s následujícími parametry:

* Odvození klíče klíč (KDK) = K_M

* PRF = HMACSHA512

* Popisek = additionalAuthenticatedData

* kontext = contextHeader || keyModifier

Hlavička kontextu je s proměnnou délkou a v podstatě slouží jako kryptografický otisk algoritmy, pro které jsme se odvozování K_E a K_H. Modifikátor klíče je 128-bit řetězec náhodně vygenerované pro každé volání `Encrypt` a slouží k zajištění s zahltí pravděpodobnost, že KE a KH jsou jedinečné pro tuto operaci šifrování konkrétní ověřování, i v případě všech ostatních vstup KDF konstantní.

Pro režimu CBC šifrování + operace ověřování klíčem HMAC | K_E | je délka bloku symetrický šifrovací klíče, a | K_H | je velikost digest rutiny HMAC s klíčem. Pro šifrování GCM + operace ověřování | K_H | = 0.

## <a name="cbc-mode-encryption--hmac-validation"></a>Režimu CBC šifrování + ověření klíčem HMAC

Po vygenerování K_E prostřednictvím výše mechanismus jsme generování náhodných inicializační vektor a spuštění šifrovací algoritmus symetrického bloku na kódování jako prostý text. Inicializační vektor a ciphertext jsou spusťte prostřednictvím rutiny HMAC inicializován s klíčem K_H k vytvoření MAC. Tento proces a návratovou hodnotu je reprezentována graficky níže.

![Proces režimu CBC a vraťte se](subkeyderivation/_static/cbcprocess.png)

*výstup: = keyModifier || IV || E_cbc (K_E, iv, data) || Metoda HMAC (K_H, iv || E_cbc (K_E, iv, data))*

> [!NOTE]
> `IDataProtector.Protect` Bude implementace [předřazení magic záhlaví a id klíče](xref:security/data-protection/implementation/authenticated-encryption-details) do výstupu před jeho vrácením volající. Protože jsou implicitně magic záhlaví a id klíče součástí [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), a protože klíče modifikátor předány jako vstup KDF, to znamená, že každý jednobajtová poslední vrácená datové části je ověřována MAC.

## <a name="galoiscounter-mode-encryption--validation"></a>Režim Galois/čítač šifrování + ověření

Po vygenerování K_E prostřednictvím výše mechanismus jsme generování náhodných hodnotu nonce 96 bitů a spuštění šifrovací algoritmus symetrického bloku zašifrovat jako prostý text a vytvořit značku 128-bit ověřování.

![Proces režimu GCM a vraťte se](subkeyderivation/_static/galoisprocess.png)

*výstup: = keyModifier || hodnotu Nonce || E_gcm (K_E, nonce, data) || authTag*

> [!NOTE]
> I když GCM nativně podporuje koncept AAD, jsme se stále napájení AAD pouze na původní KDF vyjádření výslovného má být předán prázdný řetězec GCM pro její parametr AAD. Důvodem je dvojí. První, [pro podporu flexibility](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers) jsme nikdy nechcete používat K_M přímo jako šifrovací klíč. Kromě toho GCM ukládá velmi přísná jedinečnosti požadavků na vstupy. Pravděpodobnost, že rutiny šifrování GCM někdy vyvolaná na dva nebo více distinct nastaví vstupních dat se stejným (klíč, nonce) pár nesmí být delší než 2 ^ 32. Pokud jsme opravte K_E nejde udělat víc než 2 ^ 32 operace šifrování než jsme spustit afoul o 2 ^ omezit -32. Může se to zdát jako velký počet operací, ale vysokým provozem webového serveru můžete projít 4 miliardy požadavků v pouhé dní, a to i v rámci normálního doba života pro tyto klíče. Zůstane kompatibilní 2 ^ omezení pravděpodobnosti-32, budeme nadále používat modifikátor klíče 128-bit a hodnotu nonce 96 bitů, které výrazně rozšiřuje počet operací použitelné pro jakékoli dané K_M. Pro zjednodušení návrhu sdílíme KDF kódové cestě mezi operace CBC a GCM a vzhledem k tomu, že AAD považován za KDF již není třeba předávat do rutiny GCM.
