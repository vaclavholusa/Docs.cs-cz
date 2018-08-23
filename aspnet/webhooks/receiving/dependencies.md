---
uid: webhooks/receiving/dependencies
title: Závislosti přijímače ASP.NET – Webhooky | Dokumentace Microsoftu
author: rick-anderson
description: Závislosti přijímače a injektáž závislostí v ASP.NET – Webhooky.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754990"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="ec8f6-103">Závislosti přijímače ASP.NET – Webhooky</span><span class="sxs-lookup"><span data-stu-id="ec8f6-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="ec8f6-104">Microsoft ASP.NET WebHooks je navržená s injektáž závislostí v paměti.</span><span class="sxs-lookup"><span data-stu-id="ec8f6-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="ec8f6-105">Většina závislosti v systému můžete nahradit alternativní implementace pomocí modulu vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="ec8f6-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="ec8f6-106">Podrobnosti najdete na [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) seznam závislostí příjemce.</span><span class="sxs-lookup"><span data-stu-id="ec8f6-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="ec8f6-107">Pokud byl zaregistrován žádné závislosti, použije se výchozí implementaci.</span><span class="sxs-lookup"><span data-stu-id="ec8f6-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="ec8f6-108">Podrobnosti najdete na [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) seznam výchozí implementace.</span><span class="sxs-lookup"><span data-stu-id="ec8f6-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
