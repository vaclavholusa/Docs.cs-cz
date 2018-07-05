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
# <a name="aspnet-webhooks-receiver-dependencies"></a>Závislosti přijímače ASP.NET – Webhooky

Microsoft ASP.NET WebHooks je navržená s injektáž závislostí v paměti. Většina závislosti v systému můžete nahradit alternativní implementace pomocí modulu vkládání závislostí.

Podrobnosti najdete na [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) seznam závislostí příjemce. Pokud byl zaregistrován žádné závislosti, použije se výchozí implementaci. Podrobnosti najdete na [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) seznam výchozí implementace.
