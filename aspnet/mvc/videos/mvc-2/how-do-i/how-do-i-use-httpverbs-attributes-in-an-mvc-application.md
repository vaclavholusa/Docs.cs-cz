---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-use-httpverbs-attributes-in-an-mvc-application
title: 'Jak na: použití HttpVerbs atributy v aplikaci MVC? | Microsoft Docs'
author: rick-anderson
description: V této video PEL Jan ukazuje, jak použít HttpVerbs atributy pro řízení přístupu k akce MVC. Nejprve se vytvoří ukázkovou aplikaci s co výchozí...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2009
ms.topic: article
ms.assetid: d2488a1d-0f3f-4994-8fbe-4f59b8c9503e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-use-httpverbs-attributes-in-an-mvc-application
msc.type: video
ms.openlocfilehash: 42817e712db0489f277ac65250b9c0f102088c0a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="how-do-i-use-httpverbs-attributes-in-an-mvc-application"></a><span data-ttu-id="33431-105">Jak na: použití HttpVerbs atributy v aplikaci MVC?</span><span class="sxs-lookup"><span data-stu-id="33431-105">How Do I: Use HttpVerbs Attributes in an MVC Application?</span></span>
====================
<span data-ttu-id="33431-106">podle [PEL Jan](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="33431-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="33431-107">V této video PEL Jan ukazuje, jak použít HttpVerbs atributy pro řízení přístupu k akce MVC.</span><span class="sxs-lookup"><span data-stu-id="33431-107">In this video Chris Pels shows how to use the HttpVerbs attributes to control access to MVC actions.</span></span> <span data-ttu-id="33431-108">Ukázková aplikace se nejdřív vytvoří s výchozí řadiče a zobrazení pro úpravy informace.</span><span class="sxs-lookup"><span data-stu-id="33431-108">First, a sample application is created with a default controller and view for editing the information.</span></span> <span data-ttu-id="33431-109">Druhou akci indexu v dalším kroku se přidá do kontroleru, který má atribut HttpPost, který jej omezuje na volané jenom v případě, že se používá HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="33431-109">Next, a second Index action is added to the controller which has an HttpPost attribute which restricts it to being called only when an HTTP POST is used.</span></span> <span data-ttu-id="33431-110">Jako následné akce je atribut AcceptVerbs() implementovaný jako alternativní syntaxe pro Visual Studio 2008.</span><span class="sxs-lookup"><span data-stu-id="33431-110">As a follow-up, the AcceptVerbs() attribute is implemented as an alternative syntax for Visual Studio 2008.</span></span> <span data-ttu-id="33431-111">Pak je popsána použití HttpVerbs brání bezpečnostní riziko spojené s pomocí HTTP GET Pokud chcete provést odstranění z odkazu.</span><span class="sxs-lookup"><span data-stu-id="33431-111">A use of the HttpVerbs for preventing the security risk associated with using an HTTP GET to perform a delete from a link is then discussed.</span></span>

[<span data-ttu-id="33431-112">&#9654;Podívejte se na video (16 minuty)</span><span class="sxs-lookup"><span data-stu-id="33431-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-httpverbs-attributes-in-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="33431-113">[Předchozí](how-do-i-work-with-model-binders-in-an-mvc-application.md)
> [další](mvc2-html-encoding.md)</span><span class="sxs-lookup"><span data-stu-id="33431-113">[Previous](how-do-i-work-with-model-binders-in-an-mvc-application.md)
[Next](mvc2-html-encoding.md)</span></span>
