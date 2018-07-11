---
uid: web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
title: 'Postup: práce s adresami URL při směrování ASP.NET? | Dokumentace Microsoftu'
author: rick-anderson
description: Chris pixelů na toto video ukazuje, jak určení adres URL webu, který využívá směrování ASP.NET. Nejprve webová stránka je vytvořena a směrování je definována do hlavní knihy...
ms.author: aspnetcontent
ms.date: 10/15/2010
ms.assetid: 08f9d0a7-cfa0-4914-a672-8a64295d7ba8
msc.legacyurl: /web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
msc.type: video
ms.openlocfilehash: 12b1b0bc41365b480c0104c1a1b00fa5ef515137
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818550"
---
<a name="how-do-i-work-with-urls-in-aspnet-routing"></a>Postup: práce s adresami URL při směrování ASP.NET?
====================
podle [Chris pixelů na](https://twitter.com/chrispels)

Chris pixelů na toto video ukazuje, jak určení adres URL webu, který využívá směrování ASP.NET. Nejprve se vytvoří webovou stránku a směrování je definován v globální třída aplikace (asax). V dalším kroku se vytvoří ukázkovou webovou stránku a adresu URL na základě s definovanou trasou se přidá na stránku pomocí standardní "pevně zakódovanou" přístup, například "~/Stats/Visitors". Další odkaz se pak přidá na stránku, která se dynamicky vygeneruje stejnou adresu URL v kódu pomocí RouteValue metodu, která přijímá názvu trasy a parametry. Stejná adresa URL je následně implementované pomocí kódu namísto kódu přímo na stránce. Pak změnil původní trasy a umístění fyzické stránky, výsledkem je pevně zakódovaný odkaz už práce vzhledem k tomu, jak dynamicky generované odkazy funkce správně. Nakonec jsou popsány pak hodnotou dynamicky generované odkazy.

[&#9654;Podívejte se na video (20 minut)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-work-with-urls-in-aspnet-routing)

> [!div class="step-by-step"]
> [Předchozí](how-do-i-use-routing-with-aspnet-web-forms.md)
