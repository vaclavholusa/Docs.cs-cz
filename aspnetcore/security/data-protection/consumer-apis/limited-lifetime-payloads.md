---
title: Omezení doby trvání chráněné datové části v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak omezit životnost chráněné pomocí rozhraní API ASP.NET Core Data Protection datové části.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 324887b3d29de989ad855c4e78fd5a235fdb560e
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>Omezení doby trvání chráněné datové části v ASP.NET Core

Existují scénáře, kdy vývojář aplikace chce vytvořit chráněné datové části, jejíž platnost vyprší po nastaveném časovém období. Například může chráněné datové části představují token pro resetování hesla, která má být platné pouze pro jednu hodinu. To je určitě možné pro vývojáře k vytvoření vlastní datovou část formátu, který obsahuje datum vypršení platnosti embedded a pokročilé vývojáře může Chcete přesto provést, ale pro většinu vývojáři Správa těchto vypršení můžou růst zdlouhavé.

Pro zjednodušení pro naše cílová skupina vývojáře balíček [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) obsahuje nástroj rozhraní API pro vytváření datových částí, které automaticky vyprší po nastaveném časovém období. Tato rozhraní API přečnívat odhlásit z `ITimeLimitedDataProtector` typu.

## <a name="api-usage"></a>Využití rozhraní API

`ITimeLimitedDataProtector` Rozhraní je základní rozhraní pro ochranu a při rušení časově omezené / samoobslužné zneplatněním obsažených datových částí. K vytvoření instance `ITimeLimitedDataProtector`, je nutné nejdříve instanci běžný [IDataProtector](xref:security/data-protection/consumer-apis/overview) sestavený s konkrétním účelem. Jednou `IDataProtector` instance je k dispozici, volejte `IDataProtector.ToTimeLimitedDataProtector` metoda rozšíření a vraťte se zpět ochranného zařízení s možnostmi předdefinované vypršení platnosti.

`ITimeLimitedDataProtector` poskytuje následující metody rozšíření a prostor pro rozhraní API:

* CreateProtector (účel řetězec): ITimeLimitedDataProtector - toto rozhraní API je podobná existující `IDataProtectionProvider.CreateProtector` v tom, že může sloužit k vytvoření [účel řetězy](xref:security/data-protection/consumer-apis/purpose-strings) z ochranného kořenové časově omezené.

* Chránit (byte [] ve formátu prostého textu, vypršení platnosti DateTimeOffset): byte]

* Chránit (byte [] ve formátu prostého textu, časový interval životního cyklu): byte]

* Chránit (byte [] ve formátu prostého textu): byte]

* Chránit (řetězec ve formátu prostého textu, vypršení platnosti DateTimeOffset): řetězec

* Chránit (řetězec ve formátu prostého textu, časový interval životního cyklu): řetězec

* Chránit (prostý text řetězec): řetězec

Kromě základních `Protect` metody, které trvat jenom jako prostý text, existují nové přetížení, které umožňují zadat datum vypršení platnosti datové části. Datum vypršení platnosti lze zadat jako absolutní datum (prostřednictvím `DateTimeOffset`) nebo jako relativní časové (ze systému aktuální čas, prostřednictvím `TimeSpan`). Pokud přetížení, které neberou vypršení platnosti je volána, předpokládá se nikdy do vypršení platnosti datové části.

* Odemknout (byte [] protectedData, se vypršení platnosti DateTimeOffset): byte]

* Odemknout (byte [] protectedData): byte]

* Odemknout (se vypršení platnosti DateTimeOffset protectedData řetězec): řetězec

* Odemknout (protectedData řetězec): řetězec

`Unprotect` Metody vrátit původní nechráněný data. Pokud ještě nevypršela platnost datové části, absolutní vypršení platnosti je vrácena jako volitelný vnější parametr spolu s původní nechráněný daty. Pokud vypršela platnost datové části, vyvolá výjimku všechny přetížení metody Unprotect cryptographicexception –.

>[!WARNING]
> Má není doporučeno používat tato rozhraní API k ochraně datových částí, které vyžadují dlouhodobé nebo neomezené trvalost. "Můžete I dovolit pro chráněné datové části být trvale neopravitelné po měsíci?" může sloužit jako dobré pravidlo; Pokud je odpověď žádné pak vývojáři měli zvážit alternativní rozhraní API.

Ukázka níže používá [cesty bez DI kódu](xref:security/data-protection/configuration/non-di-scenarios) pro vytvoření instance dat ochrany systému. Pokud chcete tuto ukázku spustit, ujistěte se, že jste přidali nejprve odkaz na balíček Microsoft.AspNetCore.DataProtection.Extensions.

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
