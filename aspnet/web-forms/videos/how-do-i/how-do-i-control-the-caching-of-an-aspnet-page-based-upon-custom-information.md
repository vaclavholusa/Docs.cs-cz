---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: "[Jak na:] Řízení ukládání do mezipaměti pro stránku ASP.NET na základě vlastních informací | Microsoft Docs"
author: rick-anderson
description: "V této video PEL Jan ukazuje, jak k řízení kritérií pro ukládání do mezipaměti na základě vlastních informací stránky ASP.NET. Vytvoření ukázkové stránky a pak e...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: b785de4e1161ae82ee458148aee4b30820801147
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a>[Jak na:] Řízení ukládání do mezipaměti pro stránku ASP.NET na základě vlastních informací
====================
podle [PEL Jan](https://twitter.com/chrispels)

V této video PEL Jan ukazuje, jak k řízení kritérií pro ukládání do mezipaměti na základě vlastních informací stránky ASP.NET. Vytvoření ukázkové stránky a pak používá direktivy OutputCache s atributem VaryByCustom, který obsahuje vlastní hodnotu. Dále je v souboru global.asax modul, který poskytuje zpracování vlastního atributu přepsat metodu GetVaryCustomByString(). V této metodě je vrácen řetězec, který jednoznačně identifikuje uložené v mezipaměti verzi stránky. Nakonec je diskuse o ukládání do mezipaměti pomocí vlastní hodnotu použití několika způsoby pro webový server.

[&#9654; Podívejte se na video (12 minuty)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
