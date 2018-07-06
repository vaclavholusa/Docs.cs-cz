---
uid: web-forms/videos/how-do-i/how-do-i-send-email-asynchronously-with-aspnet
title: '[Postup:] Odeslání e-mailu asynchronně s technologií ASP.NET | Dokumentace Microsoftu'
author: rick-anderson
description: V tomto videu pixelů na Chris ukazuje způsob použití třídy System.Net.Mail v technologii ASP.NET odesílat asynchronní e-mailovou zprávu. Nejdříve si projděte postup konfigurace web si...
ms.author: aspnetcontent
ms.date: 09/24/2008
ms.assetid: 77a5c8fa-ebb2-426d-b56b-a5a98a46b516
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-send-email-asynchronously-with-aspnet
msc.type: video
ms.openlocfilehash: e1405a82117a079dbdca72911b85b742c916262c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830040"
---
<a name="how-do-i-send-email-asynchronously-with-aspnet"></a>[Postup:] Odeslání e-mailu asynchronně pomocí technologie ASP.NET
====================
podle [Chris pixelů na](https://twitter.com/chrispels)

V tomto videu pixelů na Chris ukazuje způsob použití třídy System.Net.Mail v technologii ASP.NET odesílat asynchronní e-mailovou zprávu. Nejdříve si projděte postup konfigurace webové stránky k odesílání e-mailům prostřednictvím &lt;mailSettings&gt; element v souboru web.config. Dále vytvořte jednoduché uživatelské rozhraní pro zadávání informací o e-mailu. Naučíte se vytvářet pomocí třídy MailMessage vytvořit e-mailovou zprávu v kódu stránky. Jako součást tohoto procesu vytvořte obslužnou rutinu události pro asynchronní zpětné volání po odeslání e-mailu. Obslužná rutina události zjistit, jak použít instanci třídy AsynchCompletedEventArgs, který poskytuje informace o procesu odeslání e-mailu. Nakonec odeslat testovací e-mail asynchronně, proveďte kroky v režimu ladění a zobrazení skutečné e-maily přijaté z procesu.

[&#9654;Podívejte se na video (18 minut)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-send-email-asynchronously-with-aspnet)
