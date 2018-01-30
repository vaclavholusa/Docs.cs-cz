---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: "[Jak na:]  Mezipaměť stránky ASP.NET na základě informací v hlavičce HTTP | Microsoft Docs"
author: rick-anderson
description: "V této video PEL Jan ukazuje, jak udržovat stránky ve výstupní mezipaměti technologie ASP.NET na základě informací v hlavičce protokolu HTTP na stránku. První, potenciální nadpisů HTTP..."
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
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="97066-104">[Jak na:]  Mezipaměť stránky ASP.NET na základě informací v hlavičce protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="97066-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>
====================
<span data-ttu-id="97066-105">podle [PEL Jan](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="97066-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="97066-106">V této video PEL Jan ukazuje, jak udržovat stránky ve výstupní mezipaměti technologie ASP.NET na základě informací v hlavičce protokolu HTTP na stránku.</span><span class="sxs-lookup"><span data-stu-id="97066-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="97066-107">Nejprve jsou kontrolovány potenciální hodnoty hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="97066-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="97066-108">Poté vytvoření ukázkové stránky a pak direktivy OutputCache používá s VaryByHeader atribut, který obsahuje hodnotu "přijmout jazyk", záhlaví HTTP, k řízení ukládání do mezipaměti podle jazyka prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="97066-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="97066-109">Ukázkové stránky je zobrazit v aplikaci Internet Explorer, který je nastaven na angličtinu a pak v FireFox, který je nastavený na použití francouzštinu.</span><span class="sxs-lookup"><span data-stu-id="97066-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="97066-110">Nakonec je popsána možnost přesunout definici mezipaměti do CacheProfile v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="97066-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="97066-111">&#9654; Podívejte se na video (12 minuty)</span><span class="sxs-lookup"><span data-stu-id="97066-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
