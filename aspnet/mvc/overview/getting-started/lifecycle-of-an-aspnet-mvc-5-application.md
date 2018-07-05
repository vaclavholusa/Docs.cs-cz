---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Životní cyklus aplikace ASP.NET MVC 5 | Dokumentace Microsoftu
author: cephalin
description: Stáhněte si dokument PDF, který grafy životní cyklus aplikace ASP.NET MVC 5. Tento dokument životní cyklus poskytuje podrobný pohled MVC životního cyklu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 98735f2e04bdd0f2fec19524e59f6272dbc4ca57
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393912"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="04ed0-104">Životní cyklus aplikace ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="04ed0-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="04ed0-105">podle [Cephas dků](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="04ed0-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="04ed0-106">Stáhněte si dokument PDF</span><span class="sxs-lookup"><span data-stu-id="04ed0-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="04ed0-107">Tady si můžete stáhnout soubor PDF dokument, který grafy životního cyklu každou aplikaci ASP.NET MVC 5, příjem HTTP žádost o odeslání odpovědi HTTP zpátky do klienta.</span><span class="sxs-lookup"><span data-stu-id="04ed0-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="04ed0-108">Je navržená jako vzdělávací nástroj pro ty, kteří začínají s ASP.NET MVC a také jako referenční informace pro uživatele, kteří potřebují k podrobnostem specifických aspektů aplikace.</span><span class="sxs-lookup"><span data-stu-id="04ed0-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="04ed0-109">Dokument PDF má následující funkce:</span><span class="sxs-lookup"><span data-stu-id="04ed0-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="04ed0-110">Relevantní [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) fází, které vám pomohou pochopit, kde MVC integruje do [životního cyklu aplikací ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).</span><span class="sxs-lookup"><span data-stu-id="04ed0-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="04ed0-111">Souhrnný přehled životního cyklu aplikace MVC, které vám umožní pochopit hlavní fáze, které prochází každá aplikace MVC v kanálu zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="04ed0-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="04ed0-112">Zobrazení podrobností, která zobrazuje cvičení dolů do podrobností v kanálu zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="04ed0-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="04ed0-113">Můžete porovnat vyšší úroveň zobrazení a zobrazení podrobností, abyste viděli, jak podrobnosti životního cyklu se shromáždí do různých fází.</span><span class="sxs-lookup"><span data-stu-id="04ed0-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="04ed0-114">[Stáhněte si PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) větší zobrazení.</span><span class="sxs-lookup"><span data-stu-id="04ed0-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="04ed0-115">Umístění a účel všechny přepisovatelné metody v [řadič](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) objektu v kanálu zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="04ed0-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="04ed0-116">Může nebo nemusí být potřeba přepsat všechny jedna metoda, ale je důležité porozumět jejich role v životního cyklu aplikací, takže můžete psát kód ve fázi odpovídající životní cyklus pro efekt, který chcete.</span><span class="sxs-lookup"><span data-stu-id="04ed0-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="04ed0-117">Úžasné nahoru diagramy zobrazující, jak je vyvolán jednotlivých typů filtrů (ověřování, autorizace, akce a výsledku).</span><span class="sxs-lookup"><span data-stu-id="04ed0-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="04ed0-118">Odkaz na článek užitečný nebo blogu z každého bodu zájmu v zobrazení podrobností.</span><span class="sxs-lookup"><span data-stu-id="04ed0-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="04ed0-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04ed0-119">Next Steps</span></span>

<span data-ttu-id="04ed0-120">Splňuje tento dokument vašim požadavkům?</span><span class="sxs-lookup"><span data-stu-id="04ed0-120">Does this document meet your need?</span></span> <span data-ttu-id="04ed0-121">Uvítáme vaše zpětná vazba.</span><span class="sxs-lookup"><span data-stu-id="04ed0-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="04ed0-122">Pokud máte na každou otázku na životní cyklus ASP.NET MVC ve vaší aplikaci [Stackoverflow](http://stackoverflow.com/help) a [fóra ASP.NET MVC](https://forums.asp.net/1146.aspx) jsou užitečná místa, kde dotaz.</span><span class="sxs-lookup"><span data-stu-id="04ed0-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="04ed0-123">Postupujte podle [mě](https://twitter.com/Cephas_MSFT) na twitteru, můžete získat aktualizace na můj poslední kurzy.</span><span class="sxs-lookup"><span data-stu-id="04ed0-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
