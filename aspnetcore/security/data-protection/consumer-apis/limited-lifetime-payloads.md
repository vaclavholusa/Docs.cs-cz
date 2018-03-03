---
title: "Omezení délky trvání chráněných datových částí"
author: rick-anderson
description: "Tento dokument vysvětluje, jak omezit životnost chráněné datové části pomocí funkce Ochrana dat ASP.NET Core rozhraní API."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: d631851b5b933d75c37a308f492840e3442e6f1a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="03b7f-103">Omezení délky trvání chráněných datových částí</span><span class="sxs-lookup"><span data-stu-id="03b7f-103">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="03b7f-104">Existují scénáře, kdy vývojář aplikace chce vytvořit chráněné datové části, jejíž platnost vyprší po nastaveném časovém období.</span><span class="sxs-lookup"><span data-stu-id="03b7f-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="03b7f-105">Například může chráněné datové části představují token pro resetování hesla, která má být platné pouze pro jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="03b7f-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="03b7f-106">To je určitě možné pro vývojáře k vytvoření vlastní datovou část formátu, který obsahuje datum vypršení platnosti embedded a pokročilé vývojáře může Chcete přesto provést, ale pro většinu vývojáři Správa těchto vypršení můžou růst zdlouhavé.</span><span class="sxs-lookup"><span data-stu-id="03b7f-106">It's certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="03b7f-107">Pro zjednodušení pro naše cílová skupina vývojáře balíček [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) obsahuje nástroj rozhraní API pro vytváření datových částí, které automaticky vyprší po nastaveném časovém období.</span><span class="sxs-lookup"><span data-stu-id="03b7f-107">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="03b7f-108">Tato rozhraní API přečnívat odhlásit z `ITimeLimitedDataProtector` typu.</span><span class="sxs-lookup"><span data-stu-id="03b7f-108">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="03b7f-109">Využití rozhraní API</span><span class="sxs-lookup"><span data-stu-id="03b7f-109">API usage</span></span>

<span data-ttu-id="03b7f-110">`ITimeLimitedDataProtector` Rozhraní je základní rozhraní pro ochranu a při rušení časově omezené / samoobslužné zneplatněním obsažených datových částí.</span><span class="sxs-lookup"><span data-stu-id="03b7f-110">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="03b7f-111">K vytvoření instance `ITimeLimitedDataProtector`, je nutné nejdříve instanci běžný [IDataProtector](overview.md) sestavený s konkrétním účelem.</span><span class="sxs-lookup"><span data-stu-id="03b7f-111">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="03b7f-112">Jednou `IDataProtector` instance je k dispozici, volejte `IDataProtector.ToTimeLimitedDataProtector` metoda rozšíření a vraťte se zpět ochranného zařízení s možnostmi předdefinované vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="03b7f-112">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="03b7f-113">`ITimeLimitedDataProtector` poskytuje následující metody rozšíření a prostor pro rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="03b7f-113">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="03b7f-114">CreateProtector (účel řetězec): ITimeLimitedDataProtector - toto rozhraní API je podobná existující `IDataProtectionProvider.CreateProtector` v tom, že může sloužit k vytvoření [účel řetězy](purpose-strings.md) z ochranného kořenové časově omezené.</span><span class="sxs-lookup"><span data-stu-id="03b7f-114">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="03b7f-115">Chránit (byte [] ve formátu prostého textu, vypršení platnosti DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="03b7f-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="03b7f-116">Chránit (byte [] ve formátu prostého textu, časový interval životního cyklu): byte]</span><span class="sxs-lookup"><span data-stu-id="03b7f-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="03b7f-117">Chránit (byte [] ve formátu prostého textu): byte]</span><span class="sxs-lookup"><span data-stu-id="03b7f-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="03b7f-118">Chránit (řetězec ve formátu prostého textu, vypršení platnosti DateTimeOffset): řetězec</span><span class="sxs-lookup"><span data-stu-id="03b7f-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="03b7f-119">Chránit (řetězec ve formátu prostého textu, časový interval životního cyklu): řetězec</span><span class="sxs-lookup"><span data-stu-id="03b7f-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="03b7f-120">Chránit (prostý text řetězec): řetězec</span><span class="sxs-lookup"><span data-stu-id="03b7f-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="03b7f-121">Kromě základních `Protect` metody, které trvat jenom jako prostý text, existují nové přetížení, které umožňují zadat datum vypršení platnosti datové části.</span><span class="sxs-lookup"><span data-stu-id="03b7f-121">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="03b7f-122">Datum vypršení platnosti lze zadat jako absolutní datum (prostřednictvím `DateTimeOffset`) nebo jako relativní časové (ze systému aktuální čas, prostřednictvím `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="03b7f-122">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="03b7f-123">Pokud přetížení, které neberou vypršení platnosti je volána, předpokládá se nikdy do vypršení platnosti datové části.</span><span class="sxs-lookup"><span data-stu-id="03b7f-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="03b7f-124">Odemknout (byte [] protectedData, se vypršení platnosti DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="03b7f-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="03b7f-125">Odemknout (byte [] protectedData): byte]</span><span class="sxs-lookup"><span data-stu-id="03b7f-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="03b7f-126">Odemknout (se vypršení platnosti DateTimeOffset protectedData řetězec): řetězec</span><span class="sxs-lookup"><span data-stu-id="03b7f-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="03b7f-127">Odemknout (protectedData řetězec): řetězec</span><span class="sxs-lookup"><span data-stu-id="03b7f-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="03b7f-128">`Unprotect` Metody vrátit původní nechráněný data.</span><span class="sxs-lookup"><span data-stu-id="03b7f-128">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="03b7f-129">Pokud ještě nevypršela platnost datové části, absolutní vypršení platnosti je vrácena jako volitelný vnější parametr spolu s původní nechráněný daty.</span><span class="sxs-lookup"><span data-stu-id="03b7f-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="03b7f-130">Pokud vypršela platnost datové části, vyvolá výjimku všechny přetížení metody Unprotect cryptographicexception –.</span><span class="sxs-lookup"><span data-stu-id="03b7f-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="03b7f-131">Má není doporučeno používat tato rozhraní API k ochraně datových částí, které vyžadují dlouhodobé nebo neomezené trvalost.</span><span class="sxs-lookup"><span data-stu-id="03b7f-131">It's not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="03b7f-132">"Můžete I dovolit pro chráněné datové části být trvale neopravitelné po měsíci?"</span><span class="sxs-lookup"><span data-stu-id="03b7f-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="03b7f-133">může sloužit jako dobré pravidlo; Pokud je odpověď žádné pak vývojáři měli zvážit alternativní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="03b7f-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="03b7f-134">Ukázka níže používá [cesty bez DI kódu](../configuration/non-di-scenarios.md) pro vytvoření instance dat ochrany systému.</span><span class="sxs-lookup"><span data-stu-id="03b7f-134">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="03b7f-135">Pokud chcete tuto ukázku spustit, ujistěte se, že jste přidali nejprve odkaz na balíček Microsoft.AspNetCore.DataProtection.Extensions.</span><span class="sxs-lookup"><span data-stu-id="03b7f-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
