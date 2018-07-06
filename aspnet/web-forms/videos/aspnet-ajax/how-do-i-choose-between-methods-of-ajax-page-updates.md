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
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a>[Postup:] Volba metody AJAX aktualizace stránky?
====================
podle [Joe Stagner](https://github.com/JoeStagner)

V tomto videu porovnává Joe Stagner dvou základních způsobech provedení aktualizace stránky stylu AJAX v aplikaci technologie ASP.NET. První způsob je pomocí ovládacího prvku UpdatePanel, kdy žádný další kód musí být napsána na straně klienta nebo na straně serveru. Výhodou použití UpdatePanel je, že vše funguje automaticky. Sankce je, že u klienta vyžaduje velké množství dat, které mají být zahrnuty v AJAX žádostí a odpovědí a na serveru vyžaduje životní cyklus celou stránku má být proveden. Druhý způsob je použít zpětná volání sítě, kde další kód musí být napsána na straně klienta i na straně serveru. Výhodou použití zpětných volání sítě je, že u klienta vyžaduje velmi málo dat mají být zahrnuty v odpovědi a časový limit požadavku AJAX, a na serveru vyžaduje pouze metody volané služby má být proveden. Penality je čas a úsilí, které bude trvat zápis nezbytný kód. Joe, uzavře video diskuze o co byste měli zvážit při výběru mezi dvě základní metody aktualizace stránky AJAX-style. (Toto video souboru používá kód z [Jak můžu začít pracovat s ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) videa a [co a jak vytvořit síťová zpětná na straně klienta pomocí ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) videa.)

[&#9654;Podívejte se na video (11 minut)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> [Předchozí](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [další](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)
