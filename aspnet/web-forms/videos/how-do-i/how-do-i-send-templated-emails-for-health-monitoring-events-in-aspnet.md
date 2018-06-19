---
uid: web-forms/videos/how-do-i/how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet
title: '[Jak na:] Odesílání podle šablony e-mailů pro sledování událostí v technologii ASP.NET stavu | Microsoft Docs'
author: rick-anderson
description: V tomto videu Jan PEL ukazuje způsob použití TemplatedEmailWebEventProvider pro odeslání e-mailů, když dojde k události monitorování stavu, které využívají šablonu pro t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2008
ms.topic: article
ms.assetid: 5c107c6e-9fb7-4206-bd3f-221cb0767f8a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet
msc.type: video
ms.openlocfilehash: 80852c52c453bf353173dbdff79de185a9b7420b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26570841"
---
<a name="how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet"></a><span data-ttu-id="afccb-103">[Jak na:] Odesílání podle šablony e-mailů pro sledování událostí v technologii ASP.NET stavu</span><span class="sxs-lookup"><span data-stu-id="afccb-103">[How Do I:] Send Templated Emails for Health Monitoring Events in ASP.NET</span></span>
====================
<span data-ttu-id="afccb-104">podle [PEL Jan](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="afccb-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="afccb-105">V tomto videu Jan PEL ukazuje, jak používat TemplatedEmailWebEventProvider k odesílání e-mailů, když dojde k události monitorování stavu, které využívají šablonu pro obsah e-mailu.</span><span class="sxs-lookup"><span data-stu-id="afccb-105">In this video Chris Pels shows how to use the TemplatedEmailWebEventProvider to send emails when health monitoring events occur that utilize a template for the email content.</span></span> <span data-ttu-id="afccb-106">Nejdříve si projděte postup konfigurace &lt;zprostředkovatele&gt; a &lt;pravidla&gt; elementů v souboru web.config informace o použití podle šablony e-mailu a přidružit sledování událostí s použitím šablon e-mailu poskytovatelem stavu.</span><span class="sxs-lookup"><span data-stu-id="afccb-106">First, see how to configure the &lt;provider&gt; and &lt;rules&gt; elements in the web.config file to implement the use of templated email and associate a health monitoring event with the templated email provider.</span></span> <span data-ttu-id="afccb-107">Jakmile je nakonfigurovaný podle šablony zprostředkovatele naleznete v části Vytvoření šablony e-mailu pomocí jako standardní stránky .aspx.</span><span class="sxs-lookup"><span data-stu-id="afccb-107">Once the templated provider is configured see how to create the email template using as standard .aspx page.</span></span> <span data-ttu-id="afccb-108">Zjistěte, jaké informace jsou k dispozici ve třídě MailEventNotificaitonInfo, je předaná TemplatedEmailWebEventProvider na stránku šablony .aspx.</span><span class="sxs-lookup"><span data-stu-id="afccb-108">Learn what information is available in the MailEventNotificaitonInfo class that is passed by the TemplatedEmailWebEventProvider to the template .aspx page.</span></span> <span data-ttu-id="afccb-109">V tématu jak ji můžete použít k obsahovat jakékoli informace je vhodné v obsahu e-mailů.</span><span class="sxs-lookup"><span data-stu-id="afccb-109">See how it can be used to include whatever information is appropriate in the email content.</span></span> <span data-ttu-id="afccb-110">Nakonec zobrazte webu test, který odesílá e-mailů v reakci na události sledování stavu.</span><span class="sxs-lookup"><span data-stu-id="afccb-110">Finally, view the test web site which sends emails in response to health monitoring events.</span></span> <span data-ttu-id="afccb-111">Zobrazte skutečné e-maily přijaté, které obsahují informace o události na základě šablony monitorování stavu.</span><span class="sxs-lookup"><span data-stu-id="afccb-111">Then view the actual emails received that contain the health monitoring event information based upon the template.</span></span>

[<span data-ttu-id="afccb-112">&#9654; Podívejte se na video (25 minuty)</span><span class="sxs-lookup"><span data-stu-id="afccb-112">&#9654; Watch video (25 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-send-templated-emails-for-health-monitoring-events-in-aspnet)
