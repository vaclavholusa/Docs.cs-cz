---
title: "Začínáme s rozhraními API ochrany dat."
author: rick-anderson
description: "Tento dokument vysvětluje, jak používat ochranu dat ASP.NET Core rozhraní API pro ochranu a při rušení dat v aplikaci."
keywords: ASP.NET Core, ochrany dat
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 535bfaf2077cda91c27e7d0d68b9804e8596070e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="8ebb0-104">Začínáme s rozhraními API ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="8ebb0-104">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="8ebb0-105">Ve své nejjednodušší, ochranu dat se skládá z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="8ebb0-105">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="8ebb0-106">Vytvoření ochrany dat z zprostředkovatele ochrany data.</span><span class="sxs-lookup"><span data-stu-id="8ebb0-106">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="8ebb0-107">Volání `Protect` metoda s daty, které chcete chránit.</span><span class="sxs-lookup"><span data-stu-id="8ebb0-107">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="8ebb0-108">Volání `Unprotect` metoda s daty, které chcete zapnout zpět do prostého textu.</span><span class="sxs-lookup"><span data-stu-id="8ebb0-108">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="8ebb0-109">Většina architektury a modely aplikace, jako je například technologie ASP.NET nebo SignalR, už konfiguraci systému ochrany dat a přidat do kontejneru služby, ke kterému přistupujete pomocí vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="8ebb0-109">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="8ebb0-110">Následující příklad ukazuje konfiguraci služby kontejneru pro vkládání závislostí a registrace zásobníku ochrany dat, přijetí zprostředkovatel ochrany dat prostřednictvím DI, vytvoření ochranu a ochranu pak Probíhá rušení ochrany dat</span><span class="sxs-lookup"><span data-stu-id="8ebb0-110">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="8ebb0-111">Když vytvoříte ochranného zařízení je nutné zadat jednu nebo více [účel řetězce](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="8ebb0-111">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="8ebb0-112">Řetězec účel zajišťuje izolaci mezi příjemci.</span><span class="sxs-lookup"><span data-stu-id="8ebb0-112">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="8ebb0-113">Například ochranného zařízení vytvořené pomocí účel řetězec "green" nebude moct zrušení ochrany dat poskytované ochranného zařízení s účelem "Fialová".</span><span class="sxs-lookup"><span data-stu-id="8ebb0-113">For example, a protector created with a purpose string of "green" would not be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="8ebb0-114">Instance `IDataProtectionProvider` a `IDataProtector` jsou bezpečné pro přístup z více vláken pro více volající.</span><span class="sxs-lookup"><span data-stu-id="8ebb0-114">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="8ebb0-115">Je určena, jakmile získá odkaz na komponentu `IDataProtector` prostřednictvím volání `CreateProtector`, tento odkaz se bude používat pro několik volání `Protect` a `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="8ebb0-115">It is intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="8ebb0-116">Volání `Unprotect` cryptographicexception – vyvolá výjimku, pokud nelze ověřit nebo dešifrovat znalosti chráněné datové části.</span><span class="sxs-lookup"><span data-stu-id="8ebb0-116">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="8ebb0-117">Některé součásti chtít ignorování chyb během zrušení operace; komponenta, která čte ověřovací soubory cookie může zpracovat tuto chybu a považovat požadavku, jako kdyby měl žádný soubor cookie na všech než nesplní žádost přímý.</span><span class="sxs-lookup"><span data-stu-id="8ebb0-117">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="8ebb0-118">Součásti, které chcete toto chování má konkrétně catch cryptographicexception – místo požití všechny výjimky.</span><span class="sxs-lookup"><span data-stu-id="8ebb0-118">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
