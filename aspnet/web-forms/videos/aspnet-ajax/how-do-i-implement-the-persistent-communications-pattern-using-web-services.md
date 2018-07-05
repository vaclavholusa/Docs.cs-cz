---
uid: web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
title: '[Postup:] Implementace vzorce trvalé komunikace pomocí webových služeb? | Dokumentace Microsoftu'
author: JoeStagner
description: Tradiční webu v prohlížeči a serverem nemají probíhající komunikaci, ale komunikovat pouze v reakci na uživatel provádějící úkon...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/22/2007
ms.topic: article
ms.assetid: 424c06cd-6d61-43cd-a1f2-d1a6b62e47b1
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
msc.type: video
ms.openlocfilehash: 8d7aac37b3b5b47f0533454f2d1d6f1f8677af99
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367471"
---
<a name="how-do-i-implement-the-persistent-communications-pattern-using-web-services"></a><span data-ttu-id="16ca9-104">[Postup:] Implementace vzorce trvalé komunikace pomocí webových služeb?</span><span class="sxs-lookup"><span data-stu-id="16ca9-104">[How Do I:] Implement the Persistent Communications Pattern using Web Services?</span></span>
====================
<span data-ttu-id="16ca9-105">podle [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="16ca9-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="16ca9-106">Tradiční webu v prohlížeči a serverem nemají probíhající komunikace, ale komunikovat pouze v reakci na uživatel provést akci.</span><span class="sxs-lookup"><span data-stu-id="16ca9-106">In a traditional Web site the browser and the server do not maintain an ongoing communication, but communicate only in response to the user performing an action.</span></span> <span data-ttu-id="16ca9-107">Moderní webu kde stránce stane kontejner aplikace může být výhodné pro prohlížeče a serveru pro uchování probíhající komunikace, aby mohla probíhat aktualizace stránky bez uživatele provést akci.</span><span class="sxs-lookup"><span data-stu-id="16ca9-107">In a modern Web site where the page becomes an application container, it can be advantageous for the browser and the server to maintain an ongoing communication so that page updates can occur without the user performing an action.</span></span> <span data-ttu-id="16ca9-108">To se označuje jako vzorce trvalé komunikace pro AJAX.</span><span class="sxs-lookup"><span data-stu-id="16ca9-108">This is known as the Persistent Communications Pattern for AJAX.</span></span> <span data-ttu-id="16ca9-109">ASP.NET AJAX obsahuje dva hlavní způsoby pro webové vývojáře implementace vzorce trvalé komunikace.</span><span class="sxs-lookup"><span data-stu-id="16ca9-109">ASP.NET AJAX provides two main ways for Web developers to implement the Persistent Communications Pattern.</span></span> <span data-ttu-id="16ca9-110">V předchozích videu jsme viděli, jak použít jako základ pro implementaci ovládacího prvku UpdatePanel technologie ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="16ca9-110">In an earlier video we saw how to use the ASP.NET AJAX UpdatePanel as the basis of the implementation.</span></span> <span data-ttu-id="16ca9-111">V tomto videu jsme dozvíte jak se implementují stejné vzorce pomocí volání JavaScrpt webová služba, která eliminuje potřebu prvku UpdatePanel technologie ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="16ca9-111">In this video we learn how to implement the same pattern using a JavaScrpt call to a Web service, which removes the need for an ASP.NET AJAX UpdatePanel.</span></span>

[<span data-ttu-id="16ca9-112">&#9654;Podívejte se na video (16 minut)</span><span class="sxs-lookup"><span data-stu-id="16ca9-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-the-persistent-communications-pattern-using-web-services)

> [!div class="step-by-step"]
> <span data-ttu-id="16ca9-113">[Předchozí](how-do-i-localize-an-aspnet-ajax-application.md)
> [další](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span><span class="sxs-lookup"><span data-stu-id="16ca9-113">[Previous](how-do-i-localize-an-aspnet-ajax-application.md)
[Next](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span></span>
