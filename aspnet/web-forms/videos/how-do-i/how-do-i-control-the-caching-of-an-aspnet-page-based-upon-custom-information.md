---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Postup:] Řízení ukládání stránky ASP.NET do mezipaměti na základě vlastních informací | Dokumentace Microsoftu'
author: rick-anderson
description: V toto video pixelů na Chris ukazuje, jak k řízení kritérií pro ukládání do mezipaměti na základě vlastních informací stránky ASP.NET. Vytvoří ukázkovou stránku a pak O...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: d2c8e2384d39255f66c11f1cc303398750229779
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376017"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="ca94d-104">[Postup:] Řízení ukládání stránky ASP.NET do mezipaměti na základě vlastních informací</span><span class="sxs-lookup"><span data-stu-id="ca94d-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="ca94d-105">podle [Chris pixelů na](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="ca94d-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="ca94d-106">V toto video pixelů na Chris ukazuje, jak k řízení kritérií pro ukládání do mezipaměti na základě vlastních informací stránky ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ca94d-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="ca94d-107">Vytvoří ukázkovou stránku a pak direktivy OutputCache se používá s atributem VaryByCustom, který obsahuje vlastní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ca94d-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="ca94d-108">V dalším kroku je přepsána metoda GetVaryCustomByString() v souboru global.asax modul, který poskytuje zpracování vlastního atributu.</span><span class="sxs-lookup"><span data-stu-id="ca94d-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="ca94d-109">V této metodě je vrácen řetězec, který jednoznačně identifikuje verzi v mezipaměti na stránce.</span><span class="sxs-lookup"><span data-stu-id="ca94d-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="ca94d-110">Nakonec je diskuse o ukládání do mezipaměti pomocí vlastní hodnoty použití několika způsoby pro webový server.</span><span class="sxs-lookup"><span data-stu-id="ca94d-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="ca94d-111">&#9654;Podívejte se na video (12 minut)</span><span class="sxs-lookup"><span data-stu-id="ca94d-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
