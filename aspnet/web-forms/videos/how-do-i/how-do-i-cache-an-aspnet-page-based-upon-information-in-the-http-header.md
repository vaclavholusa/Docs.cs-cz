---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[Jak na:]  Mezipaměť stránky ASP.NET na základě informací v hlavičce HTTP | Microsoft Docs'
author: rick-anderson
description: V této video PEL Jan ukazuje, jak udržovat stránky ve výstupní mezipaměti technologie ASP.NET na základě informací v hlavičce protokolu HTTP na stránku. První, potenciální nadpisů HTTP...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2009
ms.topic: article
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: ce5ea10396d0fe31d72425e2431102a0cb0c3bd0
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
ms.locfileid: "28882214"
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a>[Jak na:]  Mezipaměť stránky ASP.NET na základě informací v hlavičce protokolu HTTP
====================
podle [PEL Jan](https://twitter.com/chrispels)

V této video PEL Jan ukazuje, jak udržovat stránky ve výstupní mezipaměti technologie ASP.NET na základě informací v hlavičce protokolu HTTP na stránku. Nejprve jsou kontrolovány potenciální hodnoty hlavičky protokolu HTTP. Poté vytvoření ukázkové stránky a pak direktivy OutputCache používá s VaryByHeader atribut, který obsahuje hodnotu "přijmout jazyk", záhlaví HTTP, k řízení ukládání do mezipaměti podle jazyka prohlížeče uživatele. Ukázkové stránky je zobrazit v aplikaci Internet Explorer, který je nastaven na angličtinu a pak v FireFox, který je nastavený na použití francouzštinu. Nakonec je popsána možnost přesunout definici mezipaměti do CacheProfile v souboru web.config.

[&#9654; Podívejte se na video (12 minuty)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
