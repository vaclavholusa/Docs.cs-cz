---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Životní cyklus aplikace ASP.NET MVC 5 | Microsoft Docs
author: cephalin
description: Stáhněte si dokument PDF, který grafy životního cyklu aplikace ASP.NET MVC 5. Tento dokument životního cyklu poskytuje podrobný pohled životního cyklu MVC...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 50d58d10c11677fa72ede6a03e686cbde4cbae1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036489"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="4aaa4-104">Životní cyklus aplikace ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="4aaa4-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="4aaa4-105">podle [Cephas odkazů](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="4aaa4-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="4aaa4-106">Stáhněte si dokument PDF</span><span class="sxs-lookup"><span data-stu-id="4aaa4-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="4aaa4-107">Zde si můžete stáhnout dokument PDF, který grafy životní cyklus každá aplikace ASP.NET MVC 5, příjem HTTP žádost o odeslání odpovědi HTTP zpět do klienta.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="4aaa4-108">Je určený jako výukových nástroj pro ty, kteří jsou nové do architektury ASP.NET MVC i taky jako odkaz pro ty, kteří potřebují přejdete do konkrétních aspektů aplikace.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="4aaa4-109">Dokument PDF má následující funkce:</span><span class="sxs-lookup"><span data-stu-id="4aaa4-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="4aaa4-110">Relevantní [třídě HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) tak, aby vám pomohou pochopit, kde MVC integruje do [životního cyklu aplikace ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).</span><span class="sxs-lookup"><span data-stu-id="4aaa4-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="4aaa4-111">Široký přehled o životním cyklu aplikace MVC, kde můžete porozumět hlavních fází, které každá aplikace MVC projdou v kanálu zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="4aaa4-112">Zobrazení podrobností, které obsahuje projde dolů na podrobné informace o kanálu zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="4aaa4-113">Souhrnné zobrazení a zobrazení podrobností, které chcete zobrazit, jak se shromažďují životní cykly podrobnosti do různých fází, můžete porovnat.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="4aaa4-114">[Stáhnout PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) zobrazíte větší zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="4aaa4-115">Umístění a účel všechny přepisovatelné metody v [řadič](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) objektu v kanálu zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="4aaa4-116">Může nebo nemusí mít potřebu potlačí všechny jednu metodu, ale je důležité porozumět jejich role v průběhu životního cyklu aplikací, takže můžete napsat kód ve fázi odpovídající životní cyklus účinky, které chcete.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="4aaa4-117">Diagramy napěněnou up znázorňující, jak každý typ filtru (ověřování, autorizace, akci a výsledek) je vyvolána.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="4aaa4-118">Propojit užitečné článku nebo blog z každého bodu zájem o podrobné zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="4aaa4-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4aaa4-119">Next Steps</span></span>

<span data-ttu-id="4aaa4-120">Splňuje tento dokument vašim potřebám?</span><span class="sxs-lookup"><span data-stu-id="4aaa4-120">Does this document meet your need?</span></span> <span data-ttu-id="4aaa4-121">Uvítáme vaše názory.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="4aaa4-122">Pokud všechny otázky, které máte na rozhraní ASP.NET MVC životního cyklu ve vaší aplikaci, [Stackoverflow](http://stackoverflow.com/help) a [ASP.NET MVC fóra](https://forums.asp.net/1146.aspx) jsou skvělý místech požádat.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="4aaa4-123">Postupujte podle [mi](https://twitter.com/Cephas_MSFT) na twitteru, abyste získali aktualizace na můj nejnovější kurzy.</span><span class="sxs-lookup"><span data-stu-id="4aaa4-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
