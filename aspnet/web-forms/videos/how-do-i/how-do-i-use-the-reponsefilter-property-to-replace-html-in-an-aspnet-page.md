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
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Postup:] Použití vlastnosti Reponse.Filter k nahrazení kódu HTML na stránce ASP.NET
====================
podle [Chris pixelů na](https://twitter.com/chrispels)

V pixelů na toto video Chris ukazuje způsob použití vlastnosti Reponse.Filter zachytí a alter odesílané do stránku HTML. Nejprve se vytvoří ukázkovou stránku s jednoduchý text. Vlastní třída Stream pak, je vytvořen, který slouží jako datový proud nahrazení pro aktuální datový proud odesílané do prohlížeče uživatele. V této třídě vlastní datový proud obsah stránky se načítají z datového proudu, změnit a potom zapsané do proudu odpovědi. V této třídě vlastní Stream metody zápisu upravit tak, aby nahrazení kódu HTML do základního datového proudu odpovědi, změny a co se odesílá do webového prohlížeče. Nakonec je přiřazena nová třída datového proudu na stránce změnit vlastnost Response.Filter\_načíst událost, a tím, poskytuje mechanismus pro změnu obsahu stránky.

[&#9654;Podívejte se na video (13 min)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
