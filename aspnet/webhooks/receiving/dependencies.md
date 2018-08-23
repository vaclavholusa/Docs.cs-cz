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
# <a name="aspnet-webhooks-receiver-dependencies"></a>Závislosti přijímače ASP.NET – Webhooky

Microsoft ASP.NET WebHooks je navržená s injektáž závislostí v paměti. Většina závislosti v systému můžete nahradit alternativní implementace pomocí modulu vkládání závislostí.

Podrobnosti najdete na [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) seznam závislostí příjemce. Pokud byl zaregistrován žádné závislosti, použije se výchozí implementaci. Podrobnosti najdete na [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) seznam výchozí implementace.
