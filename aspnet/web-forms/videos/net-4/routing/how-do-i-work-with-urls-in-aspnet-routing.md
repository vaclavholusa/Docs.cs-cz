---
uid: web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
title: 'Jak na: práce s adresami URL ve směrování ASP.NET? | Microsoft Docs'
author: rick-anderson
description: V tomto videu Jan PEL ukazuje, jak k určení adres URL webu, který využívá směrování ASP.NET. Nejprve webová stránka je vytvořena a směrování je definována do hlavní knihy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2010
ms.topic: article
ms.assetid: 08f9d0a7-cfa0-4914-a672-8a64295d7ba8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
msc.type: video
ms.openlocfilehash: 3d87db9589dc5d330a29b3a25546dc65234f5f41
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30893882"
---
<a name="how-do-i-work-with-urls-in-aspnet-routing"></a><span data-ttu-id="0d363-105">Jak na: práce s adresami URL ve směrování ASP.NET?</span><span class="sxs-lookup"><span data-stu-id="0d363-105">How Do I: Work with URLs in ASP.NET Routing?</span></span>
====================
<span data-ttu-id="0d363-106">podle [PEL Jan](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="0d363-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="0d363-107">V tomto videu Jan PEL ukazuje, jak k určení adres URL webu, který využívá směrování ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0d363-107">In this video Chris Pels shows how to specify URLs in a web site that utilizes ASP.NET routing.</span></span> <span data-ttu-id="0d363-108">Nejprve webová stránka je vytvořena a směrování je definována v globální třídy aplikace (.asax).</span><span class="sxs-lookup"><span data-stu-id="0d363-108">First, a web site is created and routing is defined in the Global Application Class (.asax).</span></span> <span data-ttu-id="0d363-109">V dalším kroku vytvoření ukázkové webové stránky a adresu URL na základě s definovanou trasou se přidá na stránku pomocí standardní "pevně zakódovanou" přístup, například "~/Stats/Visitors".</span><span class="sxs-lookup"><span data-stu-id="0d363-109">Next, a sample web page is created and a URL based upon a defined route is added to the page using the standard "hard coded" approach, e.g., "~/Stats/Visitors".</span></span> <span data-ttu-id="0d363-110">Jiné spojení se pak přidá na stránku, která se dynamicky vygeneruje stejnou adresu URL v kódu pomocí RouteValue metodu, která přijímá názvu trasy a parametry.</span><span class="sxs-lookup"><span data-stu-id="0d363-110">Another link is then added to the page which dynamically generates the same URL in markup using the RouteValue method which accepts the route name and parameters.</span></span> <span data-ttu-id="0d363-111">Stejná adresa URL je pak implementovaná pomocí kódu místo značek přímo na stránce.</span><span class="sxs-lookup"><span data-stu-id="0d363-111">The same URL is then implemented using code rather than markup directly in the page.</span></span> <span data-ttu-id="0d363-112">Původní trasy a fyzickou stránku umístění se poté změní, výsledkem je naprogramováno odkaz už práci, zatímco dynamicky generované odkazy funkce správně.</span><span class="sxs-lookup"><span data-stu-id="0d363-112">The original route and physical page location are then changed, resulting in the hard coded link no longer working whereas both dynamically generated links function properly.</span></span> <span data-ttu-id="0d363-113">Nakonec je pak popsané hodnotu dynamicky generovaném odkazy.</span><span class="sxs-lookup"><span data-stu-id="0d363-113">Finally, the value of dynamically generated links is then discussed.</span></span>

[<span data-ttu-id="0d363-114">&#9654;Podívejte se na video (20 minut)</span><span class="sxs-lookup"><span data-stu-id="0d363-114">&#9654; Watch video (20 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-work-with-urls-in-aspnet-routing)

> [!div class="step-by-step"]
> [<span data-ttu-id="0d363-115">Předchozí</span><span class="sxs-lookup"><span data-stu-id="0d363-115">Previous</span></span>](how-do-i-use-routing-with-aspnet-web-forms.md)
