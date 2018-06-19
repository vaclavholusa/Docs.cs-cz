---
uid: web-forms/videos/how-do-i/how-do-i-send-email-asynchronously-with-aspnet
title: '[Jak na:] Odesílání e-mailů asynchronně s technologií ASP.NET | Microsoft Docs'
author: rick-anderson
description: V tomto videu Jan PEL ukazuje způsob použití třídy System.Net.Mail technologie ASP.NET k odeslání asynchronní e-mailové zprávy. Nejdříve si projděte postup konfigurace webového serveru...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/24/2008
ms.topic: article
ms.assetid: 77a5c8fa-ebb2-426d-b56b-a5a98a46b516
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-send-email-asynchronously-with-aspnet
msc.type: video
ms.openlocfilehash: a9e35a8fe3a6918da712e5f12c75937ef7f6e76d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26572065"
---
<a name="how-do-i-send-email-asynchronously-with-aspnet"></a><span data-ttu-id="b1a56-104">[Jak na:] Odesílání e-mailů asynchronně s technologií ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b1a56-104">[How Do I:] Send Email Asynchronously with ASP.NET</span></span>
====================
<span data-ttu-id="b1a56-105">podle [PEL Jan](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="b1a56-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="b1a56-106">V tomto videu Jan PEL ukazuje způsob použití třídy System.Net.Mail technologie ASP.NET k odeslání asynchronní e-mailové zprávy.</span><span class="sxs-lookup"><span data-stu-id="b1a56-106">In this video, Chris Pels shows how to use the System.Net.Mail classes in ASP.NET to send an asynchronous email message.</span></span> <span data-ttu-id="b1a56-107">Nejdříve si projděte postup konfigurace webu k odesílání e-mailům prostřednictvím &lt;mailSettings –&gt; element v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="b1a56-107">First, see how to configure a web site to send email using the &lt;mailSettings&gt; element in the web.config file.</span></span> <span data-ttu-id="b1a56-108">Dále vytvořte jednoduché uživatelské rozhraní pro zadávání informací o e-mailu.</span><span class="sxs-lookup"><span data-stu-id="b1a56-108">Next, create a simple user interface for entering email information.</span></span> <span data-ttu-id="b1a56-109">Naučte se vytvořit třídu poštovní zpráva použít k vytvoření e-mailovou zprávu v kódu stránky.</span><span class="sxs-lookup"><span data-stu-id="b1a56-109">Then learn how to create use the MailMessage class to create an email message in the code behind for the page.</span></span> <span data-ttu-id="b1a56-110">Jako součást tohoto procesu vytvoření obslužné rutiny události pro asynchronní zpětné následující odesílání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="b1a56-110">As part of that process create an event handler for the asynchronous callback following the sending of the email.</span></span> <span data-ttu-id="b1a56-111">Obslužná rutina události zjistit, jak použít instanci třídy AsynchCompletedEventArgs, který obsahuje informace o procesu odesílání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="b1a56-111">In the event handler see how to use the instance of the AsynchCompletedEventArgs class which provides information about the email sending process.</span></span> <span data-ttu-id="b1a56-112">Nakonec odeslat testovací e-mail asynchronně, proveďte kroky v režimu ladění a zobrazit skutečné e-maily přijaté z procesu.</span><span class="sxs-lookup"><span data-stu-id="b1a56-112">Finally, send a test email asynchronously, following the steps in the debug mode, and view the actual email received from the process.</span></span>

[<span data-ttu-id="b1a56-113">&#9654; Podívejte se na video (18 minuty)</span><span class="sxs-lookup"><span data-stu-id="b1a56-113">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-send-email-asynchronously-with-aspnet)
