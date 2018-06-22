---
title: Zrušte ochranu datových částí, jejíž klíče byly odvolány v ASP.NET Core
author: rick-anderson
description: Postup zrušení ochrany dat, které jsou chráněné pomocí klíče, které od té doby byly odvolány, v aplikaci ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: ed0c2a309e899f018b09b3edc86b65b299647914
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272383"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>Zrušte ochranu datových částí, jejíž klíče byly odvolány v ASP.NET Core


<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

Ochrana dat ASP.NET Core rozhraní API nejsou primárně určený pro neomezené trvalost důvěrné datové části. Další technologie, jako [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) a [Azure Rights Management](https://docs.microsoft.com/rights-management/) jsou vhodnější ve scénáři neomezené úložiště, a mají možnosti odpovídajícím způsobem silné správy klíčů. Ale nutné dodat, není nic zakazují vývojář pomocí funkce Ochrana dat ASP.NET Core rozhraní API pro dlouhodobou ochranu důvěrných údajů. Jsou nikdy odebrány klíče z okruhu klíč, takže `IDataProtector.Unprotect` můžete vždy obnovit existující datových částí, dokud klíče jsou k dispozici a platné.

Však problém nastane, když se pokusí zrušení ochrany dat, který je chráněný pomocí odvolané klíče, jako vývojář `IDataProtector.Unprotect` v tomto případě vyvolá výjimku. To může být vhodná pro krátkodobou nebo přechodný datových částí (například ověřování tokenů), jak tyto druhy datových částí můžete snadno znovu vytvořit v systému a v nejhorším případě návštěvník bude pravděpodobně nutné znovu přihlásit. Ale pro trvalou datových částí, s `Unprotect` throw může způsobit ztrátu dat. nepřijatelné.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Pro podporu scénáře umožnit datových částí má být zrušena ochrana i při krátkodobém odvolané klíče systému ochrany dat obsahuje `IPersistedDataProtector` typu. Chcete-li získat instanci `IPersistedDataProtector`, jednoduše získat instanci `IDataProtector` v normálním způsobem a zkuste to přetypování `IDataProtector` k `IPersistedDataProtector`.

> [!NOTE]
> Ne všechny `IDataProtector` instance lze převést na `IPersistedDataProtector`. Vývojáři využít jazyka C# jako operátor nebo podobný, aby se zabránilo výjimky za běhu způsobené neplatné přetypování a měly by být připraven pro případ selhání správně zpracovat.

`IPersistedDataProtector` zpřístupní následující plochy rozhraní API:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Toto rozhraní API trvá chráněné datová část (jako bajtové pole) a vrátí nechráněné datové části. Neexistuje žádné přetížení založené na řetězec. Následují dva výstupní parametry.

* `requiresMigration`: bude nastaven na hodnotu true, pokud klíč používaný k ochraně tato datová část již není aktivní výchozí klíč, například klíč používaný k ochraně této datové části je v minulosti a klíč vrácení operace nemá od provedou místě. Volající chtít zvažte opětovné povolení ochrany datové části v závislosti na jejich obchodních potřeb.

* `wasRevoked`: bude nastavena na hodnotu true, pokud klíč používaný k ochraně tato datová část byl odvolán.

>[!WARNING]
> Prověření extrémně opatrní při předávání `ignoreRevocationErrors: true` k `DangerousUnprotect` metoda. Pokud po voláním této metody `wasRevoked` hodnota je nastavena hodnota true, pak klíč používaný k ochraně tato datová část byl odvolán a datové části pravosti zacházeno jako podezřelá. V takovém případě pouze pokračujte v datové části nechráněné Pokud máte některé samostatné zárukou, že je platná, například jestli pochází z zabezpečené databáze místo odesílány klientem nedůvěryhodné webové.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
