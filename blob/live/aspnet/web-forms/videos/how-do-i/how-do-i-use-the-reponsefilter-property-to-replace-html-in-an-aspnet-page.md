---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: "[Jak na:] Vlastnost Reponse.Filter použít k nahrazení HTML na stránce technologie ASP.NET | Microsoft Docs"
author: rick-anderson
description: "V této video PEL Jan ukazuje, jak používat vlastnost Reponse.Filter zachytí a alter odesílány na stránku HTML. Ukázkové stránky se nejprve vytvoří w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="8a0fc-104">[Jak na:] Vlastnost Reponse.Filter použít k nahrazení HTML na stránce technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8a0fc-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="8a0fc-105">podle [PEL Jan](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="8a0fc-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="8a0fc-106">V této video PEL Jan ukazuje, jak používat vlastnost Reponse.Filter zachytí a alter odesílány na stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="8a0fc-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="8a0fc-107">Ukázkové stránky se nejprve vytvoří s jednoduchý text.</span><span class="sxs-lookup"><span data-stu-id="8a0fc-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="8a0fc-108">Potom vlastní Stream – třída se vytvoří, který slouží jako datový proud nahrazení pro aktuální datový proud odesílaný do prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="8a0fc-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="8a0fc-109">V této třídě vlastní datový proud obsahu stránce se načítají z datového proudu, změnit a pak zapisují do datového proudu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8a0fc-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="8a0fc-110">V této třídě vlastního datového proudu metody zápisu upravit tak, aby nahradit HTML v základní datového proudu odpovědi, a tím změnit, co je odeslán do prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="8a0fc-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="8a0fc-111">Nakonec je přiřazena nová třída datový proud na stránce změnit vlastnost Response.Filter\_načíst události, a poskytuje mechanismus pro změnu obsahu stránce.</span><span class="sxs-lookup"><span data-stu-id="8a0fc-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="8a0fc-112">&#9654; Podívejte se na video (13 minuty)</span><span class="sxs-lookup"><span data-stu-id="8a0fc-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
