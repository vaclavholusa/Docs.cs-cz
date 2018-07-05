---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Postup:] Použití vlastnosti Reponse.Filter k nahrazení kódu HTML na stránce ASP.NET | Dokumentace Microsoftu'
author: rick-anderson
description: V pixelů na toto video Chris ukazuje způsob použití vlastnosti Reponse.Filter zachytí a alter odesílané do stránku HTML. Ukázkovou stránku se nejprve vytvoří w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: f871a3969da21a22bfe10e3a85f43db3e0e522e8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382358"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="e0974-104">[Postup:] Použití vlastnosti Reponse.Filter k nahrazení kódu HTML na stránce ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e0974-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="e0974-105">podle [Chris pixelů na](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="e0974-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="e0974-106">V pixelů na toto video Chris ukazuje způsob použití vlastnosti Reponse.Filter zachytí a alter odesílané do stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="e0974-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="e0974-107">Nejprve se vytvoří ukázkovou stránku s jednoduchý text.</span><span class="sxs-lookup"><span data-stu-id="e0974-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="e0974-108">Vlastní třída Stream pak, je vytvořen, který slouží jako datový proud nahrazení pro aktuální datový proud odesílané do prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="e0974-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="e0974-109">V této třídě vlastní datový proud obsah stránky se načítají z datového proudu, změnit a potom zapsané do proudu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e0974-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="e0974-110">V této třídě vlastní Stream metody zápisu upravit tak, aby nahrazení kódu HTML do základního datového proudu odpovědi, změny a co se odesílá do webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e0974-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="e0974-111">Nakonec je přiřazena nová třída datového proudu na stránce změnit vlastnost Response.Filter\_načíst událost, a tím, poskytuje mechanismus pro změnu obsahu stránky.</span><span class="sxs-lookup"><span data-stu-id="e0974-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="e0974-112">&#9654;Podívejte se na video (13 min)</span><span class="sxs-lookup"><span data-stu-id="e0974-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
