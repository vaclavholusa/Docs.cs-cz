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
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Jak na:] Vlastnost Reponse.Filter použít k nahrazení HTML na stránce technologie ASP.NET
====================
podle [PEL Jan](https://twitter.com/chrispels)

V této video PEL Jan ukazuje, jak používat vlastnost Reponse.Filter zachytí a alter odesílány na stránku HTML. Ukázkové stránky se nejprve vytvoří s jednoduchý text. Potom vlastní Stream – třída se vytvoří, který slouží jako datový proud nahrazení pro aktuální datový proud odesílaný do prohlížeče uživatele. V této třídě vlastní datový proud obsahu stránce se načítají z datového proudu, změnit a pak zapisují do datového proudu odpovědi. V této třídě vlastního datového proudu metody zápisu upravit tak, aby nahradit HTML v základní datového proudu odpovědi, a tím změnit, co je odeslán do prohlížeče uživatele. Nakonec je přiřazena nová třída datový proud na stránce změnit vlastnost Response.Filter\_načíst události, a poskytuje mechanismus pro změnu obsahu stránce.

[&#9654; Podívejte se na video (13 minuty)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
