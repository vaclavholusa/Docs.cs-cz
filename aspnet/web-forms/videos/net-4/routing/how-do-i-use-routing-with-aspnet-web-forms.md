---
uid: web-forms/videos/net-4/routing/how-do-i-use-routing-with-aspnet-web-forms
title: 'Jak na: použití směrování s webovými formuláři ASP.NET? | Microsoft Docs'
author: rick-anderson
description: V tomto videu Jan PEL ukazuje, jak implementovat směrování pro webové formuláře ASP.NET 4. Nejprve koncept směrování adresu URL se porovná s mapování adresy URL na p...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2010
ms.topic: article
ms.assetid: a3ab6cd9-8f71-4b73-9336-21c0de078269
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/net-4/routing/how-do-i-use-routing-with-aspnet-web-forms
msc.type: video
ms.openlocfilehash: b268bd628d3b0108d783023cd30d21b43c1b3758
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="how-do-i-use-routing-with-aspnet-web-forms"></a><span data-ttu-id="0c645-105">Jak na: použití směrování s webovými formuláři ASP.NET?</span><span class="sxs-lookup"><span data-stu-id="0c645-105">How Do I: Use Routing with ASP.NET Web Forms?</span></span>
====================
<span data-ttu-id="0c645-106">podle [PEL Jan](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="0c645-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="0c645-107">V tomto videu Jan PEL ukazuje, jak implementovat směrování pro webové formuláře ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="0c645-107">In this video Chris Pels shows how to implement routing for Web Forms in ASP.NET 4.</span></span> <span data-ttu-id="0c645-108">Nejprve koncept směrování adresu URL se porovná s mapování adresy URL na fyzický soubor v lokalitě.</span><span class="sxs-lookup"><span data-stu-id="0c645-108">First, the concept of routing a URL is compared to mapping the URL to a physical file in the site.</span></span> <span data-ttu-id="0c645-109">Potom ukázka trasy pro adresu URL je definována v soubor global.asax souboru aplikace\_obslužné rutiny události spuštění.</span><span class="sxs-lookup"><span data-stu-id="0c645-109">Then, a sample route for a URL is defined in the global.asax file Application\_Start event handler.</span></span> <span data-ttu-id="0c645-110">Trasy, která obsahuje parametrizované hodnotu, která může uživatel zadat v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="0c645-110">The route contains a parameterized value that the user can enter in the URL.</span></span> <span data-ttu-id="0c645-111">Ukázkové stránky je poté jste vytvořili a hodnota parametru trasy, které je extrahován na stránce\_obslužné rutiny události zatížení.</span><span class="sxs-lookup"><span data-stu-id="0c645-111">A sample page is then created and the route parameter value is extracted in the Page\_Load event handler.</span></span> <span data-ttu-id="0c645-112">V dalším kroku trasu druhý je definován s více parametrů a trasy na stejné stránce jako počáteční trasy.</span><span class="sxs-lookup"><span data-stu-id="0c645-112">Next, a second route is defined that has multiple parameters and routes to the same page as the initial route.</span></span> <span data-ttu-id="0c645-113">Stránce\_obslužné rutiny události zatížení je rozšířena extrahovat hodnotu parametru další trasy a zobrazit různé informace podle toho, jaké hodnoty byly předány na stránku.</span><span class="sxs-lookup"><span data-stu-id="0c645-113">The Page\_Load event handler is expanded to extract the additional route parameter value and display different information depending upon what values have been passed to the page.</span></span>

[<span data-ttu-id="0c645-114">&#9654;Podívejte se na video (15 minut)</span><span class="sxs-lookup"><span data-stu-id="0c645-114">&#9654; Watch video (15 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-routing-with-aspnet-web-forms)

> [!div class="step-by-step"]
> <span data-ttu-id="0c645-115">[Předchozí](aspnet-4-quick-hit-outbound-webforms-routing.md)
> [další](how-do-i-work-with-urls-in-aspnet-routing.md)</span><span class="sxs-lookup"><span data-stu-id="0c645-115">[Previous](aspnet-4-quick-hit-outbound-webforms-routing.md)
[Next](how-do-i-work-with-urls-in-aspnet-routing.md)</span></span>
