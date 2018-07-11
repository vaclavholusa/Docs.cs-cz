---
uid: webhooks/receiving/dependencies
title: Závislosti přijímače ASP.NET – Webhooky | Dokumentace Microsoftu
author: rick-anderson
description: Závislosti přijímače a injektáž závislostí v ASP.NET – Webhooky.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 05dcfac121e7974fd83c5b3736616479574944a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817834"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="f6d37-103">Závislosti přijímače ASP.NET – Webhooky</span><span class="sxs-lookup"><span data-stu-id="f6d37-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="f6d37-104">Microsoft ASP.NET WebHooks je navržená s injektáž závislostí v paměti.</span><span class="sxs-lookup"><span data-stu-id="f6d37-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="f6d37-105">Většina závislosti v systému můžete nahradit alternativní implementace pomocí modulu vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="f6d37-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="f6d37-106">Podrobnosti najdete na [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) seznam závislostí příjemce.</span><span class="sxs-lookup"><span data-stu-id="f6d37-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="f6d37-107">Pokud byl zaregistrován žádné závislosti, použije se výchozí implementaci.</span><span class="sxs-lookup"><span data-stu-id="f6d37-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="f6d37-108">Podrobnosti najdete na [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) seznam výchozí implementace.</span><span class="sxs-lookup"><span data-stu-id="f6d37-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
