---
uid: web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
title: 'Postup: práce s adresami URL při směrování ASP.NET? | Dokumentace Microsoftu'
author: rick-anderson
description: Chris pixelů na toto video ukazuje, jak určení adres URL webu, který využívá směrování ASP.NET. Nejprve webová stránka je vytvořena a směrování je definována do hlavní knihy...
ms.author: aspnetcontent
ms.date: 10/15/2010
ms.assetid: 08f9d0a7-cfa0-4914-a672-8a64295d7ba8
msc.legacyurl: /web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
msc.type: video
ms.openlocfilehash: 12b1b0bc41365b480c0104c1a1b00fa5ef515137
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818550"
---
<a name="how-do-i-work-with-urls-in-aspnet-routing"></a><span data-ttu-id="6a81b-105">Postup: práce s adresami URL při směrování ASP.NET?</span><span class="sxs-lookup"><span data-stu-id="6a81b-105">How Do I: Work with URLs in ASP.NET Routing?</span></span>
====================
<span data-ttu-id="6a81b-106">podle [Chris pixelů na](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="6a81b-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="6a81b-107">Chris pixelů na toto video ukazuje, jak určení adres URL webu, který využívá směrování ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6a81b-107">In this video Chris Pels shows how to specify URLs in a web site that utilizes ASP.NET routing.</span></span> <span data-ttu-id="6a81b-108">Nejprve se vytvoří webovou stránku a směrování je definován v globální třída aplikace (asax).</span><span class="sxs-lookup"><span data-stu-id="6a81b-108">First, a web site is created and routing is defined in the Global Application Class (.asax).</span></span> <span data-ttu-id="6a81b-109">V dalším kroku se vytvoří ukázkovou webovou stránku a adresu URL na základě s definovanou trasou se přidá na stránku pomocí standardní "pevně zakódovanou" přístup, například "~/Stats/Visitors".</span><span class="sxs-lookup"><span data-stu-id="6a81b-109">Next, a sample web page is created and a URL based upon a defined route is added to the page using the standard "hard coded" approach, e.g., "~/Stats/Visitors".</span></span> <span data-ttu-id="6a81b-110">Další odkaz se pak přidá na stránku, která se dynamicky vygeneruje stejnou adresu URL v kódu pomocí RouteValue metodu, která přijímá názvu trasy a parametry.</span><span class="sxs-lookup"><span data-stu-id="6a81b-110">Another link is then added to the page which dynamically generates the same URL in markup using the RouteValue method which accepts the route name and parameters.</span></span> <span data-ttu-id="6a81b-111">Stejná adresa URL je následně implementované pomocí kódu namísto kódu přímo na stránce.</span><span class="sxs-lookup"><span data-stu-id="6a81b-111">The same URL is then implemented using code rather than markup directly in the page.</span></span> <span data-ttu-id="6a81b-112">Pak změnil původní trasy a umístění fyzické stránky, výsledkem je pevně zakódovaný odkaz už práce vzhledem k tomu, jak dynamicky generované odkazy funkce správně.</span><span class="sxs-lookup"><span data-stu-id="6a81b-112">The original route and physical page location are then changed, resulting in the hard coded link no longer working whereas both dynamically generated links function properly.</span></span> <span data-ttu-id="6a81b-113">Nakonec jsou popsány pak hodnotou dynamicky generované odkazy.</span><span class="sxs-lookup"><span data-stu-id="6a81b-113">Finally, the value of dynamically generated links is then discussed.</span></span>

[<span data-ttu-id="6a81b-114">&#9654;Podívejte se na video (20 minut)</span><span class="sxs-lookup"><span data-stu-id="6a81b-114">&#9654; Watch video (20 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-work-with-urls-in-aspnet-routing)

> [!div class="step-by-step"]
> [<span data-ttu-id="6a81b-115">Předchozí</span><span class="sxs-lookup"><span data-stu-id="6a81b-115">Previous</span></span>](how-do-i-use-routing-with-aspnet-web-forms.md)
