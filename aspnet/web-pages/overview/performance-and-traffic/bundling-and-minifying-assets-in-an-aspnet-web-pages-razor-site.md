---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Sdružování a Minifikace prostředky v rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs
author: microsoft
description: Sdružování a minimalizace způsoby rychleji vaší lokality. Sdružování umožňuje můžete kombinovat několik souborů JavaScript (.js) nebo více stylů CSS (...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: df00b5c9e741e429011143d04121df97c1930602
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26572869"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="84a9b-104">Sdružování a Minifikace prostředky v stránku ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="84a9b-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="84a9b-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="84a9b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="84a9b-106">Sdružování a minimalizace způsoby rychleji vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="84a9b-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="84a9b-107">Sdružování umožňuje kombinovat více JavaScript (*.js*) soubory nebo více stylů CSS (*.css*) souborů tak, aby si můžete stáhnout jako jednotku, nikoli po jednom.</span><span class="sxs-lookup"><span data-stu-id="84a9b-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="84a9b-108">Minimalizace Prohne out prázdné znaky a provádí jiné typy komprese aby stažené soubory jako malé možného.</span><span class="sxs-lookup"><span data-stu-id="84a9b-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="84a9b-109">RC verzi ASP.NET Web Pages 2 nepodporuje sdružování a minimalizace, protože balíček, který obsahuje požadované prvky ještě není k dispozici v Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="84a9b-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="84a9b-110">Omlouváme se za vzniklé potíže.</span><span class="sxs-lookup"><span data-stu-id="84a9b-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="84a9b-111">Balíček musí být k dispozici ve finální verzi ASP.NET Web Pages 2 a službě WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="84a9b-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
