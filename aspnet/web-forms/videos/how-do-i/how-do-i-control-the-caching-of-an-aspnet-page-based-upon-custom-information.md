---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Jak na:] Řízení ukládání do mezipaměti pro stránku ASP.NET na základě vlastních informací | Microsoft Docs'
author: rick-anderson
description: V této video PEL Jan ukazuje, jak k řízení kritérií pro ukládání do mezipaměti na základě vlastních informací stránky ASP.NET. Vytvoření ukázkové stránky a pak e....
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
ms.locfileid: "26572743"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="b176d-104">[Jak na:] Řízení ukládání do mezipaměti pro stránku ASP.NET na základě vlastních informací</span><span class="sxs-lookup"><span data-stu-id="b176d-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="b176d-105">podle [PEL Jan](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="b176d-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="b176d-106">V této video PEL Jan ukazuje, jak k řízení kritérií pro ukládání do mezipaměti na základě vlastních informací stránky ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b176d-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="b176d-107">Vytvoření ukázkové stránky a pak používá direktivy OutputCache s atributem VaryByCustom, který obsahuje vlastní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b176d-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="b176d-108">Dále je v souboru global.asax modul, který poskytuje zpracování vlastního atributu přepsat metodu GetVaryCustomByString().</span><span class="sxs-lookup"><span data-stu-id="b176d-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="b176d-109">V této metodě je vrácen řetězec, který jednoznačně identifikuje uložené v mezipaměti verzi stránky.</span><span class="sxs-lookup"><span data-stu-id="b176d-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="b176d-110">Nakonec je diskuse o ukládání do mezipaměti pomocí vlastní hodnotu použití několika způsoby pro webový server.</span><span class="sxs-lookup"><span data-stu-id="b176d-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="b176d-111">&#9654; Podívejte se na video (12 minuty)</span><span class="sxs-lookup"><span data-stu-id="b176d-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
