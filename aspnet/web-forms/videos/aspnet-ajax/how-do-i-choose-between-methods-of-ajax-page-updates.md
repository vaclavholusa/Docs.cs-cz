---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: "[Jak na:] Zvolit metody AJAX stránky aktualizace? | Microsoft Docs"
author: JoeStagner
description: "V tomto videu Jan Stagner porovná dvě základní metody provedení aktualizací stránky stylu AJAX v aplikaci ASP.NET. První metoda se má používat Upd..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2007
ms.topic: article
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: cc3721c24c9fed0cb028d755330a5c6189b613b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="cbaed-105">[Jak na:] Zvolit metody AJAX stránky aktualizace?</span><span class="sxs-lookup"><span data-stu-id="cbaed-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>
====================
<span data-ttu-id="cbaed-106">podle [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="cbaed-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="cbaed-107">V tomto videu Jan Stagner porovná dvě základní metody provedení aktualizací stránky stylu AJAX v aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cbaed-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="cbaed-108">První metoda se má používat UpdatePanel, kde je potřeba žádné další kód zapsat na straně klienta nebo na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="cbaed-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="cbaed-109">Výhodou použití UpdatePanel je, že vše funguje automaticky.</span><span class="sxs-lookup"><span data-stu-id="cbaed-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="cbaed-110">Snížení je, že na straně klienta vyžaduje velké množství dat, které mají být zahrnuty do AJAX žádostí a odpovědí a na serveru vyžaduje cyklu celou stránku spouštění.</span><span class="sxs-lookup"><span data-stu-id="cbaed-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="cbaed-111">Druhý způsob je použití sítě zpětná volání, kde další kód potřeba zapsat na straně klienta a na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="cbaed-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="cbaed-112">Výhodou používání zpětných volání sítě je, že na straně klienta vyžaduje velmi malé data mají být zahrnuty do AJAX žádostí a odpovědí, a na serveru se vyžaduje jenom metody volané služby má být proveden.</span><span class="sxs-lookup"><span data-stu-id="cbaed-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="cbaed-113">Penality je čas a úsilí, potřebnou k zápisu nezbytného kódu.</span><span class="sxs-lookup"><span data-stu-id="cbaed-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="cbaed-114">Jan se ukončí video hovoříte o co byste měli zvážit při výběru mezi dvě základní metody stylu AJAX aktualizací stránky.</span><span class="sxs-lookup"><span data-stu-id="cbaed-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="cbaed-115">(Toto video používá kód z [Jak můžu začít pracovat s technologií ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video a [jak na to zkontrolujte zpětná klientské sítě volání pomocí prvku ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span><span class="sxs-lookup"><span data-stu-id="cbaed-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="cbaed-116">&#9654; Podívejte se na video (11 minuty)</span><span class="sxs-lookup"><span data-stu-id="cbaed-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

>[!div class="step-by-step"]
<span data-ttu-id="cbaed-117">[Předchozí](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[další](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="cbaed-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
