---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Postup:] Řízení ukládání stránky ASP.NET do mezipaměti na základě vlastních informací | Dokumentace Microsoftu'
author: rick-anderson
description: V toto video pixelů na Chris ukazuje, jak k řízení kritérií pro ukládání do mezipaměti na základě vlastních informací stránky ASP.NET. Vytvoří ukázkovou stránku a pak O...
ms.author: aspnetcontent
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: d069b7798d3659e9f6786fb8d63862817fbdd68b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808691"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a>[Postup:] Řízení ukládání stránky ASP.NET do mezipaměti na základě vlastních informací
====================
podle [Chris pixelů na](https://twitter.com/chrispels)

V toto video pixelů na Chris ukazuje, jak k řízení kritérií pro ukládání do mezipaměti na základě vlastních informací stránky ASP.NET. Vytvoří ukázkovou stránku a pak direktivy OutputCache se používá s atributem VaryByCustom, který obsahuje vlastní hodnotu. V dalším kroku je přepsána metoda GetVaryCustomByString() v souboru global.asax modul, který poskytuje zpracování vlastního atributu. V této metodě je vrácen řetězec, který jednoznačně identifikuje verzi v mezipaměti na stránce. Nakonec je diskuse o ukládání do mezipaměti pomocí vlastní hodnoty použití několika způsoby pro webový server.

[&#9654;Podívejte se na video (12 minut)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
