---
uid: webhooks/receiving/dependencies
title: "Závislosti příjemce Webhooky ASP.NET | Microsoft Docs"
author: rick-anderson
description: "Příjemce závislosti a vkládání závislostí v Webhooky ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="833b8-103">Závislosti příjemce Webhooky ASP.NET</span><span class="sxs-lookup"><span data-stu-id="833b8-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="833b8-104">Microsoft ASP.NET WebHooks je navržen s vkládání závislostí v paměti.</span><span class="sxs-lookup"><span data-stu-id="833b8-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="833b8-105">Většina závislosti v systému můžete nahradit alternativní implementace pomocí modulu vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="833b8-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="833b8-106">Najdete v tématu [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) seznam příjemce závislosti.</span><span class="sxs-lookup"><span data-stu-id="833b8-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="833b8-107">Pokud byl zaregistrován žádné závislosti, použije se výchozí implementace.</span><span class="sxs-lookup"><span data-stu-id="833b8-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="833b8-108">Najdete v tématu [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) seznam výchozí implementace.</span><span class="sxs-lookup"><span data-stu-id="833b8-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
