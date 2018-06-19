---
uid: webhooks/receiving/dependencies
title: Závislosti příjemce Webhooky ASP.NET | Microsoft Docs
author: rick-anderson
description: Příjemce závislosti a vkládání závislostí v Webhooky ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573277"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Závislosti příjemce Webhooky ASP.NET

Microsoft ASP.NET WebHooks je navržen s vkládání závislostí v paměti. Většina závislosti v systému můžete nahradit alternativní implementace pomocí modulu vkládání závislostí.

Najdete v tématu [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) seznam příjemce závislosti. Pokud byl zaregistrován žádné závislosti, použije se výchozí implementace. Najdete v tématu [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) seznam výchozí implementace.
