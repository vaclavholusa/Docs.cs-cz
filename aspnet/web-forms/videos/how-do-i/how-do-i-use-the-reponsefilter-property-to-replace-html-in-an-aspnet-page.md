---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Postup:] Použití vlastnosti Reponse.Filter k nahrazení kódu HTML na stránce ASP.NET | Dokumentace Microsoftu'
author: rick-anderson
description: V pixelů na toto video Chris ukazuje způsob použití vlastnosti Reponse.Filter zachytí a alter odesílané do stránku HTML. Ukázkovou stránku se nejprve vytvoří w...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 0e8ae8b62bbb4ac1fc7e942cf3dd0438383cb510
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754809"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Postup:] Použití vlastnosti Reponse.Filter k nahrazení kódu HTML na stránce ASP.NET
====================
podle [Chris pixelů na](https://twitter.com/chrispels)

V pixelů na toto video Chris ukazuje způsob použití vlastnosti Reponse.Filter zachytí a alter odesílané do stránku HTML. Nejprve se vytvoří ukázkovou stránku s jednoduchý text. Vlastní třída Stream pak, je vytvořen, který slouží jako datový proud nahrazení pro aktuální datový proud odesílané do prohlížeče uživatele. V této třídě vlastní datový proud obsah stránky se načítají z datového proudu, změnit a potom zapsané do proudu odpovědi. V této třídě vlastní Stream metody zápisu upravit tak, aby nahrazení kódu HTML do základního datového proudu odpovědi, změny a co se odesílá do webového prohlížeče. Nakonec je přiřazena nová třída datového proudu na stránce změnit vlastnost Response.Filter\_načíst událost, a tím, poskytuje mechanismus pro změnu obsahu stránky.

[&#9654;Podívejte se na video (13 min)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
