---
title: "Začínáme s rozhraními API ochrany dat."
author: rick-anderson
description: "Tento dokument vysvětluje, jak používat ochranu dat ASP.NET Core rozhraní API pro ochranu a při rušení dat v aplikaci."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/using-data-protection
ms.openlocfilehash: e8c4183f5c47d8ffec8edf163eb1e4d6f2757d9d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="fcd69-103">Začínáme s rozhraními API ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="fcd69-103">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="fcd69-104">Ve své nejjednodušší, ochranu dat se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="fcd69-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="fcd69-105">Vytvoření ochrany dat z zprostředkovatele ochrany data.</span><span class="sxs-lookup"><span data-stu-id="fcd69-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="fcd69-106">Volání `Protect` metoda s daty, které chcete chránit.</span><span class="sxs-lookup"><span data-stu-id="fcd69-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="fcd69-107">Volání `Unprotect` metoda s daty, které chcete zapnout zpět do prostého textu.</span><span class="sxs-lookup"><span data-stu-id="fcd69-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="fcd69-108">Většina architektury a modely aplikace, jako je například technologie ASP.NET nebo SignalR, už konfiguraci systému ochrany dat a přidat do kontejneru služby, ke kterému přistupujete pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="fcd69-108">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="fcd69-109">Následující příklad ukazuje konfiguraci služby kontejneru pro vkládání závislostí a registrace zásobníku ochrany dat, přijetí zprostředkovatel ochrany dat prostřednictvím DI, vytvoření ochranu a ochranu pak Probíhá rušení ochrany dat</span><span class="sxs-lookup"><span data-stu-id="fcd69-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="fcd69-110">Když vytvoříte ochranného zařízení je nutné zadat jednu nebo více [účel řetězce](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="fcd69-110">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="fcd69-111">Řetězec účel zajišťuje izolaci mezi příjemci.</span><span class="sxs-lookup"><span data-stu-id="fcd69-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="fcd69-112">Například ochranného zařízení vytvořené pomocí účel řetězec "green" nebude moct zrušení ochrany dat poskytované ochranného zařízení s účelem "Fialová".</span><span class="sxs-lookup"><span data-stu-id="fcd69-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="fcd69-113">Instance `IDataProtectionProvider` a `IDataProtector` jsou bezpečné pro přístup z více vláken pro více volající.</span><span class="sxs-lookup"><span data-stu-id="fcd69-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="fcd69-114">Rozhraní má který určeno po získá odkaz na komponentu `IDataProtector` prostřednictvím volání `CreateProtector`, tento odkaz se bude používat pro několik volání `Protect` a `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="fcd69-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="fcd69-115">Volání `Unprotect` cryptographicexception – vyvolá výjimku, pokud nelze ověřit nebo dešifrovat znalosti chráněné datové části.</span><span class="sxs-lookup"><span data-stu-id="fcd69-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="fcd69-116">Některé součásti chtít ignorování chyb během zrušení operace; komponenta, která čte ověřovací soubory cookie může zpracovat tuto chybu a považovat požadavku, jako kdyby měl žádný soubor cookie na všech než nesplní žádost přímý.</span><span class="sxs-lookup"><span data-stu-id="fcd69-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="fcd69-117">Součásti, které chcete toto chování má konkrétně catch cryptographicexception – místo požití všechny výjimky.</span><span class="sxs-lookup"><span data-stu-id="fcd69-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
