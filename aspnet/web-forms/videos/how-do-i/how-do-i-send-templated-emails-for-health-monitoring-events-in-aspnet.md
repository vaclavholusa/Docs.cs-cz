---
uid: web-forms/videos/how-do-i/how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet
title: '[Postup:] Odeslání e-mailů bez vizuálního vzhledu pro událostech monitorování stavu v ASP.NET | Dokumentace Microsoftu'
author: rick-anderson
description: Chris pixelů na toto video ukazuje způsob použití TemplatedEmailWebEventProvider k odesílání e-mailů, když dojde k události monitorování stavu, které využívají šablonu pro t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2008
ms.topic: article
ms.assetid: 5c107c6e-9fb7-4206-bd3f-221cb0767f8a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet
msc.type: video
ms.openlocfilehash: 407a9bd1e38e474286e58e94f4d98d4302a02223
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378550"
---
<a name="how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet"></a>[Postup:] Odeslání e-mailů bez vizuálního vzhledu pro událostech monitorování stavu v ASP.NET
====================
podle [Chris pixelů na](https://twitter.com/chrispels)

Chris pixelů na toto video ukazuje způsob použití TemplatedEmailWebEventProvider k odesílání e-mailů, když dojde k události monitorování stavu, které využívají šablonu pro obsah e-mailu. Nejdříve si projděte postup konfigurace &lt;poskytovatele&gt; a &lt;pravidla&gt; elementy v souboru web.config implementovat pomocí e-mailu bez vizuálního vzhledu a přidružení stavu monitorování událostí s poskytovateli e-mailu bez vizuálního vzhledu. Po nakonfigurování poskytovateli bez vizuálního vzhledu najdete v části Vytvoření šablony e-mailu pomocí jako standardní stránky ASPX. Zjistěte, jaké informace jsou k dispozici ve třídě MailEventNotificaitonInfo, který je předán podle TemplatedEmailWebEventProvider do stránky ASPX šablony. Podívejte se, jak je možné zahrnout libovolné informace je vhodné pro obsah e-mailu. Nakonec zobrazte test webu, který odešle e-mailů v reakci na událostech monitorování stavu. Zobrazte skutečné e-mailech přijatých, které obsahují informace o události na základě šablony monitorování stavu.

[&#9654;Podívejte se na video (25 minut)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet)
