---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-use-httpverbs-attributes-in-an-mvc-application
title: 'Postup: použití atributů HttpVerbs v aplikaci MVC? | Dokumentace Microsoftu'
author: rick-anderson
description: V toto video pixelů na Chris ukazuje způsob použití atributů HttpVerbs pro řízení přístupu k akce MVC. Nejprve se vytvoří ukázkovou aplikaci s co výchozí...
ms.author: aspnetcontent
ms.date: 12/30/2009
ms.assetid: d2488a1d-0f3f-4994-8fbe-4f59b8c9503e
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-use-httpverbs-attributes-in-an-mvc-application
msc.type: video
ms.openlocfilehash: 2932480ba7e573e3e093ccfd69ac88e8e95df623
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809945"
---
<a name="how-do-i-use-httpverbs-attributes-in-an-mvc-application"></a><span data-ttu-id="d3297-105">Postup: použití atributů HttpVerbs v aplikaci MVC?</span><span class="sxs-lookup"><span data-stu-id="d3297-105">How Do I: Use HttpVerbs Attributes in an MVC Application?</span></span>
====================
<span data-ttu-id="d3297-106">podle [Chris pixelů na](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="d3297-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="d3297-107">V toto video pixelů na Chris ukazuje způsob použití atributů HttpVerbs pro řízení přístupu k akce MVC.</span><span class="sxs-lookup"><span data-stu-id="d3297-107">In this video Chris Pels shows how to use the HttpVerbs attributes to control access to MVC actions.</span></span> <span data-ttu-id="d3297-108">Ukázková aplikace nejprve se vytvoří s výchozí kontroler a zobrazení pro úpravy informací.</span><span class="sxs-lookup"><span data-stu-id="d3297-108">First, a sample application is created with a default controller and view for editing the information.</span></span> <span data-ttu-id="d3297-109">Druhou akci indexu v dalším kroku se přidá do kontroleru, který má atribut HttpPost, který jej omezuje na volána pouze v případě, že se používá HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="d3297-109">Next, a second Index action is added to the controller which has an HttpPost attribute which restricts it to being called only when an HTTP POST is used.</span></span> <span data-ttu-id="d3297-110">Jako následnou akci je atribut AcceptVerbs() implementovaný jako alternativní syntaxi pro sadu Visual Studio 2008.</span><span class="sxs-lookup"><span data-stu-id="d3297-110">As a follow-up, the AcceptVerbs() attribute is implemented as an alternative syntax for Visual Studio 2008.</span></span> <span data-ttu-id="d3297-111">Pak je popsána využívání HttpVerbs kvůli rizik zabezpečení spojených s použitím HTTP GET provést odstranění z odkazu.</span><span class="sxs-lookup"><span data-stu-id="d3297-111">A use of the HttpVerbs for preventing the security risk associated with using an HTTP GET to perform a delete from a link is then discussed.</span></span>

[<span data-ttu-id="d3297-112">&#9654;Podívejte se na video (16 minut)</span><span class="sxs-lookup"><span data-stu-id="d3297-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-httpverbs-attributes-in-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="d3297-113">[Předchozí](how-do-i-work-with-model-binders-in-an-mvc-application.md)
> [další](mvc2-html-encoding.md)</span><span class="sxs-lookup"><span data-stu-id="d3297-113">[Previous](how-do-i-work-with-model-binders-in-an-mvc-application.md)
[Next](mvc2-html-encoding.md)</span></span>
