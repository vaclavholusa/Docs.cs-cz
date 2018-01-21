---
title: "Podpora zásad celého systému ochrany dat v ASP.NET Core"
author: rick-anderson
description: "Další informace o podpoře pro nastavení zásad celého systému výchozí pro všechny aplikace, které využívají ochranu dat ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 4c3ae3b628ebe17c7926c71f1fad664d719d1706
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Podpora zásad celého systému ochrany dat v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Při spuštění v systému Windows, ochrany dat systému má omezenou podporu pro nastavení zásad celého systému výchozí pro všechny aplikace, které využívají ochranu dat ASP.NET Core. Obecné cílem je, Správce může chcete změnit výchozí nastavení, například používá algoritmy nebo životnosti klíče, aniž by bylo nutné ručně aktualizovat všechny aplikace na počítači.

> [!WARNING]
> Správce systému můžete nastavit výchozí zásady, ale jejich ho nemůže vynutit. Vývojáři aplikace můžete vždy přepsat hodnotou s jedním z vlastní výběr. Výchozí zásada ovlivňuje pouze tam, kde vývojář nebyl zadán explicitní hodnotu pro nastavení aplikace.

## <a name="setting-default-policy"></a>Nastavení výchozích zásad

Pokud chcete nastavit výchozí zásady, Správce může nastavit známé hodnoty v registru systému v následujícím klíči registru:

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Pokud jste na 64bitový operační systém a chcete ovlivnit chování aplikací pro 32-bit, nezapomeňte nakonfigurovat ekvivalentní Wow6432Node výše klíče.

Níže jsou uvedeny podporované hodnoty.

| Hodnota              | Typ   | Popis |
| ------------------ | :----: | ----------- |
| EncryptionType     | odkazy řetězců | Určuje, které algoritmy se použije pro ochranu dat. Hodnota musí být CNG CBC, CNG GCM nebo spravované a je podrobněji popsané v níže. |
| DefaultKeyLifetime | DWORD  | Určuje životnost pro nově vygenerované klíče. Hodnota je uveden v dny a musí být > = 7. |
| KeyEscrowSinks     | odkazy řetězců | Určuje typy, které se používají pro klíče úschově. Hodnota je seznam oddělený středníkem klíče úschově jímky, kde každý prvek v seznamu je sestavení kvalifikovaný název typu, který implementuje [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Typy šifrování

Pokud EncryptionType CNG CBC, systém je konfigurovány k použití šifru symetrický bloku režimu CBC pro utajení a klíčem HMAC pro pravosti s služby poskytované Windows CNG (viz [zadání vlastní Windows CNG algoritmy](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) pro Další podrobnosti). Jsou podporovány následující další hodnoty, z nichž každý odpovídá vlastnosti na typ CngCbcAuthenticatedEncryptionSettings.

| Hodnota                       | Typ   | Popis |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | odkazy řetězců | Název algoritmu symetrického bloku šifrovací rozumí CNG. Tento algoritmus je otevřen v režimu CBC. |
| EncryptionAlgorithmProvider | odkazy řetězců | Název implementace poskytovatele CNG, který může vytvářet algoritmus EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | Délka klíče odvození pro šifrovací algoritmus symetrického bloku (v bitech). |
| Algoritmus has               | odkazy řetězců | Název algoritmu hash rozumí CNG. Tento algoritmus je otevřen v režimu HMAC s klíčem. |
| HashAlgorithmProvider       | odkazy řetězců | Název implementace poskytovatele CNG, který může vytvářet algoritmus algoritmus has. |

Pokud EncryptionType CNG GCM, systém je konfigurovány k použití šifru symetrický bloku Galois/čítač režimu pro utajení a pravosti s služby poskytované Windows CNG (viz [zadání vlastní Windows CNG algoritmy](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) Další podrobnosti). Jsou podporovány následující další hodnoty, z nichž každý odpovídá vlastnosti na typ CngGcmAuthenticatedEncryptionSettings.

| Hodnota                       | Typ   | Popis |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | odkazy řetězců | Název algoritmu symetrického bloku šifrovací rozumí CNG. Tento algoritmus je otevřen v režimu Galois čítačů. |
| EncryptionAlgorithmProvider | odkazy řetězců | Název implementace poskytovatele CNG, který může vytvářet algoritmus EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | Délka klíče odvození pro šifrovací algoritmus symetrického bloku (v bitech). |

Když EncryptionType spravované, systém je nakonfigurován pro použití spravovaných SymmetricAlgorithm pro důvěrnost a KeyedHashAlgorithm pro pravosti (najdete v části [zadání vlastní spravované algoritmy](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) podrobnosti). Jsou podporovány následující další hodnoty, z nichž každý odpovídá vlastnosti na typ ManagedAuthenticatedEncryptionSettings.

| Hodnota                      | Typ   | Popis |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | odkazy řetězců | Název sestavení kvalifikovaný typ, který implementuje SymmetricAlgorithm. |
| EncryptionAlgorithmKeySize | DWORD  | Délka klíče odvození pro algoritmus symetrického šifrování (v bitech). |
| ValidationAlgorithmType    | odkazy řetězců | Název typu, který implementuje KeyedHashAlgorithm kvalifikovaný sestavení. |

Pokud EncryptionType jakoukoli jinou hodnotu než null nebo prázdný, ochrany dat systému vyvolá výjimku při spuštění.

> [!WARNING]
> Při konfiguraci nastavení výchozích zásad, které zahrnuje názvy typů (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), typy musí být k dispozici pro aplikaci. To znamená, že pro aplikace běžící na ploše CLR, sestavení, které obsahují tyto typy by měla být v globální mezipaměti sestavení (GAC). ASP.NET Core aplikací běžících na [.NET Core](https://www.microsoft.com/net/core), balíčky, které obsahují tyto typy by měly být nainstalovány.
