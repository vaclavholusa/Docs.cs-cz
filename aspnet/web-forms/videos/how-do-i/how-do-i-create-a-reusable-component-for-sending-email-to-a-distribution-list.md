---
uid: web-forms/videos/how-do-i/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list
title: '[Jak na:] Vytvořte znovu použitelné komponentní pro odesílání e-mailu distribuční seznam | Microsoft Docs'
author: rick-anderson
description: V této video PEL Jan ukazuje, jak vytvořit komponenty, který lze použít u více webových stránek a webových serverů, která odešle seznam příjemců e-mailů. Firs...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/04/2008
ms.topic: article
ms.assetid: 13dd3a26-c210-432e-91fe-355c979060b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list
msc.type: video
ms.openlocfilehash: 06a2dd7bbcd3087d8d2566f1015eccb8bc7ec8aa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26571954"
---
<a name="how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list"></a><span data-ttu-id="c74b0-104">[Jak na:] Vytvořte znovu použitelné komponentní pro odesílání e-mailu distribuční seznam</span><span class="sxs-lookup"><span data-stu-id="c74b0-104">[How Do I:] Create a Reusable Component for Sending Email to a Distribution List</span></span>
====================
<span data-ttu-id="c74b0-105">podle [PEL Jan](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="c74b0-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="c74b0-106">V této video PEL Jan ukazuje, jak vytvořit komponenty, který lze použít u více webových stránek a webových serverů, která odešle seznam příjemců e-mailů.</span><span class="sxs-lookup"><span data-stu-id="c74b0-106">In this video Chris Pels shows how to create a component that can be used on multiple web pages and web sites that sends emails to a list of recipients.</span></span> <span data-ttu-id="c74b0-107">Nejprve webu ASP.NET nakonfigurujete k odesílání e-mailům prostřednictvím &lt;mailSettings –&gt; v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="c74b0-107">First, the ASP.NET web site will be configured to send email using the &lt;mailSettings&gt; in the web.config file.</span></span> <span data-ttu-id="c74b0-108">Číst seznam příjemců ze zdroje dat (DB, XML atd.) a e-mailovou zprávu poslat každého z příjemců pomocí třídy System.Net.Mail se potom vytvoří třídu a několik metod.</span><span class="sxs-lookup"><span data-stu-id="c74b0-108">Then a class and several methods are created to read a list of recipients from a data source (DB, XML, etc.) and send an email message to each of the recipients using the System.Net.Mail classes.</span></span> <span data-ttu-id="c74b0-109">V rámci této výjimky proces je zahrnuta zpracování.</span><span class="sxs-lookup"><span data-stu-id="c74b0-109">As part of this process exception handling is included.</span></span> <span data-ttu-id="c74b0-110">Kromě toho není vytvořená uživatelské rozhraní, která umožní uživateli zadat položky jako je například adresa odesílatele subjektu, přidejte přílohy atd.</span><span class="sxs-lookup"><span data-stu-id="c74b0-110">In addition, a user interface is created to allow the user to specify items such as the From address, Subject, add an attachment, etc.</span></span>

[<span data-ttu-id="c74b0-111">&#9654; Podívejte se na video (35 minut)</span><span class="sxs-lookup"><span data-stu-id="c74b0-111">&#9654; Watch video (35 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-reusable-component-for-sending-email-to-a-distribution-list)
