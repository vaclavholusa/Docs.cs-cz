---
title: "Probíhá rušení ochrany datových částí, které byly odvolány jejíž klíče"
author: rick-anderson
description: "Tento dokument vysvětluje, jak zrušit ochranu dat, které jsou chráněné pomocí klíče, které od té doby byly odvolány, v aplikaci ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 37332dda794f898fb866424b38394f5d4441e166
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a><span data-ttu-id="fb7ea-103">Probíhá rušení ochrany datových částí, které byly odvolány jejíž klíče</span><span class="sxs-lookup"><span data-stu-id="fb7ea-103">Unprotecting payloads whose keys have been revoked</span></span>

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

<span data-ttu-id="fb7ea-104">Ochrana dat ASP.NET Core rozhraní API nejsou primárně určený pro neomezené trvalost důvěrné datové části.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-104">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="fb7ea-105">Další technologie, jako [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) a [Azure Rights Management](https://docs.microsoft.com/rights-management/) jsou vhodnější ve scénáři neomezené úložiště, a mají možnosti odpovídajícím způsobem silné správy klíčů.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-105">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](https://docs.microsoft.com/rights-management/) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="fb7ea-106">Ale nutné dodat, není nic zakazují vývojář pomocí funkce Ochrana dat ASP.NET Core rozhraní API pro dlouhodobou ochranu důvěrných údajů.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-106">That said, there's nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="fb7ea-107">Jsou nikdy odebrány klíče z okruhu klíč, takže `IDataProtector.Unprotect` můžete vždy obnovit existující datových částí, dokud klíče jsou k dispozici a platné.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-107">Keys are never removed from the key ring, so `IDataProtector.Unprotect` can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="fb7ea-108">Však problém nastane, když se pokusí zrušení ochrany dat, který je chráněný pomocí odvolané klíče, jako vývojář `IDataProtector.Unprotect` v tomto případě vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-108">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as `IDataProtector.Unprotect` will throw an exception in this case.</span></span> <span data-ttu-id="fb7ea-109">To může být vhodná pro krátkodobou nebo přechodný datových částí (například ověřování tokenů), jak tyto druhy datových částí můžete snadno znovu vytvořit v systému a v nejhorším případě návštěvník bude pravděpodobně nutné znovu přihlásit.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-109">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="fb7ea-110">Ale pro trvalou datových částí, s `Unprotect` throw může způsobit ztrátu dat. nepřijatelné.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-110">But for persisted payloads, having `Unprotect` throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="fb7ea-111">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="fb7ea-111">IPersistedDataProtector</span></span>

<span data-ttu-id="fb7ea-112">Pro podporu scénáře umožnit datových částí má být zrušena ochrana i při krátkodobém odvolané klíče systému ochrany dat obsahuje `IPersistedDataProtector` typu.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-112">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an `IPersistedDataProtector` type.</span></span> <span data-ttu-id="fb7ea-113">Chcete-li získat instanci `IPersistedDataProtector`, jednoduše získat instanci `IDataProtector` v normálním způsobem a zkuste to přetypování `IDataProtector` k `IPersistedDataProtector`.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-113">To get an instance of `IPersistedDataProtector`, simply get an instance of `IDataProtector` in the normal fashion and try casting the `IDataProtector` to `IPersistedDataProtector`.</span></span>

> [!NOTE]
> <span data-ttu-id="fb7ea-114">Ne všechny `IDataProtector` instance lze převést na `IPersistedDataProtector`.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-114">Not all `IDataProtector` instances can be cast to `IPersistedDataProtector`.</span></span> <span data-ttu-id="fb7ea-115">Vývojáři využít jazyka C# jako operátor nebo podobný, aby se zabránilo výjimky za běhu způsobené neplatné přetypování a měly by být připraven pro případ selhání správně zpracovat.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-115">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="fb7ea-116">`IPersistedDataProtector` zpřístupní následující plochy rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="fb7ea-116">`IPersistedDataProtector` exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

<span data-ttu-id="fb7ea-117">Toto rozhraní API trvá chráněné datová část (jako bajtové pole) a vrátí nechráněné datové části.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-117">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="fb7ea-118">Neexistuje žádné přetížení založené na řetězec.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-118">There's no string-based overload.</span></span> <span data-ttu-id="fb7ea-119">Následují dva výstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-119">The two out parameters are as follows.</span></span>

* <span data-ttu-id="fb7ea-120">`requiresMigration`: bude nastaven na hodnotu true, pokud klíč používaný k ochraně tato datová část již není aktivní výchozí klíč, například klíč používaný k ochraně této datové části je v minulosti a klíč vrácení operace nemá od provedou místě.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-120">`requiresMigration`: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="fb7ea-121">Volající chtít zvažte opětovné povolení ochrany datové části v závislosti na jejich obchodních potřeb.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-121">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="fb7ea-122">`wasRevoked`: bude nastavena na hodnotu true, pokud klíč používaný k ochraně tato datová část byl odvolán.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-122">`wasRevoked`: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="fb7ea-123">Prověření extrémně opatrní při předávání `ignoreRevocationErrors: true` k `DangerousUnprotect` metoda.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-123">Exercise extreme caution when passing `ignoreRevocationErrors: true` to the `DangerousUnprotect` method.</span></span> <span data-ttu-id="fb7ea-124">Pokud po voláním této metody `wasRevoked` hodnota je nastavena hodnota true, pak klíč používaný k ochraně tato datová část byl odvolán a datové části pravosti zacházeno jako podezřelá.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-124">If after calling this method the `wasRevoked` value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="fb7ea-125">V takovém případě pouze pokračujte v datové části nechráněné Pokud máte některé samostatné zárukou, že je platná, například jestli pochází z zabezpečené databáze místo odesílány klientem nedůvěryhodné webové.</span><span class="sxs-lookup"><span data-stu-id="fb7ea-125">In this case, only continue operating on the unprotected payload if you have some separate assurance that it's authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
