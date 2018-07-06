---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[Postup:] Volba metody AJAX aktualizace stránky? | Dokumentace Microsoftu'
author: JoeStagner
description: V tomto videu porovnává Joe Stagner dvou základních způsobech provedení aktualizace stránky stylu AJAX v aplikaci technologie ASP.NET. Prvním způsobem je použít Upd...
ms.author: aspnetcontent
ms.date: 07/09/2007
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: 226fb0423ea05ad9034c909037358331918f2892
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838436"
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="ad5e8-105">[Postup:] Volba metody AJAX aktualizace stránky?</span><span class="sxs-lookup"><span data-stu-id="ad5e8-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>
====================
<span data-ttu-id="ad5e8-106">podle [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="ad5e8-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="ad5e8-107">V tomto videu porovnává Joe Stagner dvou základních způsobech provedení aktualizace stránky stylu AJAX v aplikaci technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ad5e8-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="ad5e8-108">První způsob je pomocí ovládacího prvku UpdatePanel, kdy žádný další kód musí být napsána na straně klienta nebo na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="ad5e8-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="ad5e8-109">Výhodou použití UpdatePanel je, že vše funguje automaticky.</span><span class="sxs-lookup"><span data-stu-id="ad5e8-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="ad5e8-110">Sankce je, že u klienta vyžaduje velké množství dat, které mají být zahrnuty v AJAX žádostí a odpovědí a na serveru vyžaduje životní cyklus celou stránku má být proveden.</span><span class="sxs-lookup"><span data-stu-id="ad5e8-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="ad5e8-111">Druhý způsob je použít zpětná volání sítě, kde další kód musí být napsána na straně klienta i na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="ad5e8-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="ad5e8-112">Výhodou použití zpětných volání sítě je, že u klienta vyžaduje velmi málo dat mají být zahrnuty v odpovědi a časový limit požadavku AJAX, a na serveru vyžaduje pouze metody volané služby má být proveden.</span><span class="sxs-lookup"><span data-stu-id="ad5e8-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="ad5e8-113">Penality je čas a úsilí, které bude trvat zápis nezbytný kód.</span><span class="sxs-lookup"><span data-stu-id="ad5e8-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="ad5e8-114">Joe, uzavře video diskuze o co byste měli zvážit při výběru mezi dvě základní metody aktualizace stránky AJAX-style.</span><span class="sxs-lookup"><span data-stu-id="ad5e8-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="ad5e8-115">(Toto video souboru používá kód z [Jak můžu začít pracovat s ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) videa a [co a jak vytvořit síťová zpětná na straně klienta pomocí ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) videa.)</span><span class="sxs-lookup"><span data-stu-id="ad5e8-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="ad5e8-116">&#9654;Podívejte se na video (11 minut)</span><span class="sxs-lookup"><span data-stu-id="ad5e8-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> <span data-ttu-id="ad5e8-117">[Předchozí](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [další](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="ad5e8-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
