---
uid: webhooks/receiving/dependencies
title: Závislosti přijímače ASP.NET – Webhooky | Dokumentace Microsoftu
author: rick-anderson
description: Závislosti přijímače a injektáž závislostí v ASP.NET – Webhooky.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.openlocfilehash: cf45e3d2e45e4b7882b42d9aa0931e18c08f76b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401180"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="08c50-103">Závislosti přijímače ASP.NET – Webhooky</span><span class="sxs-lookup"><span data-stu-id="08c50-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="08c50-104">Microsoft ASP.NET WebHooks je navržená s injektáž závislostí v paměti.</span><span class="sxs-lookup"><span data-stu-id="08c50-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="08c50-105">Většina závislosti v systému můžete nahradit alternativní implementace pomocí modulu vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="08c50-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="08c50-106">Podrobnosti najdete na [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) seznam závislostí příjemce.</span><span class="sxs-lookup"><span data-stu-id="08c50-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="08c50-107">Pokud byl zaregistrován žádné závislosti, použije se výchozí implementaci.</span><span class="sxs-lookup"><span data-stu-id="08c50-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="08c50-108">Podrobnosti najdete na [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) seznam výchozí implementace.</span><span class="sxs-lookup"><span data-stu-id="08c50-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
