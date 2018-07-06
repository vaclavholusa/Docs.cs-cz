---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Sdružování a Minifikace prostředků v ASP.NET Web Pages lokality (Razor) | Dokumentace Microsoftu
author: microsoft
description: Sdružování a minifikace jsou způsoby, jak urychlit vašeho webu. Sdružování umožňuje zkombinujete několik souborů JavaScriptu (.js) nebo více stylů CSS (...)
ms.author: aspnetcontent
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5799c4a3ae1c36636e0e485361f0f55e62ec7ec7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813436"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="6e04a-104">Sdružování a Minifikace prostředků na webu technologie ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="6e04a-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="6e04a-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6e04a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6e04a-106">Sdružování a minifikace jsou způsoby, jak urychlit vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="6e04a-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="6e04a-107">Sdružování umožňuje kombinovat více JavaScript (*js*) soubory nebo více stylů CSS (*.css*) soubory tak, aby jejich si můžete stáhnout jako celek, spíše než postupně po jednom.</span><span class="sxs-lookup"><span data-stu-id="6e04a-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="6e04a-108">Připravenost k minifikaci marží si prázdné znaky a provádí kompresi, aby stažené soubory jako malé možný výskyt jiné typy.</span><span class="sxs-lookup"><span data-stu-id="6e04a-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="6e04a-109">Verzi RC sady ASP.NET Web Pages 2 nepodporuje sdružování a minifikace, protože balíček, který obsahuje požadované prvky ještě není k dispozici v Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6e04a-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="6e04a-110">Omlouváme se za nepříjemnosti.</span><span class="sxs-lookup"><span data-stu-id="6e04a-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="6e04a-111">Balíček má být k dispozici ve finální verzi technologie ASP.NET Web Pages 2 a službě WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="6e04a-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
